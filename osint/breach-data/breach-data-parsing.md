# Breach Data Indexing

## Background

I was participating in a TraceLabs event, and was using [ripgrep](https://github.com/BurntSushi/ripgrep) to search the data in a breach compilation. ripgrep is much faster than grep, however the search took me 50 minutes to complete. For events like TraceLabs, 50 minutes is not time you have to waste, especially when it is being done for a good cause. Talking with AccessOSINT, me and him discussed how Michael Bazzell has mentioned having really quick searches through breach and stealer log data in his podcast. This got me motivated to actually look into solutions for how to search through breach data much faster.

## Search Engines

Whenever I think about Search Engines, the first idea that came to my mind was Google or DuckDuckGo. These services search an index of URLs and provide answers based on your search query. I ended up using search engine services myself to parse through breach data. These search engines index all of your data, so you can search through it much faster. However, before using a search engine, you have to format the data to be digestible by the search engine. I will use two search engines in this blog: Meilisearch and Elasticsearch (more importantly, the ELK stack).

## Meilisearch

[Meilisearch](https://www.meilisearch.com/) is a search engine tool that can be hosted locally to parse though data you put into it. Read the [known limitations](https://docs.meilisearch.com/learn/advanced/known\_limitations.html) before you go with this solution. Here are the commands and steps I used to get it up and running:

### Setup

```bash
# Download and Install
curl -L https://install.meilisearch.com | sh
```

```bash
# Run
./meilisearch --no-analytics
# --no-analytics to remove analystics being sent
# https://docs.meilisearch.com/learn/what_is_meilisearch/telemetry.html#how-to-disable-data-collection
```

The first time you run the code, you will get a master key. Save this somewhere as you will need it later.

<figure><img src="../../.gitbook/assets/image (3) (2) (3) (2).png" alt=""><figcaption></figcaption></figure>

This is the command I ended up using for set up:

```bash
# Command
./meilisearch --no-analytics --master-key="<enter key here>"  --http-payload-size-limit '10Gb'
# --no-analytics to remove analyics
# --master-key="<enter key here>" needed to run the code
# --http-payload-size-limit '10Gb' to allow you to add large documents
```

<figure><img src="../../.gitbook/assets/image (1) (6).png" alt=""><figcaption></figcaption></figure>

At this point, you should be able to access the web interface:

<figure><img src="../../.gitbook/assets/image (7) (2).png" alt=""><figcaption></figcaption></figure>

Here you enter your master key you had gotten earlier. You have now set up Meilisearch.

### Creating an Index and adding Data

Indexes are basically the main group that the data falls under. Meilisearch uses movies as an example. Under the index, you then have all of your data. Meilisearch accepts the following formats of data: JSON, NDJSON, CSV. They all have their own syntax for how to send it into Meilisearch.

I will be creating an index called "breach". You can have multiple indexes for each breach, but I feel like one location is easier in the long run to deal with.

```bash
# Index Creation
curl \
  -X POST 'http://localhost:7700/indexes' \
  -H 'Content-Type: application/json' \
  --data-binary '{
    "uid": "breach",
    "primaryKey": "id"
  }' \
  -H 'Authorization: Bearer <add master key here>'
```

You should see the following output:

<figure><img src="../../.gitbook/assets/image (10) (1) (3).png" alt=""><figcaption></figcaption></figure>

Visiting the web portal, you should see the following:

<figure><img src="../../.gitbook/assets/image (3) (5).png" alt=""><figcaption></figcaption></figure>

We have now created the index, but have no data in the index.

### Data Format

Not every breach will have the same fields. In order to make them the same you will have to format it. Linux makes that easy with tools such as `awk`, `sed`, etc. I like to write my own scripts so I can customize it to my usage.

{% hint style="warning" %}
In Meilisearch, "The primary field is a special field that must be present in all documents. Its attribute is the [**primary key**](https://docs.meilisearch.com/learn/core\_concepts/primary\_key.html#primary-key-2) and its value is the [**document id**](https://docs.meilisearch.com/learn/core\_concepts/primary\_key.html#document-id). It uniquely identifies each document in an index, ensuring that **it is impossible to have two exactly identical documents** present in the same index".
{% endhint %}

Since we will have one index only, we have to have each primary field value be unique. An easy way to do this is to add a new column that has a number or unique Identifier. We can go from 1-X with X being the id of the last line in the breach index. Another way to do this, is to add a random string before each line:

```bash
# Source: https://unix.stackexchange.com/questions/428737/how-can-i-add-random-string-for-each-line
awk '{
     str_generator = "tr -dc '[:alnum:]' </dev/urandom | head -c 6"
     str_generator | getline random_str
     close(str_generator)
     print "name " random_str " - " $0
}' file
```

This in my opinion adds way more overhead to a file than going from 1-X does. In addition, you can never be sure that there will not be a collision between two random strings in the long run.

I ended up going with something like this, but modified for each use case:

```bash
#!/bin/bash
file=$1
output_file=${file}-clean.txt
starting_number=0 #12 digits should be able to store 1 trillion records;
# you could also make a custom aphabet (upper, lower, numbers) to save space.
while IFS='' read -r LINE || [ -n "${LINE}" ]; do
    echo "${starting_number}:${LINE}" >> $output_file #output each line with the unique id at the beginning
    ((starting_number+=1)) #append variable by 1
done < "${file}"
echo "final number ${starting_number}"
sed -i 's/:/,/g' $output_file #replace all colons with commas
sed -i '1s/^/id,email,hash\n/' $output_file #add csv headers to the first line
mv $output_file "${output_file}".csv #change file extension to .csv
```

{% hint style="info" %}
Even if the fields are different in each file. As long as there is a unique id, it should work.
{% endhint %}

To add data, use the `POST` HTTP request. An example would look like the following:

```bash
# POST request for adding data
curl \
  -X POST 'http://localhost:7700/indexes/breach/documents?primaryKey=id' \
  -H 'Content-Type: text/csv' \
  -T /home/<file_location> \
  -H 'Authorization: Bearer <add master key or auth token here>'
```

`-T` can be replaced by `--data-binary`. For big files (1Gb<), `-T` ended up working for me, so I stuck with it.

### Viewing Tasks

```bash
# Tasks
curl \
  -X GET 'http://localhost:7700/tasks' \
  -H 'Authorization: Bearer <add master key or auth token here>'
```

### Deleting Data

```bash
# Data Deletion
curl \
  -X DELETE 'http://localhost:7700/indexes/breach/documents' \
  -H 'Authorization: Bearer <add master key or auth token here>'
```

## ELK Stack

[From the website](https://www.elastic.co/what-is/elk-stack), "Elasticsearch is a search and analytics engine. Logstash is a server‑side data processing pipeline that ingests data from multiple sources simultaneously, transforms it, and then sends it to a "stash" like Elasticsearch. Kibana lets users visualize data with charts and graphs in Elasticsearch."

### Setup

```bash
# https://www.digitalocean.com/community/tutorials/how-to-install-elasticsearch-logstash-and-kibana-elastic-stack-on-ubuntu-22-04
curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch |sudo gpg --dearmor -o /usr/share/keyrings/elastic.gpg
echo "deb [signed-by=/usr/share/keyrings/elastic.gpg] https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
sudo apt update
# Elasticsearch
sudo apt install elasticsearch
# sed can be used to replace instead of nano to replace
# #network.host: 192.168.1.1 -> network.host: localhost
sudo nano /etc/elasticsearch/elasticsearch.yml
sudo systemctl start elasticsearch
sudo systemctl enable elasticsearch
# To test if Elasticsearch is up
curl -X GET "localhost:9200"
# Kibana
sudo apt install kibana
sudo systemctl enable kibana
sudo systemctl start kibana
# sed can be used to replace instead of nano to replace
sudo nano /etc/kibana/kibana.yml
# Remove the # before the following:
# server.port: 5601
# server.host: "your-hostname"
# elasticsearch.hosts: ["http://localhost:9200"]
sudo systemctl start kibana
sudo systemctl enable kibana
sudo ufw allow 5601/tcp
# To test if Kibana is up
http://localhost:5601
# Logstash
sudo apt-get install logstash
sudo systemctl start logstash
sudo systemctl enable logstash
sudo systemctl status logstash
# Filebeat
sudo apt-get install filebeat
# sed can be used to replace instead of nano to replace
sudo nano /etc/filebeat/filebeat.yml
# Remove the # before one OR the other in following (based on where you want to output):
# output.elasticsearch:
   # Array of hosts to connect to.
   # hosts: ["localhost:9200"]
# output.logstash
     # hosts: ["localhost:5044"]
sudo systemctl start filebeat
sudo systemctl enable filebeat
```

I would recommend using [Postman](https://www.postman.com/), which makes it easier to make HTTP requests with a web interface:

<figure><img src="../../.gitbook/assets/image (13) (1) (3) (1).png" alt=""><figcaption></figcaption></figure>

### Creating an Index and adding Data:

{% hint style="info" %}
Although I will be using Postman, I will be adding the full commands I use below.
{% endhint %}

```bash
// https://linuxhint.com/elasticsearch-create-index/
curl -X PUT "localhost:9200/breach?pretty"
# `pretty` makes the output look better
```

<figure><img src="../../.gitbook/assets/image (5) (2) (1).png" alt=""><figcaption></figcaption></figure>

I then started to clean up a breach I have in a CSV format, so then I can upload it to Elasticsearch. I only had two fields:email and pain-text password. I was able to upload it to Elasticsearch using the following Logstash config:

```json
// breach.conf
input {
    file {
        path => "<full path to breach csv file>"
        start_position => beginning
    }
}
filter {
    csv {
        columns => [
                "Email",
                "Password"
        ]
        separator => ","
        }
}
output {
    stdout
    {
        codec => rubydebug
    }
     elasticsearch {
        action => "index"
        hosts => ["127.0.0.1:9200"]
        index => "breach"
    }
}
```

Run Logstash:

```bash
# Logstash
sudo /usr/share/logstash/bin/logstash -f <conf file location>
# sudo is needed to bypass an error where is was unable to read a file
```

Uploading millions of records took a while. After this, I created an index pattern in Kibana (**Stack Management** -> **Index patterns** -> **Create index pattern**). I chose the name of the index in Elasticsearch I already had made ("breach").

<figure><img src="../../.gitbook/assets/image (12) (1) (1).png" alt=""><figcaption></figcaption></figure>

After the index pattern is created, the following can be seen when the "breach" index pattern is clicked on:

<figure><img src="../../.gitbook/assets/image (3) (5) (1).png" alt=""><figcaption></figcaption></figure>

The data is now visible in **Discover** under **Analytics:**

<figure><img src="../../.gitbook/assets/image (11) (1).png" alt=""><figcaption></figcaption></figure>

The data is now searchable through the web UI. The data is also searchable through postman or command line as well:

<figure><img src="../../.gitbook/assets/image (2) (6).png" alt=""><figcaption></figcaption></figure>

You can go all out with Kibana dashboards with statistics and graphs on emails, passwords, etc. but that is out of the scope of this blog.

## Recommendations

* Name all breaches in the following syntax `breach_<name of breach>`
  * You can then use 1 index for all breaches, but also have them have their own index.
* Clean data to one format
  * This way all the data is organized (I use CSV, with the "," as the delimiter)
* Save backups of the unedited breach
  * There is a chance that your clean-up might have missed a password or another field
  * Also, you can start over from square one in case the data gets corrupted

## Statistics

| grep -i | ripgrep -aFiN | Search Engine         | File Size (Raw) |
| ------- | ------------- | --------------------- | --------------- |
| 0.038s  | 0.028s        | 0.029s (Meilisearch)  | 122 MB          |
| 16.681s | 9.564s        | 983ms (Meilisearch)   | 6.6 GB          |
| 22.303s | 28.367s       | 883ms (Elasticsearch) | 9.4 GB          |

{% hint style="info" %}
The last Elasticsearch data was searched on about 313,900,000 documents. 9.4 GB of raw data ended up being 65.1 GB of indexed data.
{% endhint %}

## Sources

* [https://phoenixnap.com/kb/how-to-install-elk-stack-on-ubuntu](https://phoenixnap.com/kb/how-to-install-elk-stack-on-ubuntu)
* [https://www.digitalocean.com/community/tutorials/how-to-install-elasticsearch-logstash-and-kibana-elastic-stack-on-ubuntu-22-04](https://www.digitalocean.com/community/tutorials/how-to-install-elasticsearch-logstash-and-kibana-elastic-stack-on-ubuntu-22-04)
* [https://linuxhint.com/elasticsearch-create-index/](https://linuxhint.com/elasticsearch-create-index/)
* [https://levelup.gitconnected.com/load-csv-data-into-elasticsearch-fdb562a7abd9](https://levelup.gitconnected.com/load-csv-data-into-elasticsearch-fdb562a7abd9)
