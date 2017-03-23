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
* Pig Loadfunc transforms nutchDB to readable text file form
* Create table and manage data by Hive

### Command Line
* git clone 
```
git clone https://github.com/apache/nutch
git checkout release-1.12
```
* customize nutch
```
patch -p1 < /googleplaycrawler/googleplaycrawler.patch
```
* run googleplaycrawler on single nutch cluster
```
echo "https://play.google.com/store/apps/details?id=com.facebook.orca" > seed

hadoop fs -put seed 

hadoop jar build/apache-nutch-1.12.job org.apache.nutch.googleplay.GooglePlayCrawler seed -numFetchers 10

```
* check output

```

hadoop fs -text file:///xxxxxx/nutchdb/segments/xxxxx/parse_data/part-00000/data

```
* fix the skew data job
```
patch -p1 < fixskew.patch
```
* uplode seeds file and run web scrawler in AWS EMR
![emr](https://github.com/ly16/GooglePlay-Web-Crawler/blob/master/results/Screen%20Shot%202017-03-23%20at%2012.39.58%20AM.png?raw=true)

* Aws S3
```
register target/nutchdbloader-0.0.1-SNAPSHOT.jar
register /home/hadoop/hadoop-2.7.3/share/hadoop/tools/lib/hadoop-aws-2.7.3.jar register nutch-1.12.jar
loaded = load 's3n://test/nutchdb/segments/*/parse_data/part-*/data' using com.example.NutchParsedDataLoader();
filtered = filter loaded by $0 is not null;
store filtered into 'output';
```
### text reults
![results](https://github.com/ly16/GooglePlay-Web-Crawler/blob/master/results/Screen%20Shot%202017-03-23%20at%2012.43.29%20AM.png?raw=true)
