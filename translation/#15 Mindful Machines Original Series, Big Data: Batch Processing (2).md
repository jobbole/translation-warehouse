# Mindful Machines Original Series, Big Data: Batch Processing

APRIL 24, 2018 BY MARCIN MEJRAN

This is the second part of the Mindful Machines series on Big Data (aka: Big Data Cheat Sheet), in the [previous post](https://mindfulmachines.io/blog/2018/4/10/series-big-data-batch-storage) we covered Batch Storage, in following posts we’ll cover Stream Processing, NoSQL and Infrastructure.

Your historical data is overflowing and you want to do something with it? What do you choose to process it? Presto? Spark? Redshift? MapReduce? In this post we go over the myriad of options out there and how they stack against each other. This isn’t a complete list of available technologies but rather the highlight reel that, among other things, explicitly avoids enterprise solutions although does cover PaaS.

## Programmatic Batch Processing

These systems provide a programmatic (Java, Scala, Python, etc.) interface for querying data stored in batch storage systems (HDFS, S3, Cassandra, HBase, etc.).

*   **Overall**
    *   Provide a flexible interface for querying data
    *   Schemas need to be managed manually or loaded from files
    *   Modern system provide high level APIs that allow for whole query optimizations
*   **[Apache Hadoop MapReduce](https://hortonworks.com/apache/mapreduce/):** A cornerstone of the Big Data ecosystem which provides a way to efficiently process petabytes of data
    *   Open source but PaaS and enterprise versions exist
    *   Written in Java
    *   Released in 2006
    *   Implementation of Google’s [MapReduce paper](https://static.googleusercontent.com/media/research.google.com/en//archive/mapreduce-osdi04.pdf)
    *   Provides a way to query large amounts of data across multiple machines in an efficient and easy to implement way compared to traditional cluster computing approaches
    *   Writing raw Java MapReduce code is relatively complicated
    *   Google has not been using MapReduce as it’s primary big data processing model since [2014](http://www.datacenterknowledge.com/archives/2014/06/25/google-dumps-mapreduce-favor-new-hyper-scale-analytics-system) and there are newer technologies that are unseating MapReduce in the open source world (see other entries).
    *   Requires a Hadoop (YARN) cluster which introduces operational overhead
        *   PaaS versions exist ([Amazon EMR](https://aws.amazon.com/emr/), [Azure HDInsight](https://azure.microsoft.com/en-us/services/hdinsight/), [Google Dataproc](https://cloud.google.com/dataproc/)) and can lower operational knowledge needed
    *   Commercial support provided by [Hortonworks](https://hortonworks.com/) and [Cloudera](https://www.cloudera.com/)
*   **[Cascading](https://www.cascading.org/projects/cascading/)/[scalding](https://github.com/twitter/scalding):** Java/Scala, respectively, frameworks that abstract away the complexity of writing MapReduce code
    *   Open source
    *   Written in Java/Scala
    *   Significantly lowers the overhead of writing MapReduce code
    *   Can leverage [Tez](https://hortonworks.com/apache/tez/) or [Flink](https://flink.apache.org/) to significantly improve [performance](http://scalding.io/2015/05/scalding-cascading-tez-%E2%99%A5/)
*   **[Apache Spark](https://spark.apache.org/):** A highly popular cluster computing framework based on in memory storage of intermediate data
    *   Open source but PaaS and SaaS versions exist
    *   Written in Scala
    *   Started in 2009, described in a [paper](http://people.csail.mit.edu/matei/papers/2010/hotcloud_spark.pdf) published in 2010
    *   Shines in providing a mostly unified API across Python, Scala, Java, R and SQL that lets you mix together native code and optimized built-in commands
    *   Support a streaming paradigm on top of it’s batch processing engine
    *   Provides a build in machine learning library ([MLLib and ML](https://spark.apache.org/docs/latest/ml-guide.html))
    *   Contains significant configurable settings and requires tuning to get good [performance](https://databricks.com/blog/2017/07/12/benchmarking-big-data-sql-platforms-in-the-cloud.html)
    *   Spark’s biggest code contributor and commercial backer (Databricks) markets how much [faster](https://databricks.com/blog/2017/07/12/benchmarking-big-data-sql-platforms-in-the-cloud.html) it’s proprietary PaaS version is than the open source version which creates skewed incentives for them.
    *   PaaS solutions provided by [Amazon EMR](https://aws.amazon.com/emr/), [Azure HDInsight](https://azure.microsoft.com/en-us/services/hdinsight/) and [Google Dataproc](https://cloud.google.com/dataproc/)
    *   SaaS solution provided by [Databricks Unified Analytics Platform](https://databricks.com/product/unified-analytics-platform)
    *   Commercial support provided by [Hortonworks](https://hortonworks.com/) and [Cloudera](https://www.cloudera.com/)
*   **[Apache Flink](https://flink.apache.org/):** Cluster computing framework that aims to provide improvements compared to Spark
    *   Open source but PaaS versions exist
    *   Written in Java and Scala
    *   Released in 2013
    *   Provides an API across Python, Scala, Java and SQL
    *   Support a batch paradigm on top of it’s streaming processing engine
    *   Less configuration overhead than Spark
    *   Provides a built in machine learning library, [FlinkML](https://ci.apache.org/projects/flink/flink-docs-release-1.4/dev/libs/ml/index.html), but it’s less comprehensive and [performant](https://link.springer.com/article/10.1186/s41044-016-0020-2) than Spark’s
    *   Newer project that shows a lot of promise but Spark has added significant performance and feature improvement in newer versions that likely more than closed the gap
    *   PaaS solutions provided by [Amazon EMR](https://aws.amazon.com/emr/) and [Google Dataproc](https://cloud.google.com/dataproc/)

## SQL Batch Processing

These frameworks provide a SQL interface for querying data stored in HDFS or other blob storage systems (S3, etc.) in a distributed fashion.

*   **Overall**
    *   Provide a centralized schema repository
    *   Allow for whole query optimizations but restrict you to only using SQL and potentially custom UDFs
    *   Most require a traditional SQL server to host table metadata
*   **[Apache Hive](https://hive.apache.org/):** A SQL layer originally on top of HAdoop MapReduce and now on top of YARN
    *   Open source but PaaS versions exist
    *   Written in Java
    *   Released by Facebook in 2009
    *   Custom UDFs, in Java, can be difficult and time consuming to write
    *   More optimized for complex analytical queries
    *   The newest versions partially bypass MapReduce and run a daemon on individual nodes ([LLAP](https://cwiki.apache.org/confluence/display/Hive/LLAP)) to further optimize performance.
        *   As a result newer versions stack up quite well [performance](https://www.youtube.com/watch?v=dS1Ke-_hJV0) [wise](https://www.slideshare.net/ssuser6bb12d/hive-presto-and-spark-on-tpcds-benchmark) against Presto and Spark
    *   PaaS solutions provided by [Amazon EMR](https://aws.amazon.com/emr/), [Azure HDInsight](https://azure.microsoft.com/en-us/services/hdinsight/) and [Google Dataproc](https://cloud.google.com/dataproc/)
    *   Commercial support provided by [Hortonworks](https://hortonworks.com/) and [Cloudera](https://www.cloudera.com/)
*   **[Apache Spark SQL](https://spark.apache.org/sql/):** A SQL computing layer that is built on top of Spark
    *   Open source
    *   Written in Scala
    *   Started as Shark in 2010
    *   Requires a Spark cluster
    *   More optimized for complex analytical queries
    *   Custom UDFs are easy to write in Scala, Python, Java or R
    *   Requires a Spark cluster which can be difficult to tune
    *   PaaS solutions provided by [Amazon EMR](https://aws.amazon.com/emr/), [Azure HDInsight](https://azure.microsoft.com/en-us/services/hdinsight/) and [Google Dataproc](https://cloud.google.com/dataproc/)
    *   Commercial support provided by [Hortonworks](https://hortonworks.com/) and [Cloudera](https://www.cloudera.com/)
*   **[Apache Flink SQL](https://flink.apache.org/):**  A SQL computing layer that is built on top of Flink
    *   Open source
    *   Written in Java
    *   Released in 2016
    *   Requires a Flink cluster
    *   Custom UDFs are easy to write in Scala or Java
    *   Performance compared to Spark is hard to get numbers for
    *   PaaS solutions provided by [Amazon EMR](https://aws.amazon.com/emr/) and [Google Dataproc](https://cloud.google.com/dataproc/)
*   **[Presto](https://prestodb.io/):** A SQL computing layer optimized for massive datasets
    *   Open source
    *   Written in Java
    *   Released in 2013 by Facebook
    *   More optimized for many smaller OLAP queries
    *   Support for custom Java UDFs
    *   Requires tuning to get good performance
    *   Provides comparable performance to [Redshift](https://engineering.grab.com/scaling-like-a-boss-with-presto)
    *   Performance improvements compared to [Spark](http://tech.marksblogg.com/billion-nyc-taxi-rides-ec2-versus-emr.html) although results may differ on [Databrick’s SaaS Spark](https://databricks.com/blog/2017/07/12/benchmarking-big-data-sql-platforms-in-the-cloud.html)
    *   Used by Facebook to query their 300PB data warehouse
    *   PaaS version in [Amazon Athena](https://aws.amazon.com/athena/)
*   **[Apache Impala:](https://impala.apache.org/)** A SQL computing layer released by Cloudera based on Google’s Dremel
    *   Open source
    *   Released in 2012 by Cloudera
    *   Written in C++
    *   Support for custom UDFs in C++ and Java (but Java is slower)
    *   Based on the [Dremel](https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/36632.pdf) paper by Google
    *   More optimized for many smaller OLAP queries
    *   Commercial support provided by [Cloudera](https://www.cloudera.com/)
*   **[Amazon Redshift Spectrum:](https://aws.amazon.com/redshift/spectrum/)** A computing engine version of Redshift
    *   Proprietary PaaS
    *   Unlike Redshift can scale computing independently of storage and access arbitrary file formats stored in S3
    *   Limited support for custom UDFs in Python
    *   Can leverage Redshift for table metadata
    *   Required a running Redshift cluster

## Data Warehouse

These are full featured Data Warehouses that tie together the data storage and data processing into a single entity.

*   **Overall**
    *   Low latency and high throughput query performance but not necessarily faster than other modern batch processing solutions
    *   Columnar data storage
    *   Limits on flexibility (data types, UDFs, data processing approaches, etc.)
    *   Lock-in if used as primary data store
    *   Computing tied to storage system in terms of sc aling
*   **[Druid](http://druid.io/):** Columnar data store designed to provide low-latency analytical queries
    *   Open source
    *   Written in Java
    *   Open sourced in 2012
    *   Provides sub-second analytical/OLAP queries
    *   Supports real-time ingestion of data rather than just batch ingestion
    *   Provides a limited subset of SQL queries (only large to small table joins)
    *   Custom UDF support exists in Java but is complicated
    *   Seamless scaling of the cluster up/down independently of storage
    *   Leverages “deep” storage such as S3 or HDFS to avoid data loss if nodes go down
    *   Complicated infrastructure setup involving multiple types of nodes and distributed storage (S3, HDFS, etc.)
        *   Number of external dependencies (S3/HDFS, ZooKeeper, RDBM) which increases operational overhead
    *   Well suited for time series data
    *   Used by [Airbnb, eBay, Netflix, Walmart and others](http://druid.io/druid-powered.html)
*    **[ClickHouse](https://clickhouse.yandex/):** Columnar data store designed to provide low-latency analytical queries and simplicity
    *   Open Source
    *   Written in C++
    *   Open sourced in 2016 by [Yandex](https://yandex.com/)
    *   No support for custom UDFs
    *   Significantly [higher performance](https://blog.cloudflare.com/how-cloudflare-analyzes-1m-dns-queries-per-second/#comment-3302778860) than Druid for some workloads
    *   Less scalable than Druid or other approaches
    *   Leverages Zookeeper but can run a single node cluster without it
*   **[Amazon Redshift](https://aws.amazon.com/redshift/):** A fully-managed data warehouse solution that lets you efficiently store and query data using a SQL syntax.
    *   Proprietary PaaS
    *   General purpose analytical store that support full SQL syntax
    *   Limited support for custom UDFs in Python
    *   Loading/unloading data takes time (hours potentially)
    *   No real time ingestion, only batch, although micro-batches can simulate real-time
    *   Need to explicitly scale the cluster up/down (with write downtime for the duration)
    *   *   Storage and computing are tied together
    *   Lack of complex data types such as arrays, structs, maps or native json
*   **[Google BigQuery](https://cloud.google.com/bigquery/):** A fully-managed data warehouse solution that lets you efficiently store and query data using a SQL syntax.
    *   Proprietary PaaS
    *   General purpose analytical store that support full SQL syntax
    *   Real time ingestion support
    *   Limited support for custom UDFs in Javascript
    *   [Fastest queries than Redshift but more expensive](https://blog.fivetran.com/warehouse-benchmark-dce9f4c529c1)
    *   Unlike Redshift it is serverless and you do not need to manage, scale or pay for a cluster yourself
    *   Supports complex data types (arrays, structs) but not native json
*   **[Azure SQL Data Warehouse:](https://azure.microsoft.com/en-us/services/sql-data-warehouse/)** A fully-managed data warehouse solution that lets you scale computing independently of storage
    *   Proprietary PaaS
    *   General purpose analytical store that support full SQL syntax
    *   No real time ingestion, only batch, although micro-batches can simulate real-time
    *   No real support for custom UDFs (only ones written in SQL)
    *   [Performance may not be the best compared to Redshift](http://sql10.blogspot.com/2017/02/sql-server-vs-azure-data-warehouse-vs.html)
    *   Computing nodes can be scaled independently of storage
    *   Lack of complex data types such as arrays, structs, maps or native json

## RDBM

The traditional SQL database may seem an odd choice however, in addition to simply scaling vertically, with [sharding](https://en.wikipedia.org/wiki/Shard_(database_architecture)) and read-replicas it can scale across multiple nodes. In the following points I’m focusing more on these databases as analytical data stores (relatively few large queries) rather than traditional databases (massive numbers of relatively small queries).

*   **Overall**
    *   Powerful [ACID](https://en.wikipedia.org/wiki/ACID) guarantees
    *   Row level updates and inserts
    *   Requires structured data however some databases also have support for free form JSON fields
    *   Can scale to handle large data sizes
        *   Vertically: Modern machines can be quite large so even a single machine can store significant data
        *   Horizontally: Sharding is possible although it requires additional manual setup and potentially client logic changes
    *   There are tradeoffs as you scale (ie: queries across partitions or complex queries)
        *   Computing tied to storage system in terms of scaling
        *   Multi-master or automatic failover setups can be tricky to get right so often a single point of failure exists
    *   Used by [Uber](https://eng.uber.com/mysql-migration/) and [Facebook](https://code.facebook.com/posts/190251048047090/myrocks-a-space-and-write-optimized-mysql-database/) to handle massive amounts of data
    *   There are better purpose built technologies if you truly ne ed to scale big
*   **[MySQL](https://www.mysql.com/)**
    *   Open source; PaaS and enterprise versions exist
    *   Support for JSON data types
    *   Recent support for window functions
    *   Commercial support by [Oracle](https://www.mysql.com/products/) (who owns MySQL), PaaS support by [AWS](https://aws.amazon.com/rds/)
*   **[MariaDB](https://mariadb.org/)**
    *   Open source
    *   Originally a fork of MySQL
    *   Supports window function
    *   No JSON data type but native functions for working with JSON
    *   Support for a [columnar](https://mariadb.com/products/technology/columnstore) storage engine which significantly speeds up analytical workloads
    *   Commercial support by [MariaDB](https://mariadb.com/), PaaS support by [AWS](https://aws.amazon.com/rds/)
*   **[PostgreSQL](https://www.postgresql.org/)**
    *   Open source;PaaS and enterprise versions exist
    *   Support for JSON data types
    *   Commercial support by various companies
    *   Better parallel single query optimizations than MySQL
    *   Third party support for [columnar](https://github.com/citusdata/cstore_fdw) storage engine which significantly speeds up analytical workloads
    *   Support for sharding via [PL/Proxy](https://plproxy.github.io/)
*   **[Amazon Aurora:](https://aws.amazon.com/rds/aurora/)** Fully managed MySQL and PostgeSQL compatible databases on AWS
    *   Proprietary PaaS
    *   Automatically and seamlessly allocates storage
    *   Data is replicated across and within availability zones
    *   Claims [improved performance](https://www.percona.com/blog/2016/05/26/aws-aurora-benchmarking-part-2/) compared to open source versions due to tight coupling with the SSD storage layer
        *   PostgreSQL performance may be [lower](https://www.chooseacloud.com/postgresql) on Aurora
    *   Lags behind open source in version support, Aurora MySQL 5.7 support came out over 2 years after MySQL 5.7
    *   Does not support clustering beyond read replicas
