## GooglePlay Web Crawler
### What is Hadoop Ecosystem?
![hadoop](https://github.com/ly16/GooglePlay-Web-Crawler/blob/master/results/1232046853.jpg)
- The core compositions of Hadoop are HDFS, Yarn, and other engines and App, like Mapreduce, Tez, Nutch, Pig, Hive, Spark, etc.
- HDFS is composed of NameNode and DataNode for data storage.
- Yarn is composed of Resource Manager and node Manager for resource assignment.
- APPs like Pig, Hive are higher level language processor. They can conduct mapreduce job much easier.

### How does web crawler work?
- Use a customized Nutch to crawl apps metadata in GooglePlay
* Inject seed to nutchDB
* Generate urls to crawl from nutchDB
* Fetch app meatadata from html pages
* parse extracted metadata and outlinks
* update nutchDB with new outlinks
- Command Line
* git clone 
```
git clone https://github.com/apache/nutch
git checkout release-1.12
```
* customize nutch
```
patch -p1 < /googleplaycrawler/googleplaycrawler.patch

```
* 
