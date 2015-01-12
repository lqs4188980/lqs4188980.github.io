---
layout: post
title: "Leetcode Summary"
categories: Notes
---
Leetcode Summary
==================================

Algorithm
---------------------



<hr />
Database
---------------------

SQL join review:    

+ tableA INNER JOIN tableB ON tableA.column1 = tableB.column1
    will select the rows that tableA.column1 = tableB.column1 exactly.
    
+ tableA FULL OUTER JOIN tableB ON tableA.column1 = tableB.column1
    will select the rows that tableA.column1 = tableB.column1, or either tableA.column1 = null or tableB.column2 = null.
    
+ tableA LEFT OUTER JOIN tableB ON tableA.column1 = tableB.column1
    will select the all the rows in tableA. For the values that not in tableB, null will appear in the result
    
+ tableA CROSS JOIN tableB will produce two table's cartesian product.

Problems:       

+ Combine Two Tables:
> Write a SQL query for a report that provides the following information for each person in the Person table, regardless if there is an address for  each of those people.

    Apparently it is a left join for person table, because it requires for each person in person table, but regardless if there is an address for each of those people. 

        Answer:
        SELECT      Person.FirstName, 
                    Person.LastName, 
                    Address.City, 
                    Address.State 
        FROM        Person 
        LEFT JOIN   Address 
        ON          Address.PersonId = Person.PersonId;

+ Second Highest Salary
> For example, given the above Employee table, the second highest salary is 200. If there is no second highest salary, then the query should return null.

    The most tricky part I found is you must identify the empty set and return null if it is empty. You can use `select distinct Employee.Salary from Employee order by Salary desc limit 1, 1;` find out the second highest salary, however, you need to output as `select ifnull((select distinct Employee.Salary from Employee order by Salary desc limit 1, 1), null) as SecondHighestSalary;` so that the `ifnull` function can identify the empty set.

        Answer:
        select 
                IFNULL((
                    SELECT  DISTINCT Employee.Salary 
                    FROM    Employee 
                    ORDER BY Salary desc limit 1, 1
                ), null) 
        as SecondHighestSalary;

+ Nth Highest Salary    
    Almost the same as above.       

        Answer:
        CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
        BEGIN
        DECLARE M INT;
        SET M=N-1;
        RETURN (
            # Write your MySQL query statement below.
            select 
                ifnull((
                    select distinct Employee.Salary 
                    from Employee 
                    order by Salary desc limit M, 1
                ), null) 
            as SecondHighestSalary);
        END
    
