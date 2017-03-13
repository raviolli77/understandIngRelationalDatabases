# Understanding Relational Databases
## Created by: Raul Eulogio

# Table of Contents
1. [Abstract](#Abstract)
2. [OLAP](OLAP.md)
3. [In-Memory Database Connection](connectingInMemoryDatabasePython.md)[Easy]
4. [Open-Database Connectivity](#ODBCConnection)[Hard]

## <a name="Abstract"></a>Abstract
This repository was created as a way to document database warehousing with respect to a company/entity for accurate and succint data collection and extraction. Going into concepts that relate to creating effective database including, but not limited to: normalization of data, Data Marting, and efficient data collecting practices. I will create seperate markdown files relating to different aspects of relational databases. For now (as of 2/6/2017) I have created a brief overview of 


## <a name="OLAP"></a>Online Analytical Processing Server
This [section](OLAP.md) goes over the the concept of **Online Analytical Processing Server** and all things relating to this data warehouse strategy

## <a name="InMemory"></a>In-Memory Database Connection Using Python
This [section](connectingInMemoryDatabasePython.md) goes over the basics of connecting to an in-memory database using **Python**. Relevant to this documentation because **SQL** does have its limitations with respect to *data analysis* and *data mining*. So a popular and powerful tool is **Python** with which you can connect to **SQL** databases. So this is a small introduction into doing so. 

## <a name="ODBCConnection"></a>Open-Database Connectivity 
This file can be seen as a continuation of the previous article, but goes into more detail. It focuses on Connections done through API calls called **Open-Database Connectivity** for accessing databases. This goes more indepth and focuses on connecting to databases that are all ready established, so say a database that you work with in a company. The previous [article](connectingInMemoryDatabasePython.md) focuses on teaching how to do so with an *in-memory database*, this section goes into detail about how to connect to an existing database. 
