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

Regular Expression Matching 

+ Abstraction: DFS.
+ How DFS works: For current character in pattern string, 
    1. If it is not the last char and the next char is not '\*' or it is the last char, which is the normal matching, we compare the current char of s and p. If the current char in p is not '.' and not equal to the current one in s, we return false, else we increase both pointers by 1 and recursively match from the pointer position. Also in this condition, we must make sure the pointer of s still inside.
    2. Else, the pointer of p doesn't reach the final char and the next char is '\*'. We recursively match the current char of s with the char behind '\*' in p. If match fails, we increase the pointer of s by 1, and do again. If it is fail matching until the pointer of s reaching end, increase the pointer of p by 2 and return the result of matching. 

Dungeon Game    

+ Abstraction: Backtracking. Optimized by Dynamic Programming   
+ Basic idea:   
    minInit[i][j] represents the minimum initial hp when start from (i, j) that can reach to target     
    minInit[i][j] = max(1, min(minInit[i + 1][j], minInit[i][j + 1]) - dungeon[i][j])   

Longest Substring Without Repeating Characters  

+ Abstraction: Double pointer + cache.
+ Longest substring quoted by double pointer. Use HashSet as dictionary to check duplication. If duplication find, update max, start remove element pointed by slow pointer from hashset just after the dup char. If no dup, add element pointed by fast pointer to hashset, increase fast pointer. Attention: you need to update max after the loop finished. Then you'll get the max length.  

Add Two Numbers 

+ Abstraction: add number digit by digit.   
+ You need carry, and the raw value is digit1 + digit2 + carry. The new digit is raw % 10, the carry is raw  / 10. After one list reach to end, you need continue calculate the unfinished list with carry. At last you need to check if carry is zero, if not , we still need to add one more digit.   

Longest Palindromic Substring   

+ Abstraction: O(n^2) pairwise substring search. Using dynamic programming cache palindrome judge table.
+ i from [o, length), j from [i - 1, 0]; If charAt(i) == charAt(j) and (i - j < 2 \|\| palin[i - 1][j + 1] == true), palin[i][j] = true; Then update max value, start and end.    

ZigZag Conversion   

+ Abstract: Index manipulation and string concatenation.    
+ Separate different rows and append chars to each row. You need a index variable and boolean direction variable. When direction is increasing, increase index and compare it with rows; if it equals to rows, then index = max(0, rows - 2). When direction is decreasing, decrease index and compare with 0; if it less than 0, then index = 1 < rows ? 1 : 0;    

Reverse Integer 

+ Abstract: Get each digit of an integer x. x % 10 get tail digit, then x / 10 ready for next one until x == 0.
+ Attention: Use long type store reversed integer. Use sign variable store the sign of x. After reversing, let reversed integer * sign, if the result is greater than largest Integer, or less than smallest integer, return 0. Else cast to int type return.   

String to Integer (atoi)    

+ Abstract: String manipulation and convert string to integer.
+ Rules: skip as much spaces as possible. Then if the first non-space char is neither sign nor digit, return 0. If it is sign, set hasSign to true, set start to index, and check next. If the next still sign, and hasSign is true, return 0. If next one is digit, and hasSign = false, set start to index, and break, else, direct break. Then we from the next position of start scan if some chars are not digit. If some chars are not digit, we break. Then we need to filter out if hasSign is true and end - start < 2, which is the sequence only contains a sign, we return 0.   
+ Parse: set index to 0, sign to 1; If first char is sign, we set sign again based on first char, and increase index; else, nothing changes. Use long type as raw cache and check: if sign is 1, then if raw >= Integer.MAX\_VALUE, return Integer.MAX\_VALUE; if  sign is -1, then if raw > Integer.MAX\_VALUE, return Integer.MIN\_VALUE. After parsing, sign multiplies raw and cast the result to int type. 

Palindrome Number   

+ Abstraction: Get digits from two side.    
+ First, we need to calculate magnitude order. Then we began to get digits from head and tail of the integer.  If they are not equal, then return false; else, continue until the number reach to zero. Head digit = integer / order, tail digit = integer % 10.    

Container With Most Water   

+ Abstraction: Double pointers, always shrink the smaller side. Calculate the area using the smaller height and the distance between the two lines. 

Integer to Roman        

+ Abstraction: Integer manipulation.    
+ Romans has different threshold, which matches different symbols. The count for corresponding symbol is calculated by num / threshold. After append the symbols, the number decrease to num % threshold.


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
