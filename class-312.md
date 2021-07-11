# **Mongo and Mongoose**
* ### Fill in the chart below with five differences between SQL and NoSQL databases
    |   SQL | NoSQL |
    | --------| ------ |
    | are table based databases whereas | databases are document based, key-value pairs, graph databases or wide-column stores|
    |  have predefined schema |  have dynamic schema for unstructured data. |
    | are scaled by increasing the horse-power of the hardware | are scaled by increasing the databases || are good fit for the complex query intensive environment | are not good fit for complex queries |
    | are not best fit for hierarchical data storage | fits better for the hierarchical data storage | 
    | are vertically scalable. You can manage increasing load by increasing the CPU, RAM, SSD, etc, on a single server | are horizontally scalable. You can just add few more servers easily in your NoSQL database infrastructure to handle the large traffic. |

* ### What kind of data is a good fit for an SQL database?
    complex query intensive environment, heavy duty transactional type applications
* ### Give a real world example
     MySql, Oracle, Sqlite, Postgres and MS-SQL

* ### What kind of data is a good fit a NoSQL database?
    hierarchical data storage as it follows the key-value pair way of storing data similar to JSON data

* ### Give a real world example.
    MongoDB, BigTable, Redis, RavenDb, Cassandra, Hbase, Neo4j and CouchDb

* ### Which type of database is best for hierarchical data storage?
    **NoSQL Database**

* ### Which type of database is best for scalability?
    **SQL Database**


* ### What does SQL stand for?
    **Structured Query Language**

* ### What is a realational database?
    A relational database is a type of database that stores and provides access to data points that are related to one another. Relational databases are based on the relational model, an intuitive, straightforward way of representing data in tables. In a relational database, each row in the table is a record with a unique ID called the key. The columns of the table hold attributes of the data, and each record usually has a value for each attribute, making it easy to establish the relationships among data points

* ### What type of structure does a relational database work with?
    - A relational database consists of a collection of tables, each having a unique name.
    - A row in a table represents a relationship among a set of values.Thus a table represents a collection of relationships.
    - There is a direct correspondence between the concept of a table and the mathematical concept of a relation. A substantial theory has been developed for relational databases.

* ### What is a ‘schema’?
    The database schema is its structure described in a formal language supported by the database management system (DBMS). The term "schema" refers to the organization of data as a blueprint of how the database is constructed (divided into database tables in the case of relational databases)

* ### What is a NoSQL database?
    A NoSQL (originally referring to "non-SQL" or "non-relational") database provides a mechanism for storage and retrieval of data that is modeled in means other than the tabular relations used in relational databases. Such databases have existed since the late 1960s, but the name "NoSQL" was only coined in the early 21st century,triggered by the needs of Web 2.0 companies.NoSQL databases are increasingly used in big data and real-time web applications

* ### How does it work?
    NoSQL databases store data in documents rather than relational tables. Accordingly, we classify them as "not only SQL" and subdivide them by a variety of flexible data models. Types of NoSQL databases include pure document databases, key-value stores, wide-column databases, and graph databases. NoSQL databases are built from the ground up to store and process vast amounts of data at scale and support a growing number of modern businesses.
* ### What is inside of a Mongo database?
    documents (specifically BSON documents) which are gathered together in collections. A database stores one or more collections of documents.

* ### Which is more flexible - SQL or MongoDB? and why.
    MongoDB is more flexible and ensures high and diverse data availability, a SQL Database operates with the ACID (Atomicity, Consistency, Isolation, and Durability) properties and ensures greater reliability of transactions. 

* ### What is the disadvantage of a NoSQL database?
    - NoSQL databases don’t have the reliability functions which Relational Databases have (basically don’t support ACID
    - In order to support ACID developers will have to implement their own code, making their systems more complex.
    - NoSQL is not compatible (at all) with SQL
    - NoSQL are very new compared to Relational Databases, which means that are far less stable and may have a lot less functionalities

