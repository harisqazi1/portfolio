# GitBook to Docusaurus Migration

## Introduction

I have been using Gitbook for some time now. I have used this as my primary mode of blogging. Recently, I realized that I do not have a backup in case GitBook were to ever shutdown or be disabled for a while. GitBook does backup the source code to GitHub, if you configure it as such. Since I did that, I then manipulated the files to work for Docusaurus as well. This blog is about the setup that works for me, and what I did to get it up and running. My recommendation is to have GitBook synced with one platform (GitHub or GitLab), and then have the backup on the other platform. This will make you have a backup for the code, as well as the platform displaying the content. Here is the end result: [https://portfolio-b0g.pages.dev/](https://portfolio-b0g.pages.dev/). I will update this document as I update the code/config.

## Commands

Although this can be run as a script. I **do not recommend doing so**, since one misstep can potentially make things go south really quickly. A good check to make sure you are on the right track is using npm (or another package manager) to visually see the changes being made. Understanding of Markdown is required since that is what both GitBook and Docusaurus use.&#x20;

### Python2 (use sudo):

```bash
#!/bin/bash
#Prereq: npm and rename
#RUN IN DIRECTORY WHERE BOTH FOLDERS ARE NEXT TO EACH OTHER
echo "Gitbook -> Docusaurus"
git clone https://github.com/harisqazi1/portfolio.git
npm install -g n
n latest
# npx create-docusaurus@latest my-website classic
echo y | npx create-docusaurus@latest my-website classic
# clean-up
# cd my-website
# npm run start
# copy all pics from Gitbook to your docusaurus directory <replace test with your directory name>
cp -r portfolio/* my-website/docs/
cp portfolio/.gitbook/assets/* my-website/static/img/
# See: https://docusaurus.io/docs/markdown-features/admonitions
# img error - find all markdown files and replace
find my-website/ -type f -name "*.md" -exec sed -i 's/<figcaption>/<\/img><figcaption>/g' {} +
#info
find my-website/ -type f -name "*.md" -exec sed -i 's/{% raw %}
{% hint style="info" %}/:::info/g' {} +
#danger
find my-website/ -type f -name "*.md" -exec sed -i 's/{% hint style="danger" %}/:::danger/g' {} +
#warning
find my-website/ -type f -name "*.md" -exec sed -i 's/{% hint style="warning" %}/:::caution/g' {} +
#note
find my-website/ -type f -name "*.md" -exec sed -i 's/{% hint style="note" %}/:::note/g' {} +
#tip
find my-website/ -type f -name "*.md" -exec sed -i 's/{% hint style="tip" %}/:::tip/g' {} +
# all hint end tag
find my-website/ -type f -name "*.md" -exec sed -i 's/{% endhint %}
{% endraw %}/:::/g' {} + 
#folder in folder image (most to least for sed coverage - 5 levels
find my-website/ -type f -name "*.md" -exec sed -i 's/..\/..\/..\/..\/.gitbook\/assets/\/img/g' {} +
find my-website/ -type f -name "*.md" -exec sed -i 's/..\/..\/..\/.gitbook\/assets/\/img/g' {} +
find my-website/ -type f -name "*.md" -exec sed -i 's/..\/..\/.gitbook\/assets/\/img/g' {} +
find my-website/ -type f -name "*.md" -exec sed -i 's/..\/.gitbook\/assets/\/img/g' {} +
find my-website/ -type f -name "*.md" -exec sed -i 's/.gitbook\/assets/\/img/g' {} +

rename 's/\s//g' my-website/static/img/*

echo "Screenshot link in file fix"
string=""
grep -r -oh -E -I "Screenshot\s(.*?)\.png" my-website/docs/ | while read -r line ; do
    echo "Processing $line" #remove this 
    string=$line
    string=$(sed 's/\s//g' <<< $string)
    find my-website/docs/ -type f -name "*.md"  -exec sed -i "s/$line/$string/g" {} \;
done

echo "fixing image lins with spaces in it"
grep -r -oh -E -I "image(.*?)\)\.png" my-website/docs/ | while read -r line ; do
    echo "Processing $line" #remove this 
    string=$line
    string=$(sed 's/\s//g' <<< $string)
    echo "line -->  $line"
    echo "string -->$string"
    find my-website/docs/ -type f -name "*.md"  -exec sed -i "s/$line/$string/g" {} \;
done

echo "decoding percent encodings and removing spaces"
urldecode() { #change to your version of python 2
  python2.7 -c "import sys, urllib as ul;print ul.unquote_plus(sys.argv[1])" "$1"
}
string=""
grep -r -oh -E -I "image%(.*?)\.png" my-website/docs/ | while read -r line ; do
    echo "Processing $line" #remove this 
    string=$line
    string=$(urldecode $line)
    string=$(sed 's/\s//g' <<< $string)
    echo "line -->  $line"
    echo "string -->$string"
    find my-website/docs/ -type f -name "*.md"  -exec sed -i "s/$line/$string/g" {} \;
done

echo "inkpasted with url decode and space remove"
urldecode() { #change to your version of python 2
  python2.7 -c "import sys, urllib as ul;print ul.unquote_plus(sys.argv[1])" "$1"
}
string=""
grep -r -oh -E -I "inkedpasted(.*).jpg" my-website/docs/ | while read -r line ; do
    echo "Processing $line" #remove this 
    string=$line
    string=$(urldecode $line)
    string=$(sed 's/\s//g' <<< $string)
    echo "line -->  $line"
    echo "string -->$string"
    find my-website/docs/ -type f -name "*.md"  -exec sed -i "s/$line/$string/g" {} \;
done

find my-website/docs/media-library.md -type f -name "*.md" -exec sed -i 's/<br>/<br \/>/g' {} +
rm static/img/img_4573\(1\).jpg
rm static/img/img_4573.jpg

rm my-website/src/pages/index.js
rm -r my-website/docs/write-ups-1 #error made by Gitbook
rm -r my-website/docs/tutorial-*
rm -r my-website/docs/intro.md

#using Heredocs in bash to write to docusaurus config file

cat << EOF > my-website/docusaurus.config.js
// @ts-check
// Note: type annotations allow type checking and IDEs autocompletion

const lightCodeTheme = require('prism-react-renderer/themes/github');
const darkCodeTheme = require('prism-react-renderer/themes/dracula');

/** @type {import('@docusaurus/types').Config} */
const config = {
  title: 'Portfolio',
  tagline: 'Dinosaurs are cool',
  favicon: 'img/favicon.ico',

  // Set the production url of your site here
  // url: 'https://your-docusaurus-test-site.com',
  url: 'https://your-docusaurus-test-site.com',
  // Set the /<baseUrl>/ pathname under which your site is served
  // For GitHub pages deployment, it is often '/<projectName>/'
  baseUrl: '/',

  // GitHub pages deployment config.
  // If you aren't using GitHub pages, you don't need these.
  organizationName: 'facebook', // Usually your GitHub org/user name.
  projectName: 'docusaurus', // Usually your repo name.

  onBrokenLinks: 'throw',
  onBrokenMarkdownLinks: 'warn',

  // Even if you don't use internalization, you can use this field to set useful
  // metadata like html lang. For example, if your site is Chinese, you may want
  // to replace "en" with "zh-Hans".
  i18n: {
    defaultLocale: 'en',
    locales: ['en'],
  },

  presets: [
    [
      'classic',
      /** @type {import('@docusaurus/preset-classic').Options} */
      ({
        docs: {
          // https://docusaurus.io/docs/docs-introduction
          routeBasePath: '/', // Serve the docs at the site's root
          sidebarPath: require.resolve('./sidebars.js'),
          // Please change this to your repo.
          // Remove this to remove the "edit this page" links.
          editUrl:
            'https://github.com/facebook/docusaurus/tree/main/packages/create-docusaurus/templates/shared/',
        },
        blog: false,
         /* blog: {
          //blog: false, // Optional: disable the blog plugin
          showReadingTime: true,
          // Please change this to your repo.
          // Remove this to remove the "edit this page" links.
          editUrl:
            'https://github.com/facebook/docusaurus/tree/main/packages/create-docusaurus/templates/shared/',
        },  */
        theme: {
          customCss: require.resolve('./src/css/custom.css'),
        },
      }),
    ],
  ],

  themeConfig:
    /** @type {import('@docusaurus/preset-classic').ThemeConfig} */
    ({
      // Replace with your project's social card
      image: 'img/docusaurus-social-card.jpg',
      navbar: {
        title: 'Portfolio',
        logo: {
          alt: 'My Site Logo',
          src: 'img/logo.svg',
        },
        items: [
          /* {
            type: 'docSidebar',
            sidebarId: 'tutorialSidebar',
            position: 'left',
            label: 'Tutorial',
          }, */
          // {to: '/blog', label: 'Blog', position: 'left'},
          {
            href: 'https://github.com/harisqazi1/portfolio',
            label: 'GitHub',
            position: 'right',
          },
        ],
      },
      footer: {
        copyright: \`Copyright © \${new Date().getFullYear()} Haris Qazi. Built with Docusaurus.\`,
      },
      prism: {
        theme: lightCodeTheme,
        darkTheme: darkCodeTheme,
      },
    }),
};

module.exports = config;
EOF

#exclude the following if you do not want to see the output
cd my-website
npm run start

#change name on top left 
# add logo? or emoji even
#edit docusaurus config
# removed footer from docusaurs config OR customize it for yourself 
# removed blog page from config
# change title in config.js as well
# comment out // {to: '/blog', label: 'Blog', position: 'left'},
# delete or rename the /src/pages/index.js file
# edit title to Portfolio and tagline in config.js
# comment out tutorial in config.js as well
```

### Python3 (use sudo):

```bash
#!/bin/bash
#Prereq: npm and rename
#RUN IN DIRECTORY WHERE BOTH FOLDERS ARE NEXT TO EACH OTHER
echo "Gitbook -> Docusaurus"
git clone https://github.com/harisqazi1/portfolio.git
npm install -g n
n latest
# npx create-docusaurus@latest my-website classic
echo y | npx create-docusaurus@latest my-website classic
# clean-up
# cd my-website
# npm run start
# copy all pics from Gitbook to your docusaurus directory <replace test with your directory name>
cp -r portfolio/* my-website/docs/
cp portfolio/.gitbook/assets/* my-website/static/img/
# See: https://docusaurus.io/docs/markdown-features/admonitions
# img error - find all markdown files and replace
find my-website/ -type f -name "*.md" -exec sed -i 's/<figcaption>/<\/img><figcaption>/g' {} +
#info
find my-website/ -type f -name "*.md" -exec sed -i 's/{% raw %}
{% hint style="info" %}/:::info/g' {} +
#danger
find my-website/ -type f -name "*.md" -exec sed -i 's/{% hint style="danger" %}/:::danger/g' {} +
#warning
find my-website/ -type f -name "*.md" -exec sed -i 's/{% hint style="warning" %}/:::caution/g' {} +
#note
find my-website/ -type f -name "*.md" -exec sed -i 's/{% hint style="note" %}/:::note/g' {} +
#tip
find my-website/ -type f -name "*.md" -exec sed -i 's/{% hint style="tip" %}/:::tip/g' {} +
# all hint end tag
find my-website/ -type f -name "*.md" -exec sed -i 's/{% endhint %}
{% endraw %}/:::/g' {} + 
#folder in folder image (most to least for sed coverage - 5 levels
find my-website/ -type f -name "*.md" -exec sed -i 's/..\/..\/..\/..\/.gitbook\/assets/\/img/g' {} +
find my-website/ -type f -name "*.md" -exec sed -i 's/..\/..\/..\/.gitbook\/assets/\/img/g' {} +
find my-website/ -type f -name "*.md" -exec sed -i 's/..\/..\/.gitbook\/assets/\/img/g' {} +
find my-website/ -type f -name "*.md" -exec sed -i 's/..\/.gitbook\/assets/\/img/g' {} +
find my-website/ -type f -name "*.md" -exec sed -i 's/.gitbook\/assets/\/img/g' {} +

rename 's/\s//g' my-website/static/img/*

echo "Screenshot link in file fix"
string=""
grep -r -oh -E -I "Screenshot\s(.*?)\.png" my-website/docs/ | while read -r line ; do
    echo "Processing $line" #remove this 
    string=$line
    string=$(sed 's/\s//g' <<< $string)
    find my-website/docs/ -type f -name "*.md"  -exec sed -i "s/$line/$string/g" {} \;
done

echo "fixing image lins with spaces in it"
grep -r -oh -E -I "image(.*?)\)\.png" my-website/docs/ | while read -r line ; do
    echo "Processing $line" #remove this 
    string=$line
    string=$(sed 's/\s//g' <<< $string)
    echo "line -->  $line"
    echo "string -->$string"
    find my-website/docs/ -type f -name "*.md"  -exec sed -i "s/$line/$string/g" {} \;
done

echo "decoding percent encodings and removing spaces"
urldecode() { #change to your version of python 2
  python3 -c "import sys, urllib.parse as ul; print(ul.unquote_plus(sys.argv[1]))" "$1"
}
string=""
grep -r -oh -E -I "image%(.*?)\.png" my-website/docs/ | while read -r line ; do
    echo "Processing $line" #remove this 
    string=$line
    string=$(urldecode $line)
    string=$(sed 's/\s//g' <<< $string)
    echo "line -->  $line"
    echo "string -->$string"
    find my-website/docs/ -type f -name "*.md"  -exec sed -i "s/$line/$string/g" {} \;
done

echo "inkpasted with url decode and space remove"
urldecode() { #change to your version of python 2
  python3 -c "import sys, urllib.parse as ul; print(ul.unquote_plus(sys.argv[1]))" "$1"
}
string=""
grep -r -oh -E -I "inkedpasted(.*).jpg" my-website/docs/ | while read -r line ; do
    echo "Processing $line" #remove this 
    string=$line
    string=$(urldecode $line)
    string=$(sed 's/\s//g' <<< $string)
    echo "line -->  $line"
    echo "string -->$string"
    find my-website/docs/ -type f -name "*.md"  -exec sed -i "s/$line/$string/g" {} \;
done

find my-website/docs/media-library.md -type f -name "*.md" -exec sed -i 's/<br>/<br \/>/g' {} +
rm static/img/img_4573\(1\).jpg
rm static/img/img_4573.jpg

rm my-website/src/pages/index.js
rm -r my-website/docs/write-ups-1 #error made by Gitbook
rm -r my-website/docs/tutorial-*
rm -r my-website/docs/intro.md

#using Heredocs in bash to write to docusaurus config file

cat << EOF > my-website/docusaurus.config.js
// @ts-check
// Note: type annotations allow type checking and IDEs autocompletion

const lightCodeTheme = require('prism-react-renderer/themes/github');
const darkCodeTheme = require('prism-react-renderer/themes/dracula');

/** @type {import('@docusaurus/types').Config} */
const config = {
  title: 'Portfolio',
  tagline: 'Dinosaurs are cool',
  favicon: 'img/favicon.ico',

  // Set the production url of your site here
  // url: 'https://your-docusaurus-test-site.com',
  url: 'https://your-docusaurus-test-site.com',
  // Set the /<baseUrl>/ pathname under which your site is served
  // For GitHub pages deployment, it is often '/<projectName>/'
  baseUrl: '/',

  // GitHub pages deployment config.
  // If you aren't using GitHub pages, you don't need these.
  organizationName: 'facebook', // Usually your GitHub org/user name.
  projectName: 'docusaurus', // Usually your repo name.

  onBrokenLinks: 'throw',
  onBrokenMarkdownLinks: 'warn',

  // Even if you don't use internalization, you can use this field to set useful
  // metadata like html lang. For example, if your site is Chinese, you may want
  // to replace "en" with "zh-Hans".
  i18n: {
    defaultLocale: 'en',
    locales: ['en'],
  },

  presets: [
    [
      'classic',
      /** @type {import('@docusaurus/preset-classic').Options} */
      ({
        docs: {
          // https://docusaurus.io/docs/docs-introduction
          routeBasePath: '/', // Serve the docs at the site's root
          sidebarPath: require.resolve('./sidebars.js'),
          // Please change this to your repo.
          // Remove this to remove the "edit this page" links.
          editUrl:
            'https://github.com/facebook/docusaurus/tree/main/packages/create-docusaurus/templates/shared/',
        },
        blog: false,
         /* blog: {
          //blog: false, // Optional: disable the blog plugin
          showReadingTime: true,
          // Please change this to your repo.
          // Remove this to remove the "edit this page" links.
          editUrl:
            'https://github.com/facebook/docusaurus/tree/main/packages/create-docusaurus/templates/shared/',
        },  */
        theme: {
          customCss: require.resolve('./src/css/custom.css'),
        },
      }),
    ],
  ],

  themeConfig:
    /** @type {import('@docusaurus/preset-classic').ThemeConfig} */
    ({
      // Replace with your project's social card
      image: 'img/docusaurus-social-card.jpg',
      navbar: {
        title: 'Portfolio',
        logo: {
          alt: 'My Site Logo',
          src: 'img/logo.svg',
        },
        items: [
          /* {
            type: 'docSidebar',
            sidebarId: 'tutorialSidebar',
            position: 'left',
            label: 'Tutorial',
          }, */
          // {to: '/blog', label: 'Blog', position: 'left'},
          {
            href: 'https://github.com/harisqazi1/portfolio',
            label: 'GitHub',
            position: 'right',
          },
        ],
      },
      footer: {
        copyright: \`Copyright © \${new Date().getFullYear()} Haris Qazi. Built with Docusaurus.\`,
      },
      prism: {
        theme: lightCodeTheme,
        darkTheme: darkCodeTheme,
      },
    }),
};

module.exports = config;
EOF

#exclude the following if you do not want to see the output
cd my-website
npm run start


#change name on top left 
# add logo? or emoji even
#edit docusaurus config
# removed footer from docusaurs config OR customize it for yourself 
# removed blog page from config
# change title in config.js as well
# comment out // {to: '/blog', label: 'Blog', position: 'left'},
# delete or rename the /src/pages/index.js file
# edit title to Portfolio and tagline in config.js
# comment out tutorial in config.js as well
```

## Docusaurus.config.js

```javascript
// @ts-check
// Note: type annotations allow type checking and IDEs autocompletion

const lightCodeTheme = require('prism-react-renderer/themes/github');
const darkCodeTheme = require('prism-react-renderer/themes/dracula');

/** @type {import('@docusaurus/types').Config} */
const config = {
  title: 'Portfolio',
  tagline: 'Dinosaurs are cool',
  favicon: 'img/favicon.ico',

  // Set the production url of your site here
  // url: 'https://your-docusaurus-test-site.com',
  url: 'https://your-docusaurus-test-site.com',
  // Set the /<baseUrl>/ pathname under which your site is served
  // For GitHub pages deployment, it is often '/<projectName>/'
  baseUrl: '/',

  // GitHub pages deployment config.
  // If you aren't using GitHub pages, you don't need these.
  organizationName: 'facebook', // Usually your GitHub org/user name.
  projectName: 'docusaurus', // Usually your repo name.

  onBrokenLinks: 'throw',
  onBrokenMarkdownLinks: 'warn',

  // Even if you don't use internalization, you can use this field to set useful
  // metadata like html lang. For example, if your site is Chinese, you may want
  // to replace "en" with "zh-Hans".
  i18n: {
    defaultLocale: 'en',
    locales: ['en'],
  },

  presets: [
    [
      'classic',
      /** @type {import('@docusaurus/preset-classic').Options} */
      ({
        docs: {
          // https://docusaurus.io/docs/docs-introduction
          routeBasePath: '/', // Serve the docs at the site's root
          sidebarPath: require.resolve('./sidebars.js'),
          // Please change this to your repo.
          // Remove this to remove the "edit this page" links.
          editUrl:
            'https://github.com/facebook/docusaurus/tree/main/packages/create-docusaurus/templates/shared/',
        },
        blog: false,
         /* blog: {
          //blog: false, // Optional: disable the blog plugin
          showReadingTime: true,
          // Please change this to your repo.
          // Remove this to remove the "edit this page" links.
          editUrl:
            'https://github.com/facebook/docusaurus/tree/main/packages/create-docusaurus/templates/shared/',
        },  */
        theme: {
          customCss: require.resolve('./src/css/custom.css'),
        },
      }),
    ],
  ],

  themeConfig:
    /** @type {import('@docusaurus/preset-classic').ThemeConfig} */
    ({
      // Replace with your project's social card
      image: 'img/docusaurus-social-card.jpg',
      navbar: {
        title: 'Portfolio',
        logo: {
          alt: 'My Site Logo',
          src: 'img/logo.svg',
        },
        items: [
          /* {
            type: 'docSidebar',
            sidebarId: 'tutorialSidebar',
            position: 'left',
            label: 'Tutorial',
          }, */
          // {to: '/blog', label: 'Blog', position: 'left'},
          {
            href: 'https://github.com/harisqazi1/portfolio',
            label: 'GitHub',
            position: 'right',
          },
        ],
      },
      footer: {
        copyright: `Copyright © ${new Date().getFullYear()} Haris Qazi. Built with Docusaurus.`,
      },
      prism: {
        theme: lightCodeTheme,
        darkTheme: darkCodeTheme,
      },
    }),
};

module.exports = config;
```

## Post Set Up Checklist

* [ ] Update logo on top right by downloading an image of logo, and then using the relative/absolute path to reference in the config file
* [ ] Move to website deployment platform (Cloudflare Pages, GitHub Pages, etc.)
  * [ ] Update domain to custom domain
  * [ ] Modify config and script based on issues while deployment

## Next Updates

* [https://docusaurus.io/docs/sidebar/items#embedding-generated-index-in-doc-page](https://docusaurus.io/docs/sidebar/items#embedding-generated-index-in-doc-page)

## Alternatives (in no particular order)

* [docsify](https://docsify.js.org/#/)
* [mdBook](https://rust-lang.github.io/mdBook/)
* [VuePress](https://vuepress.vuejs.org/)
