# Ansible

I have never personally used Ansible before. I recently was planning on updating my Linux script for speeding up the process for setting up a Pop\_OS! desktop, when I realized that I can learn about Ansible while trying to set it up for my own usage. This post will be my notes I take while learning about the topic. **My configuration is at:** [**https://github.com/harisqazi1/ansible**](https://github.com/harisqazi1/ansible)**.**

## Notes

Ansible Playbooks are a way to repeat the same configuration to a whole network of hosts. It can also be used for one host as well, which is what I will be doing. The Playbooks are written in the YAML format.

### YAML Cheatsheet

```yaml
--- 
# '---' = start of document
- first item
- second item
- third item

# A dictionary in YAML is similar to that of Python with the `key:value` pairs
# There has to be a space after the colon (:)
employee:
	name: Haris
	hobby: Ansible
	skills: 
		- Python
		- Java
		
# Dictionaries and lists can be abbreviated:
employee: {name: Haris, hobby: Ansible}
fruits: ['Apple', 'Orange', 'Strawberry', 'Mango']

# Booleans are also possible
# Multiple formats: true, TRUE, True
# Use lowercase ‘true’ or ‘false’ for boolean values in dictionaries if you want to be compatible with default yamllint options.

bash: false
zsh: true 

# https://docs.ansible.com/ansible/latest/reference_appendices/YAMLSyntax.html#yaml-syntax
# Values can span multiple lines using | or >. Spanning multiple lines using a 
# “Literal Block Scalar” | will include the newlines and any trailing spaces. 
# Using a “Folded Block Scalar” > will fold newlines to spaces 
# it’s used to make what would otherwise be a very long line easier
# to read and edit. In either case the indentation will be ignored. 

include_newlines: |
            exactly as you see
            will appear these three
            lines of poetry

fold_newlines: >
            this is really a
            single line of text
            despite appearances

#Only in double quotes you can use escapes "\"
foo: "a \t TAB and a \n NEWLINE"

# Ansible uses “{{ var }}” for variables
foo: "{{ variable }}"

# '...' end of document
... 
```

After writing a the YAML portion, I discovered [https://learnxinyminutes.com/docs/yaml/](https://learnxinyminutes.com/docs/yaml/), which describes YAML in a similar format to mine, but is a bit more detailed.

### Ansible

* Ansible automates the management of remote systems and controls their desired state. A basic Ansible environment has three main components:
  * **Control node**: A system on which Ansible is installed. You run Ansible commands such as ansible or ansible-inventory on a control node.
  * **Managed node**: A remote system, or host, that Ansible controls.
  * **Inventory**: A list of managed nodes that are logically organized. You create an inventory on the control node to describe host deployments to Ansible.
* Ansible executes each task in order, against all machines matched by the host pattern
  * Tasks execute modules
  * If a task fails on a host, Ansible takes that host out of the rotation for the rest of the playbook.
* Most Ansible modules are "idempotent" modules, where they check the if the desired result or "final state" has been achieved already so it does not have to repeat the work
* Run the playbook with the following command: `ansible-playbook playbook.yml`
  * add `--verbose` to see detailed output
* `ansible-pull` is a small script that will checkout a repository of configuration instructions from git, and then run `ansible-playbook` against that content.
* Verify playbooks (`ansible-playbook`) with `--check`, `--diff`, `--list-hosts`, `--list-tasks`, and `--syntax-check`
* Use `ansible-lint` for Ansible-specific feedback on your playbooks

### Playbooks

* **Playbook**: A list of plays that define the order in which Ansible performs operations, from top to bottom.
* **Play**: An ordered list of tasks that maps to managed nodes in an inventory. The Play contains variables, roles and an ordered lists of tasks and can be run repeatedly. It basically consists of an implicit loop over the mapped hosts and tasks and defines how to iterate over them.
  * **Roles**: A limited distribution of reusable Ansible content (tasks, handlers, variables, plugins, templates and files) for use inside of a Play. To use any Role resource, the Role itself must be imported into the Play.
  * **Tasks**: The definition of an ‘action’ to be applied to the managed host. Tasks must always be contained in a Play, directly or indirectly (Role, or imported/included task list file).
    * Task names are optional, but extremely useful. In its output, Ansible shows you the name of each task it runs. Choose names that describe what each task does and why.
    * For many modules, the ‘state’ parameter is optional. Different modules have different default settings for ‘state’, and some modules support several ‘state’ settings. Explicitly setting ‘state=present’ or ‘state=absent’ makes playbooks and roles clearer.
  * **Handlers**: A special form of a Task, that only executes when notified by a previous task which resulted in a ‘changed’ status.
* **Task**: A list of one or more modules that defines the operations that Ansible performs.
* **Module**: A unit of code or binary that Ansible runs on managed nodes.

## Custom Configuration (Linux)

There will be multiple files and folders that I will reference here. You can reference [https://github.com/harisqazi1/ansible](https://github.com/harisqazi1/ansible) for the full configuration.&#x20;

### Inventory

In order to start created a custom configuration, we need to have an inventory of assets. For my use, it is only the localhost. This can be done in two ways:

#### Method 1: Create an inventory file with an .ini extension and add the following:

```ini
[all]
127.0.0.1  ansible_connection=local
```

You then reference this file while calling the playbook: `ansible-playbook -i inventory.ini playbook.yml`

#### Method 2: You can add the following lines to the playbook file:

```YAML
hosts: 127.0.0.1
connection: local
```

For method 2, you do not have to use any inventory file as an argument as there is none.

{% hint style="warning" %}
In my testing for localhost, Method 1 is the one that worked for me.
{% endhint %}

### Creating the playbook

I started off with modifying the sample ansible playbook a bit.

```YAML
- name: Ubuntu Configuration
  hosts: all
  tasks:
   - name: Ping my hosts
     ansible.builtin.ping:
   - name: Print message
     ansible.builtin.debug:
       msg: Hello world
```

I then referenced [https://github.com/AlexNabokikh/ubuntu-playbook](https://github.com/AlexNabokikh/ubuntu-playbook) to see how they did their setup, and I started building my own. Eventually, I updated the playbook to include roles, tasks, and even pointed it to my configuration file (snippet below):

```yaml
- name: Ubuntu Configuration
  hosts: all # this is referencing the inventory.ini file
  #pointing it to my config file
  vars_files:
    - config.yml
  #imports from Ansible Galaxy 
  roles:
    - role: petermosmans.customize-gnome
      tags: ["gnome"]
    - role: tomereli.oh_my_zsh_p10k
      tags: ["ohmyzsh"]
  # The tasks that the playbook will be doing
  tasks:
    - name: Install apt packages that have keys
      ansible.builtin.import_tasks:
        file: tasks/apt_signed.yml
      tags: ["apt_signed"]

    - name: Install apt packages
      ansible.builtin.import_tasks:
        file: tasks/apt.yml
      tags: ["apt"]
```

### Tasks

Tasks are basically documents that are built out of Ansible modules. For creating the tasks I needed, I first thought about what I needed to make it happen. For example, in order to download a software, can I download it through apt or flatpak? If not, then I have to look into downloading a file via a URL and then installing is using the shell ansible module. Since a good portion of Ansible is making sure an item is in a desired state, it is up to the user to configure how that will be done. For me, the Ansible documentation was a great place to start. If I ran into issues, I then looked to forums like stackoverflow to figure out where my error was. Here is an example of a task I made for downloading TOR into the downloads folder:

```yaml
---
# Downloads TOR from the TOR website using the get_url module
- name: Download tor
  ansible.builtin.get_url:
    url: https://www.torproject.org/dist/torbrowser/12.0.7/tor-browser-linux64-12.0.7_ALL.tar.xz
    dest: "/home/{{ username }}/Downloads/"
# Unzips the file into the downloads folder using the unarchive module
- name: Unarchive the tor zip
  ansible.builtin.unarchive:
    src: "/home/{{ username }}/Downloads/tor-browser-linux64-12.0.7_ALL.tar.xz"
    dest: "/home/{{ username }}/Downloads/"
    remote_src: yes
```

### Ansible Galaxy:

Ansible Galaxy is what I would describe as a repository of various Ansible configurations for different results. These configurations are made by the community. If you use a configuration from Ansible Galaxy, it would be called a "role" for your playbook. I think the best way to think about these are like imports in code. Similar to imports, you add these in your coding file (YAML for Ansible), and it pulls it down and start applying those configurations to your system. In my testing, some Galaxy configurations did not work, so I would recommend testing it out and seeing if it works, instead of blindly adding it to the playbook.

## Sources:

* https://docs.ansible.com/ansible/latest/playbook\_guide/playbooks\_intro.html
* https://docs.ansible.com/ansible/latest/reference\_appendices/YAMLSyntax.html#yaml-syntax
* https://galaxy.ansible.com/gantsign/oh-my-zsh
* https://github.com/AlexNabokikh/ubuntu-playbook/blob/master/main.yml
* https://github.com/veggiemonk/ansible-ohmyzsh/blob/master/tasks/main.yml
* https://docs.ansible.com/ansible/latest/getting\_started/get\_started\_playbook.html
* https://docs.ansible.com/ansible/latest/playbook\_guide/playbooks\_delegation.html#local-playbooks
* https://www.jeffgeerling.com/blog/2022/aptkey-deprecated-debianubuntu-how-fix-ansible
* https://stackoverflow.com/questions/71585303/how-can-i-manage-keyring-files-in-trusted-gpg-d-with-ansible-playbook-since-apt
* https://stackoverflow.com/questions/71995796/installing-the-deb-package-via-ansible
* https://www.howtouselinux.com/post/ansible-loop-over-items-with-a-pause-between-iterations
* https://stackoverflow.com/a/50923310
* https://github.com/staticdev/ansible-role-brave/blob/main/tasks/main.yml
* https://kiwix.ounapuu.ee/content/askubuntu.com\_en\_all\_2022-11/questions/910821/programs-installed-via-snap-not-showing-up-in-launcher
* https://9to5answer.com/snap-installed-applications-not-showing-on-launcher
* https://askubuntu.com/questions/800685/add-a-snap-icon-to-the-desktop-ubuntu-16-04
* https://raw.githubusercontent.com/staticdev/ansible-role-brave/main/tasks/main.yml
* https://github.com/AlexNabokikh/ubuntu-playbook/blob/master/tasks/extra\_packages.yml
* https://github.com/AlexNabokikh/ubuntu-playbook/blob/master/tasks/hostname.yml
* https://stackoverflow.com/questions/40230184/how-to-do-multiline-shell-script-in-ansible#40230416
* https://stackoverflow.com/questions/47994497/how-to-pipe-commands-using-ansible-e-g-curl-sl-host-com-sudo-bash
* https://serverfault.com/questions/939671/ansible-expect-is-not-sending-response
