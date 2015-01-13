---
layout: post
title: "Leetcode Summary"
categories: Notes
---
Leetcode Summary
==================================

Algorithm
---------------------
Two Sum 

+ Basic idea: for each item, iterate the item behind it and calculate the sum, O(n^2)
+ Use cache: We have wasted too much time to calculate every possible combination. Actually we just need to know, for each number in the array, each item sum to which number can get the target. So we need to cache the missing number. Because it is one-to-one mapping, the best way to cache is HashTable. For quick search, we should use the missing number as key and the item as value. 
    Then We iterate over the array, and if the item is in the table, then we have found the pair that can sum to target number; if not, we use (target - item) as key and item as value, put them into table, then iterate to next. 

Median of Two Sorted Arrays

+ Basic idea: We need to find the median of two sorted arrays, we can merge them together and find out the median. The merge step takes O(m + n), and we can find out in O(1) time, which the total worst case time is O(m + n) 
+ Actually they are all sorted, and we only need to find out the median. Then merge them all together is meaningless. We can find the median of each array in O(1) time. However, We still have no idea about the real median of the two arrays.    
    The definition of median is:
    
    > If there is an odd number of data values then the median will be the value in the middle. If there is an even number of data values the median is the mean of the two data values in the middle.  
    
    Assume there are two sorted array A and B. The median index of A is Ai, and the median index of B is Bj. And The length of A is m, and the length of B is n. The real median of these two array will be the (m + n) / 2-th element, if the total length is odd, or ((m + n) / 2 + (m + n) / 2 + 1) / 2.0, when the total length is even. There are i elements before Ai in A, and j elements before Bj in B. If (i + j) >= (m + n) / 2, if A[Ai] > B[Bj], then we can drop the part in A that greater than A[Ai]; if A[Ai] > B[Bj], then we drop the part in B that greater than B[Bj]. If (i + j) < (m + n) / 2, if A[Ai] > B[Bj], then we drop the part in B that less than B[Bj], also we need to shrink the median to (m + n) / 2 - the length of dropped part. More detailed explanation: [leetcode之 median of two sorted arrays][1]



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
    

[1]: http://blog.csdn.net/yutianzuijin/article/details/11499917 "leetcode之 median of two sorted arrays"
