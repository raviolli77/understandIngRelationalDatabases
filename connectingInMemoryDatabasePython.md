# Connecting to an In-Memory Database
## Documentation relating to connecting to an *In-Memory Database*. 

**Contributors**:
+ Raul Eulogio
+ Jun Seo Park

For this exercise we will be recreating a process known as *Open Database Connection* or *ODBC* for short. As you progress on your quest to becoming a data savvy nerd, an important concept and process is *ODBC*; although much of the data abundantly available is becoming less and less **SQL** based, knowing this process is still a useful tool and skill to possess. 

*What is SQL?*

Large pools of data can't always be stored in a *csv* or *txt* file as our classes have made us believe. So the use of *databases* are necessary for data collection, extraction, and ultimately data mining. The conventional databases are referred to as relational databases, which are usually **SQL** databases. **SQL** is short for Structured Query Language, and **SQL**  databases can be thought of as huge as storage rooms for data relating across different tables. The reason they are *relational* is that the different tables available in a **SQL** database often relate to each other through *unqiue identifiers*. 

*Where do ODBC's come into play?*

When doing data analysis, you will quickly find that **SQL** has its limitations in its ability to create sophisticated calculations (i.e. machine learning, data visualizations, and other useful data based analystics). So the way we, as aspiring data scientist, cirumnavigate this to do all the cool shit we want to do is through *ODBC's*. In our case, we would want to extract data from a **SQL** database and do our analysis in **Python**, which is all made possible thanks to *ODBC's*!  


Example: 

let's insert values into our new table two tables: one called customer and the other called transactions. The table customer contains all the information relating to the customers of this table (unduplicated), while the transactions table contains all transactions made by the customers (so there can be multiple instances of customers), but ultimately this table will contain an ID that makes every transaction unique.

Here I will demonstrate the process of creating said tables using **SQL** syntax (which is relatively easy):


	CREATE TABLE customers(
		customer_id VARCHAR(36),
		first_name VARCHAR(50),
		last_name VARCHAR(50),
		address VARCHAR(200), 
		date_enrolled DATETIME
		);

So what you saw there was the creation of a table called `customers` where each column is created with a specific **data type**. This is important to learn because it helps you learn variable types, which is something that many people overlook. 

Next let's insert values into our new table:

	INSERT INTO customers (customer_id,first_name,last_name,address,date_enrolled)
	VALUES (1, 'Ben', 'Dover', '1738 S. Forest Hill Drive', '7/4/2016');	

And if you wanted to view the table with the newly added data you run this simple command:

	SELECT *
	FROM customers;

So this is the basic syntax when it comes to creating, inserting and displaying tables in **SQL**

## Bringing it back to ODBC's

**NOTE**: If you know how to install third party modules just move on to the next section (but you will need to install sqlite3!)

Obviously this isn't a **SQL** tutorial, but I wanted to show you the basics of **SQL** syntax. Now I will relate this all back to **ODBC's**. 

For this work shop, we will be using **sqlite3**, to connect to an in-memory databasse. An in-memory database is a database that is created when you connect using **Python** (and ceases to exist as soon as you stop your **Python** session). Useful tool to practice *ODBC's*. So now I will show how to go about this process using **Python 3** and the 3rd party module **sqlite3**. 

Preliminary:
Make sure you have sqlite3 downloaded. The way you can check this is by running **Python** on the terminal as such:


	$ python

You shoud be greated with the **Python** Interpreter as such:

	Python 3.5.2 |Anaconda 4.2.0 (64-bit)| (default, Jul  2 2016, 17:53:06) 
	[GCC 4.4.7 20120313 (Red Hat 4.4.7-1)] on linux
	Type "help", "copyright", "credits" or "license" for more information.
	>>>

Once you have this running execute this command:

    >>> import sqlite3


If you are given an error code, then you **do not** have it installed yet. To install you do this `pip` command) 

	pip3 install sqlite3

And you should be set!

## ODBC using Python
For this section you will be connecting and using a database with **Python**. For this exercise, I will provide guidelines to going about this process, important to note that for the data set provided, there will be some additional research needed on your part since for this example I will be inputting the data manually. Whereas you will be grabbing the data from a file I provide. 

Here I will provide the script which I will call `dataExtraction.py` to do and run the database connection and output a csv file with the appropriate data. 

`dataExtraction.py` Script:

	# Import appropriate modules first
	import numpy as np
	import pandas as pd
	import sqlite3

	# CREATE A STRING THAT PRODUCES THE SQL SYNTAX NEEDED TO CREATE THE TABLE
	# SIMILAR TO WHAT I DID EARLIER
	query = """
	CREATE TABLE customers(
		customer_id VARCHAR(36),
		first_name VARCHAR(50),
		last_name VARCHAR(50),
		address VARCHAR(200), 
		date_enrolled DATETIME
		);"""

	# HERE WE'RE CONNECTING TO THE IN-MEMORY DATABASE
	# WE'RE CALLING THIS CONNECTION 'con'
	con = sqlite3.connect(':memory:')
	# HERE WE TELL THE CONNECTION TO EXECUTE THE QUERY WHICH IS CREATING OUR TABLE
	con.execute(query) 
	# NOW WE COMMIT THE QUERY
	con.commit()

	# LET'S INSERT SOME INFORMATION INTO OUR TABLE
	# IMPORTANT TO NOTE THAT THE FORMAT THIS IS IN IS tuple WITHIN A LIST
	data = [(69, 'Carl', 'Marx', '420 W. Praiseit Street', '11/9/2007'),
	(27, 'Paco', 'Jerte', '777 N. Pardall Way', '9/10/2009')]

	# NEXT WE USE A SQL STATEMENT TO INSERT THE 'data' LIST INTO OUR 'customers' TABLE
	stmt = "INSERT INTO customers VALUES(?,?,?,?,?)"

	# HERE WE EXECUTE THE STATEMENT TO INSERT THE DATA INTO OUR TABLE
	con.executemany(stmt, data)
	# WE COMMIT AGAIN
	con.commit()

	# NEXT WE DO A QUERY TO SEE ALL THE DATA IN OUR TABLE
	cursor = con.execute('SELECT * FROM customers')


	rows = cursor.fetchall()

	# NEXT WE CONVERT THE RETRIEVED QUERY INTO A DATAFRAME USING PANDAS
	customersPD = pd.DataFrame(rows, columns = list(zip(*cursor.description))[0])

	print("It worked! :)")
	# FIN :)


So now that we have this process done a clutch ass process that **Python** has is the ability to import from other python scripts. So say we are ready for data analysis. Of course we don't want the files to be too cluttered so we create a new file called `dataAnal.py`, where all the fun data stuff happens. First, we run the script `dataExtraction.py on the terminal as such:

	$ python dataExtraction.py

You should see:

	It worked! :) 

**Important to Note**: The more *Pythonic* was is too add a shebang at the start of your script as such:

	#!/usr/bin/env python3

Then (I know this works for *Linux* didn't research Windows or Mac) you enter the command in the terminal:

	chmod +x dataExtraction.py

Now to make your file executable you just run:

	./dataExtraction.py

Not entirely necessary in this analysis but helpful since you will be using the contents from this file within context of another script. Facilitates the transition of the dataframes that you created in `dataExtraction.py`. 

Now I'll show you the contents of `dataAnal.py` and the neat little trick we do to ensure we can use the stuff we did on `dataExtraction.py`


Loading data from `dataExtraction.py` into `dataAnal.py`:

	#!/usr/bin/env python3
	# LOAD MODULES AT THE BEGINNING OF OUR SCRIPT
	import numpy as np
	import pandas as pd # Data frames
	from dataExtraction import customersPD # IMPORTANT AF!!!!
	import matplotlib.pyplot as plt # Visuals
	import seaborn as sns # Danker visuals
	from sklearn.model_selection import train_test_split # Create training and test sets
	from sklearn.model_selection import KFold, cross_val_score # Cross validation
	from sklearn.ensemble import RandomForestClassifier # Random Forest


Notice what we did, we imported the DataFrame from the previous script! So within `dataAnal.py` let's also include this to see if it worked. 

`dataAnal.py` Script:

	#!/usr/bin/env python3
	# LOAD MODULES AT THE BEGINNING OF OUR SCRIPT
	import numpy as np
	import pandas as pd # Data frames
	from dataExtraction import customersPD # IMPORTANT AF!!!!
	import matplotlib.pyplot as plt # Visuals
	import seaborn as sns # Danker visuals
	from sklearn.model_selection import train_test_split # Create training and test sets
	

	print(customersPD.head())

So if you were to run the script on the terminal you would get (if done correctly):

	$ python dataAnal.py
(Or `./dataAnal.py`)
Output:

		customer_id 	first_name	 last_name	 address 				date_enrolled
	0	69				Carl		 Marx		420 W. Praiseit Street  11/9/2007

	1	27				Paco  		 Jerte		777 N. Pardall way  	9/10/2009


This also works with **Jupyter Notebooks** as long as you make sure you run the data extraction script within the same session that you run your analysis script and have all files in the same directory!

The rest is up to your team to do the fun data analysis stuff!

**Sources Cited**
+ [SQL Syntax](https://www.w3schools.com/sql/)
+ [Python for Data Analysis](http://www3.canisius.edu/~yany/python/Python4DataAnalysis.pdf) (**Note**: page 190 contaings example inspiring this exercise)