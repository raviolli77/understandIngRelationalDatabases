# Data Warehousing - Online Analytical Processing Server (OLAP)
## Documentation regarding OLAP Database 
(Tutorial Found [Here](https://www.tutorialspoint.com/dwh/dwh_olap.htm))

Definition: *"[OLAP] is computer processing that enables a user to easily and selectively extract and view data from different points of view"* (Definition taken from [Margaret Rouse's article](http://searchdatamanagement.techtarget.com/definition/OLAP))

**Contributor**:
+ Raul Eulogio

# Table of Contents
1. [Relational OLAP](#rolap)
	+ [Data Cube](#datacube)
	+ [Data Mart](#datamart)
	+ [Delivery Process](#deliveryprocess)
	+ [Three-Tier Data Warehouse Architecture](#threetier)
2. [OLAP Operations](#olapOper)
	+ [Roll-Up Operations](#rollupoper)
	+ [Drill Down Operations](#drilldownoper)
	+ [Slice Operations](#sliceoper)
	+ [Dice Operations](#diceoper)
	+ [Pivot Operations](#pivotoper)
3. [Schemas](#schemas)
	+ [Cube Schema](#cubeschema)
	+ [Star Schema](#starschema)
	+ [Snow Flake Schema](#snowflakeschema)
4. [Sources Cited](#sourcescited)

## Four types of OLAP servers:
+ Relational OLAP (ROLAP)
+ Multidimensional OLAP (MOLAP)
+ Hybrid OLAP (HOLAP)
+ Specialized SQL Servers

## Relational OLAP <a name='rolap'></a>
These servers are placed between a relational back-end server and client front-end tool. For its ability to store and manage data, ROLAP uses relational/extended-relational DBMS (Database Management Systen). We typically model OLAP with a **Data Cube**. 

Important to note:
+ ROLAP are highly scalable
+ Analyze large volumes of data across dimensions
+ 

### Data Cube <a name='datacube'></a>
These help us model our data in multiple dimensions. Best way to show is by examples prvoided by the tutorial, so say we have a 2-D representation of our sales records with respect to one location New Delhi:

![alt-text](https://www.tutorialspoint.com/dwh/images/data_cube2d.jpg)

This works well at describing our data but say we wanted to expand on the `location` table a little  more, by say adding two other locations (Gurgaon and Mumbai), then the new 2-D table isn't as intuitive as before:

![alt-text](https://www.tutorialspoint.com/dwh/images/data_cube3d.jpg)

So we create a 3-D table or **Data Cube** for this new query which makes more intuitive sense than the previous image:

![alt-title](https://www.tutorialspoint.com/dwh/images/data_cube3d1.jpg)

This **Data Cube** is then the basis for our relational database, and will help in understanding the dissection and ultimately data analysis for our company/entity. A process known as **Data Mart**

### Data Mart <a name='datamart'></a>
Once we begin to understand the relationship of our database, we can begin creating *queries* for specific target groups, this is referred to as **Data Mart**. An example of this would be say your company is interested in data specific to a certain program that a specific branch your company operates under. Therefore **Data Marts** are very specific pieces of information confined to subjects.  

Here is an image from the tutorial to give a visual representation:

![alt-title](https://www.tutorialspoint.com/dwh/images/data_mart.jpg)

### Delivery Process <a name='deliveryprocess'></a>

The next step in the tutorial goes into **Delivery Process** which you can read up on [here](https://www.tutorialspoint.com/dwh/dwh_delivery_process.htm), but with respect to *data analysis* this is the step where we go into the database and do what is called **Data Wrangling**. This process involves extracting the data, cleaning the data, and transforming the data to fit our reporting/analysis needs. 

I won't go into too much detail about this section, because this section is more of a 'you should know how to do' section and 'better to do than to show examples' area. But it is important to note in this file for the sake of teaching OLAP. 

### Three-Tier Data Warehouse Architecture <a name='threetier'></a>

+ **Bottom Tier** - This is the data warehouse database server. So a **MySQL** database, **PostgreSQL** database, or **Microsoft Access SQL** database.
+ **Middle Tier** - This is the **Relational Data Cube** and in our case the OLAP server.
+ **Top Tier** - This is the Front End Client Layer so this can be a **Third Party Database**, Typically a website used to present the information stored so that the *data analysis* can take place!

From here we will go into OLAP operations available for the **Middle Tier** of this Three-Tier Data Warehouse Architecture since this is the area that must be **Normalized** and developed properly to ensure consistent and reliable data extraction.  

## OLAP Operations <a name='olapOper'></a>
OLAP servers run on multidimensional view of data, the operations will be discussed in multidimensional data.

Example of a multidimensional OLAP cube from the previous example:

![alt-title](https://www.tutorialspoint.com/dwh/images/data_cube3d1.jpg)

List of OLAP operations:
+ Roll-Up (Drill-up or aggregation operation)
+ Drill-down
+ Slice and Dice
+ Pivot (rotate)

### Roll-Up Operation <a name='rollupoper'></a>
Roll-Up refers to the aggregaton of a **Data Cube** in one of two ways:

+ Climbing a concept hierarchy for a dimension
+ By dimension reduction

#### Example 1
So say we have a table as follows and we wanted to lump cities into their respective countries so that if we were to create a report it would be more readable and ultimately more 'human-friendly', we would perform Roll-Up as follows. 

Roll-Up Example:

![alt-title](https://www.tutorialspoint.com/dwh/images/rollup.jpg)

This concept is referred to as **Concept Hierarchy** where 'street < city < province < country'.

#### Example 2
Example (Borrowed from [OLAP's article about OLAP Operations](http://athena.ecs.csus.edu/~olap/olap/OLAPoperations.php)):
Say for our original query we have a table set up so that we can see the relationship with the respective week (in our case week1 and week2) and the temperatures associated with these weeks. 

As shown here:
![alt-title](http://athena.ecs.csus.edu/~olap/olap/rollupexample.JPG)

Now say we wanted to create 3 levels that are more user friendly, and we decide to create these levels as such:

+ Hot (80-85)
+ Mild (64-69)
+ Cold (64-69)

The Roll-Up Operations would then create this new query:

![alt-title](http://athena.ecs.csus.edu/~olap/olap/newrollup.JPG)

Thus for this example we have the concept hierarchy as 'hot > day >week'

That's a succint overview of the Roll-Up operation, although you will often find your self doing these without knowing the proper name. For now I am creating documentation to fully understand the processes of database management and warehousing. 

TO DO:
+ Create **SQL** queries that perform roll ups
+ Find example data sets to do concrete examples on
+ Add more details/explanations

### Drill-Down Operation <a name='drilldownoper'></a>
This procedure is the opposite of *roll-up*, you can do this process either way:
+ Stepping down the hierarchy 
+ Introducing a new dimension

This example shows a pretty good demonstration, where our data was originally in *quarters*, but we did a *drill-down* procedure to receive monthly information.

![alt-title](https://www.tutorialspoint.com/dwh/images/drill_down.jpg)

Super simple concept no real need to go into too much detail, aside from showing a *SQL* query doing *drill-down* (Which will be added later..)

### Slice Operation <a name='sliceoper'></a>
*Slicing* is the process of selecting one particular dimension from a cube, giving us a new sub-cube. Again picture diagram showcases this best:

![alt-title](https://www.tutorialspoint.com/dwh/images/slice.jpg)

To reiterate we *sliced* to only receive information with respect to *Q1*. Very common practice in *SQL*

**SQL** Syntax:

	SELECT *
	FROM tbl1
	WHERE time = 'Q1';


### Dice Operation <a name='diceoper'></a>
This process is similar to *slice*, but instead we go about with two or more dimensions. So to give context using the last example, we are using both *Q1* and *Q2* as well as only picking two specific locations, *Vancouver* and *Toronto*. 

**SQL** Syntax:

	SELECT *
	FROM tbl1 
	WHERE (location = 'Toronto' OR
	location = 'Vancouver') AND
	(time = 'Q1' OR
	time = 'Q2');

### Pivot Operation <a name='pivotoper'></a>


## Schemas <a name='schemas'></a>

### Cube Schema <a name='cubeschema'></a>

### Star Schema <a name='starschema'></a>

### Snowflake Schema <a name='snowflakeschema'></a>

## Sources Cited <a name='sourcescited'></a>

+ [TutorialsPoint's Data Warehousing Tutorial](https://www.tutorialspoint.com/dwh/dwh_olap.htm)
+ [Concept Hierarchies Tutorial And Examples](http://athena.ecs.csus.edu/~olap/olap/OLAPoperations.php)
+ [SearchDataManagement's Definition of OLAP](http://searchdatamanagement.techtarget.com/definition/OLAP)
