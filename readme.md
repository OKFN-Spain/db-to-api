Database to API
=======================

Dynamically generate RESTful APIs from the contents of a database table. Provides JSON, XML, and HTML.

What Problem This Solves
------------------------

Creating an API to access information within existing database tables is laborious task, when done as a bespoke task. This is often dealt with by exporting the contents of the database as CSV files, and providing downloads of them as a “good enough” solution.

How This Solves It
------------------

*Database to API* acts as a filter, sitting between a database and the browser, allowing users to interact with that database as if it was a native API. The column names function as the key names. This obviates the need for custom code for each database layer.

When Alternative PHP Cache (APC) is installed, parsed data is stored within APC, which accelerates  its functionality substantially. While APC is not required, it is recommended highly.


Databases Supported
-------------------

We have tested the API with:

* PostgreSQL
* MySQL
* SQLite

API Structure
-------------

* all rows in a table: `/[database]/[schema]/[table].[format]`
* specific row in a table: `/[database]/[schema]/[table]/[ID].[format]`
* all rows matching a query: `/[database]/[schema]/[table]/[column]/[value].[format]`

Additional Parameters
---------------------

* `order_by`: name of column or columns to sort by, each column must be separated by comma. Add suffix '.desc' for reverse sorting ('.asc' also works)
* `limit`: number, maximum number of results to return
* `where`: 

e.g., `/[database]/[schema]/[table]/[column]/[value].[format]?order_by=[column_1].desc,[column_2]&limit=10`

Requirements
------------

* PHP
* Database
* PHP Data Objects (PDO) Extension
* APC (optional)

Usage
-----

1. Copy `config.sample.php` to `config.php`
2. Follow the below example to register a new dataset in `config.php`. Tip: It's best to provide read-only database credentials here.
3. Document the API.
4. Be aware that some Apache2 configurations prevent from reading .htaccess files
5. In .htaccess you should change the RewriteBase directive

How to Register a Dataset
-------------------------

Edit `config.php` to include a a single instance of the following for each dataset (including as many instances as you have datasets):

```php

$args = array( 
			'name' => 'database_name',
			'username' => 'username',
			'password' => 'password',
			'server' => 'localhost',
			'port' => 3306,
			'type' => 'mysql',
			'table_blacklist' => array(),
			'column_blacklist' => array(),
);

register_db_api( 'dataset_name', $args );

```

*Note: All fields (other than the dataset name) are optional and will default to the above.*

```php

$args = array( 
			'name' => 'mypgdatabase',
			'username' => 'mypguser',
			'password' => 'mypassword',
			'server' => 'localhost',
			'port' => 5432,
			'type' => 'pgsql',
			'table_blacklist' => array('secreta', 'secretb'),
			'column_blacklist' => array('secretcolumn'),
);

register_db_api( 'mypgdbapi', $args );

```

Retrieving the contents of the table `history` within this dataset as JSON would be accomplished with a request for `/mypgdbapi/history.json`. Note that it is the name of the dataset (`mypgdbapi`) and not the name of the database (`mypgdatabase`) that is specified in the URL.

License
-------

GPLv3 or later.

