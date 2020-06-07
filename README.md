# Song Play Analysis With S3 and Redshift

#### Description

In this project we are going to use two Amazon Web Services,
[S3](https://aws.amazon.com/en/s3/) (Data storage) <br>
and [Redshift](https://aws.amazon.com/en/redshift/) (Data warehouse with ``columnar storage``)

Data sources are provided by two public ``S3 buckets``. One bucket contains <br>
info about songs and artists, the second has info concerning actions done <br> by users (which song are listening, etc.. ). The objects  contained in both buckets <br> are JSON files. The song bucket has all
the files under the same directory but <br> the event ones don't,
so we need a descriptor file (also a JSON) in order to extract <br> data from the folders by path. We used a descriptor file because we don't have a common prefix on folders

The Redshift service is where data will be ingested and transformed, <br>
in fact though COPY command we will access to the JSON files inside <br>
the buckets and copy their content on our staging tables

--------------------------------------------

#### Schema definition


To represent this context a ``Star schema`` has been used <br>

The songplays table is the core of this schema, is it our fact table and <br>
it contains foreign keys to four tables;
* start_time REFERENCES time(start_time)
* user_id REFERENCES time(start_time)
* song_id REFERENCES songs(song_id)
* artist_id REFERENCES artists(artist_id)

There are also two staging tables; One for song dataset and one for <br>
and one for event dataset


--------------------------------------------

#### ETL process

In this project most of ETL is done with SQL (Python used just as bridge), transformation and data normalization is done by Query, check out the ``sql_queries`` python module

--------------------------------------------

#### How to run
Although the data-sources are provided by two [``S3 buckets``](https://aws.amazon.com/en/s3/) the only thing you need for running the example is an [``AWS Redshift Cluster``](https://aws.amazon.com/en/redshift/) up and running

And of course [Python](https://www.python.org/downloads/) <br>

<b> Notes: </b>
* In this example a Redshift ``dc2.large``  cluster with <b> 4 nodes </b> has been created, with a cost of ``USD 0.25/h (on-demand option)`` per cluster
* In this example we will use [``IAM role ``](https://docs.aws.amazon.com/en_us/IAM/latest/UserGuide/id_roles.html) authorization mechanism, the only policy attached to this IAM will be am [``AmazonS3ReadOnlyAccess``](https://aws.amazon.com/en/blogs/security/organize-your-permissions-by-using-separate-managed-policies/)


After opening terminal session, set your filesystem on project root folder <br>
and  insert these commands in order to run the demo: <br><br>
<I> This will create our tables, this must be runned first </I> <br>
`` python create_tables.py`` <br>

<I> And this will execute our ETL process </I> <br>
`` python etl.py`` <br>

--------------------------------------------

#### Project structure
This is the project structure, if the bullet contains ``/`` <br>
means that the resource is a folder:

* <b> /img </b> - Simply a folder with images that are used in this ``md``
* <b> create_tables.py </b> - This script will drop old tables (if exist) ad re-create new tables
* <b> etl.py </b> - This script executes the queries that extract JSON data
from the S3 bucket and ingest them to Redshift
* <b> sql_queries.py </b> - This file contains variables with SQL statement in String formats,  partitioned by CREATE, DROP, COPY and INSERT statements
* <b> dhw.cfg </b> - Configuration file used that contains info about Redshift, IAM and S3

--------------------------------------------

#### Create Table Schema

1. Write a SQL CREATE statement for each of these tables in sql_queries.py
2. Complete the logic in create_tables.py to connect to the database and create these tables
3. Write SQL DROP statements to drop tables in the beginning of create_tables.py if the tables already exist. This way, you can 4. run create_tables.py whenever you want to reset your database and test your ETL pipeline.
5. Launch a redshift cluster and create an IAM role that has read access to S3.
6. Add redshift database and IAM role info to dwh.cfg.
7. Test by running create_tables.py and checking the table schemas in your redshift database.

--------------------------------------------

#### Build ETL Pipeline

1. Implement the logic in etl.py to load data from S3 to staging tables on Redshift.
2. Implement the logic in etl.py to load data from staging tables to analytics tables on Redshift.
3. Test by running etl.py after running create_tables.py and running the analytic queries on your Redshift database to compare your results with the expected results.
4. Delete your redshift cluster when finished.

--------------------------------------------

#### Final Instructions

1. Import all the necessary libraries
2. Write the configuration of AWS Cluster, store the important parameter in some other file
3. Configuration of boto3 which is an AWS SDK for Python
4. Using the bucket, can check whether files log files and song data files are present
5. Create an IAM User Role, Assign appropriate permissions and create the Redshift Cluster
6. Get the Value of Endpoint and Role for put into main configuration file
7. Authorize Security Access Group to Default TCP/IP Address
8. Launch database connectivity configuration
9. Go to Terminal write the command "python create_tables.py" and then "etl.py"
10. Delete the cluster, roles and assigned permission
