# nicar2016_sqliteII

# SQLite II: counting, summing, and other math operations

[SQL Mnemonic Device.](http://www.sqlservercentral.com/articles/T-SQL/73634/)
##### Sweaty = SELECT
##### Feet = FROM
##### Will = WHERE
##### Give = GROUP BY
##### Horrible = HAVING 
##### Odors = ORDER BY

### Create database and table

* Create the new database and save the sqlite file

* Query to create the table

 ```
CREATE TABLE "nicar_CAdaycares031714" (
    "id" INT,
    "facility_type" VARCHAR, 
    "facility_number" VARCHAR, 
    "facility_name" VARCHAR, 
    "licensee" VARCHAR, 
    "facility_administrator" VARCAR, 
    "facility_telephone_number" VARCHAR, 
    "facility_address" VARCHAR, 
    "facility_city" VARCHAR, 
    "facility_state" VARCHAR, 
    "facility_zip" VARCHAR, 
    "county_name" VARCHAR, 
    "regional_office" VARCHAR, 
    "facility_capacity" INT, 
    "facility_status" VARCHAR, 
    "total_type_a" INT, 
    "license_first_date" VARCHAR, 
    "closed_date" VARCHAR, 
    "last_visit_date" VARCHAR, 
    "inspection_visits" INT, 
    "complaint_visits" INT, 
    "other_visits" INT, 
    "total_visits" INT, 
    "all_visit_dates" VARCHAR, 
    "inspection_visit_dates" , 
    "inspect_typea" INT, 
    "inspect_typeb" INT, 
    "other_visit_dates" VARCHAR, 
    "other_typea" INT, 
    "other_typeb" INT, 
    "all_complaint_type_a" INT )
 ```

* Import data using import wizard

### GROUP BY Statement:
* Groups things together, essentially creating piles or groups of things that are **EXACTLY** the same
* Needs one or more columns. If grouping by multiple columns separate with a comma
* Used in conjunction with aggregate functions:
 * Rows need to be sorted/grouped into piles before an aggregation function can be performed
 * Aggregate functions are like verbs, they do something
 * And like verbs, they also need a subject, i.e. a column(s)
* Before it can count, add things, average things, it must group things first. Unfortunately computers are not that smart and you must tell it to group things first
 * Coins: quarters, dimes, etc
 * Colors: black, blue, red, etc
* GROUP BY identifies uniques values within a column. Points out spelling errors, spelling inconsistencies, etc.
* California Daycare Data: Group by facility type

 ```
 SELECT facility_type
 FROM nicar_CAdaycares031714
 GROUP BY facility_type
 ```

* Query above show four unique values: Day Care Center, Day Care Center - ILL Center, Infant Center, School Age Day Care Center
* Group by facility type and county name. How many records are there?


### Wildcards
* Group strings together that have a common characteristic or pattern but are not necessarily identical
* % used to match any number of characters
* Wildcard must always use the operator LIKE
* I want to find all the records where the facility name begins with the word Little

 ```
 SELECT *
 FROM nicar_CAdaycares031714
 WHERE facility_name LIKE 'Little %' 
 ```

* What about Lil'?

### COUNT
* Once values have been grouped together, count to see how many are in each pile
* Goes in the SELECT statement

 ```
 SELECT column_name, COUNT(*)
 ```

* If you are using count, then you need to GROUP BY

 ```
 GROUP BY column_name
 ```

* Specify the column that's going to be counted in the SELECT statement. That same column needs to be in GROUP BY statement, too
* COUNT is a function, meaning it performs an action and it needs an argument - a column - to perform that function
 * It's written: COUNT()
 * Within the parenthesis, you want to use \* because that means everything, and you want to count everything
* How many total records in the table?

 ```
 SELECT COUNT(*)
 FROM nicar_CAdaycares031714
 ```
* How many daycares have a facility type of Infant Center? School Age Day Care Center?

 ```
 SELECT facility_type, COUNT(*)
 FROM nicar_CAdaycares031714
 GROUP BY facility_type
 ```

* If the column that's included in the SELECT statement isn't included in the GROUP BY statement you will be grouping all records into one pile, thus counting all records in the table

 ```
 SELECT facility_type, COUNT(*)
 FROM nicar_CAdaycares031714
 ```

* In Excel you click sort, in SQLite you ORDER BY

 ```
 SELECT facility_type, COUNT(*)
 FROM nicar_CAdaycares031714
 GROUP BY facility_type
 ORDER BY facility_type 

 OR
 
 SELECT facility_type, COUNT(*)
 FROM nicar_CAdaycares031714
 GROUP BY facility_type
 ORDER BY COUNT(*)
 ```

* First query orders by facility type, because it's text it orders from A-Z.
* Second query orders by the count, because it's numerical it orders from least to greatest
* Add DESC to go from Z-A or greatest to least
* Can also use numbers to substitute the column name: The first column in the SELECT statement is 1, the second column in the SELECT statement is 2, and so forth
* Column Aliases: Provide a name to column that you count, sum, avg, calculate, etc

 ```
 SELECT facility_type, COUNT(*) AS facility_count
 FROM nicar_CAdaycares031714
 GROUP BY facility_type
 ORDER BY 2 DESC
 ```


* COUNT(\*) vs COUNT(column_name)

 ```
 SELECT facility_administrator, COUNT(*)
 FROM nicar_CAdaycares031714
 GROUP BY facility_administrator
 ORDER BY 2 DESC
    
 SELECT facility_administrator, COUNT(facility_administrator)
 FROM nicar_CAdaycares031714
 GROUP BY facility_administrator
 ORDER BY 2 DESC
 ```

* The former includes 400 records where the facility administrator is null, the latter doesn't count the nulls
* Examples of where knowing the number of Nulls would be important?

* Which county has the most number of closed facilities?
* Which county has the highest number of facilities that contain the word Happy


### SUM
* SUM()-- Another function that requires parenthesis and a column name.
* Syntax:

 ```
 SUM(column_name)
 ```

* The importance of GROUP BY statement is seen in the SUM function
 * You need to group things together before you can add them
 * Think of money: you want to group the quarters together, then add them; group all dimes together, then add them
* Like count, the same column you are summing by, you are also grouping by
 
* In the following example, I want to know the number of inspections visits per county. Because I want to know per County, I will group by County
 
 ```
 SELECT county_name, SUM(inspection_visits)
 FROM nicar_CAdaycares031714
 GROUP BY county_name
 ORDER BY 2 DESC
 ```
 
* Which county has the largest daycare capacity? Least?
* Which county has the most type A violations? Least?


### Average
* Find the average: adds all the numbers in the set and then divides by the total number of quantities in the set
* Again, the GROUP BY statement is important because you need to group things that are exactly alike together, before you can calculate the average
* Average goes in the SELECT statement
 
 ```
 SELECT colmun_name, AVG(other_column_name)
 ```
 * the column name in the average function needs to be a numerical value
* It's a function, requires parenthesis and a column within those parenthesis
* Find the average number of inspection visits per county

 ```
 SELECT county_name, AVG(inspection_visits)
 FROM nicar_CAdaycares031714
 GROUP BY county_name
 ORDER BY 2 DESC
 ```
 
* Which city has the highest average of inspection visits?
* Which city has the highest average of type A violations?

### HAVING

* Acts like a filter - or the WHERE statement - for aggregate function

* How many counties have more 500 day cares

 ```
 SELECT county_name, COUNT(*)
 FROM nicar_CAdaycares031714
 GROUP BY county_name
 HAVING COUNT(*) > 500
 ORDER BY 2 DESC
 ```

* How many cities where the total amount of type A violation is less than 100?


### Math Across Columns
* This is equivalent to adding, subtracting, and writing formulas in excel
* when you perform math across columns grouping isn't as important. But it also depends
* The calculations go in the SELECT statement
* Subtraction: -

  ```
  SELECT (column_one - column_two)
  ```
* Addition: + 

  ```
  SELECT (column_one + column_two)
  ```
* Multiplication: \*

  ```
  SELECT (column_one * column_two)
  ```
* Division: /

  ```
  SELECT (column_one / column_two)
  ```

 * Division will result in whole numbers, as an INT. But that's not super helpful
 * To get a FLOAT, or the decimal, you want to first multiply the numerator by 1.0
  
  ```
  SELECT ((column_one * 1.0)/column_two)
  ```

* Recognizes PEMAS

* Find the total number of visits for each facility
 
 ```
 SELECT facility_name, (inspection_visits + complaint_visits + other_visits) AS total_visits
 FROM nicar_CAdaycares031714
 ORDER BY 2 DESC
 ```

* The which county has the highest percentage of total visits that lead to total number of Type A violations?

### Max and Min
* Works in a similar fashion as ORDER BY; the exception: where ORDER BY sorts every records from least to greatest or vise versa and returns every records in said order, MAX and MIN return the greatest record and least records, respectively. 
* Works on both numbers and text

 ```
 SELECT MAX(county_name)
 FROM nicar_CAdaycares031714
 ```

 * Because county_name column is text, it only shows the county name that begins with the last letter in the alphabet.
 
* You use MAX function on numbers too

 ```
 SELECT county_name, MAX(inspection_visits)
 FROM nicar_CAdaycares031714
 ```

 * The pitfall is that there are many records where the inspection visits is six.
  
  ```
  SELECT county_name, inspection_visits
  FROM nicar_CA
  WHERE inspection_visits = 6
  ```

* the difference between MAX and MIN and ORDER BY: the former returns one row, where as ORDER BY returns everything

  ```
  SELECT county_name, inspection_visits
  FROM nicar_CAdaycares031714
  ORDER BY inspection_visits DESC
  ```

  * Max and Min provide the ceiling and the floor a column



