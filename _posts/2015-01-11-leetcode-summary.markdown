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

Roman to Integer	

+ Abstraction: From digit to integer. 	
+ There are only one condition: current literal-value less than the next one, if it is not the last literal. You need use the latter value subtract the previous one.	

Longest Common Prefix	

+ Abstraction: String manipulation	
+ Compare a letter with all the string in the same position. If one of the string is null or length is 0, return "". If the position is out of the string or there are different letter, return substring(0, position).	

3Sum	

+ Abstraction: utilizing n - 1 sum to build n sum.	

3Sum Closest	

+ Abstraction: Lower the order to 2Sum, and find out 2Sum Closest.	
+ Sort the array first. Pick one element, and start find 2Sum Closest to target - num[i], if distance less than current minimum distance, update min and sum.	

4Sum	

+ Abstraction: subtract from target and decrease the number of combination to 2Sum	

Letter Combinations of a Phone Number	

+ Abstraction: Search every route. O(3^L), L is the length of digits.	

Remove Nth Node From End of List	

+ Abstraction: Double pointer start from head. The right pointer needs proceed n - 1 step at first, then the two pointer proceed at the same time until right pointer reach the final node, the left pointer is the node that needs to be deleted.	

Generate Parentheses	

+ Abstraction: Search every route. 	
+ There is a limitation: The number of '(' should >= ')'. When the number of '(' + number of ')' == 2n, add current s to result.	

Merge k Sorted Lists	

+ Abstraction: Divide and Conquer	

Swap Nodes in Pairs	

+ Abstraction: Find out operation block, for this problem, the operation block is prev -> l -> r -> n, l and r are the nodes that need to swap. r cannot be null. After swap, prev = l, l = n, r = n.	

Reverse Nodes in k-Group	

+ Abstraction: Find out operation block, use a flag and prev. reverse each pair of node. Generally need l, r, n. Notice that prev.next.next = flag, and prev.next should be the new prev. But before assign prev.next = l, we need a variable temp to store prev.next. Finally let l = flag.	

Remove Duplicates from Sorted Array	

+ Abstraction: Something same as insertion sort. A pointer point to the next waiting element, a cache variable for decide duplicate, a pointer point to current position waiting to fill. If waiting element is equal to cache, then jump to next one. Else, put them into current position waiting to fill, both waiting position and waiting element jump to next.	

Remove Element	

+ Abstraction: Two pointer, scan from head and tail. The right of the tail pointer is the block that contains disposed elements. The head pointer is pointing to the element that waiting to judge. Every time we found head pointer's element equals to elem, we put tail pointer's element to that position, shrink tail pointer by 1 and judge again.	

Implement strStr()	

+ Abstraction: string pattern matching.	
+ You should finish brute force method in a very short time. And know Rabin-Karp algorithm, KMP algorithm, and the Boyer-Moore algorithm.	

Divide Two Integers	

+ Abstraction: Use shift, increase divisor to approach dividend as much as possible, then dividend subtract the approached value, then the shift times is the quotient.	Must be very careful about overflow problem. When calculating quotient, just use long store and return. When return the result, first check if it should be negative, if it is negative, ~value + 1. Then if  the value greater than MAX_VALUE, return MAX_VALUE; if it is less than MIN_VALUE, return MIN_VALUE.	

Substring with Concatenation of All Words	

+ Abstraction: Use HashMap as checker and for each specific length, check each word if it exist in the HashMap. If it exists, update counter and count info at "found" map. If not, continue to next start and restart the process.	

Next Permutation	

+ Abstraction: Find the first digit j that larger than its previous one i. Then sort from that position j to end. Then pick one that is larger than i, swap these two integer and return. Else, sort the whole array and return.	

Longest Valid Parentheses	

+ Abstraction: Every time we pop a '(' from stack means we found an valid parentheses, we can update length. The length is :
	1. If the stack is empty, then the length is current loop index + 1.
	2. If the stack is not empty, the length is current loop index - stack.peek()	

+ So we need a stack to store each index of '('. Every time we encounter a ')', we check if the stack is empty and if the top of stack is the index point to '(', if true, we pop one from stack, and update length. If not true, we push this index into stack.	

Search in Rotated Sorted Array	

+ Abstraction: Binary Search	
+ Have to find ordered part: A[l] <= A[mid] or A[mid] <= A[r]. Then if A[l] <= A[mid], and target >= A[l] and target < A[mid], r = mid - 1, else l = mid + 1; If A[l] > A[mid], means A[mid] <= A[r], then if target > A[mid] and target <= A[r], l = mid + 1, else r = mid - 1.	

Search for a Range	

+ Abstraction: Binary Search	
+ Two methods: 
	1. Recursion: If A[mid] == target, update lower bound and upper bound, then search on the both side to update bounds. If not, only search the possible side to update bounds.	
	2. Binary search for lower bound and upper bound.	

Search Insert Position	

+ Abstraction: Binary Search for right position	
+ Normal binary search until exit the loop. If we found A[mid] == target in the loop, return mid. After exit loop, if A[mid] < target, then we know target should insert after mid; if A[mid] > target, then we know target should insert at mid.	

Valid Sudoku	

+ Abstraction: Loop the correct area.	
+ For square validation, you should specify starting row index and column index. You cannot just specify a single index.	

Sudoku Solver	

+ Abstraction: DFS	
+ Find empty slot first, then scan current row and column and square of empty slot, get a table of all the digits that already filled, then find a digit that hasn't been used and fill it into board, send the filled board to next level, until some level that cannot find an empty slot, return that board. If the returned board is null, which means current digit doesn't work, then fill the next one and try again, until find a solution.	

Count and Say	

+ Abstraction: Use a queue to track the sequence. The last output is the next input.	

Combination Sum

+ Abstraction: Deep search, until reach target.	
+ This problem is based on set. No duplicate numbers, but we can pick the same number many times. For each deeper level, we didn't increase the start index so that we can continue add the same element again and again. Because the set doesn't contain duplicate elements, so we don't need signature to check duplicates.	

Combination Sum II	

+ Abstraction: Same as above. Just added duplicate checker and increase start index for every deeper level.	

Largest Number	

+ Abstraction: String sorting. Get a total order of strings. 	
+ Write a comparator, if s1 equals to s2, return 0; else return -(s1 + s2).compareTo(s2 + s1). Need to pay attention to all zeros condition.	

First Missing Positive	

+ Abstraction: Hashing	
+ Utilize the array index nature, hash every positive integer that between 1 to A.length, into A[integer - 1]. For integer out of this range, simply let them stay where they are.	

Trapping Rain Water	

+ Abstraction: Cache + divide & conquer.	
+ Scan from left to right, find out the max left for each position. Then scan from right to left and calculate how many water can be trapped at this point. The trap area at some point is max(0, min(left[i], right[i]) - A[i]).	

Multiply Strings	

+ Abstraction: Multiply digit by digit. Pay attention to carry and preceding zeros.		

Wildcard Matching	

+ Abstraction: Greedy	
+ Every time we meet a '\*', record the position of last '\*' and position of p1, then match start from behind star with current p1, and set prevStar is true. Then if match(symbol equality or question mark), increase both pointer and set prevStar to false. If not match: if last star position is -1, which means no star in pattern, return false; else return pattern pointer to star and return sequence pointer to cached p1 position plus one and continue matching.	When p1 going to the end of sequence, we check remaining pattern is all the star, if true, then return true, else return false.	

Jump Game II	

+ Abstraction: Greedy	
+ For each l to r range, find out maximum position that r can reach. If scanner pointer reach r, then update l to r + 1, r to max and start scan from l.	

Permutations	

+ Abstraction: Mathematic	
+ We build new permutation by add the new element to available insert point of each old permutation.	

Rotate Image	

+ Abstraction: Array manipulation	
+ Use start and end set boundary, use i and j to track 	

Anagrams	

+ Abstraction: Anagram basic usage. As a signature(key).	

Pow(x, n)	

+ Abstraction: Divide & Conquer. 	
+ Divide pow(x, n) into two pow(x, n / 2); Assume, v = pow(x, n / 2), if n % 2 == 0, return v * v; else, return v * v * x.	

N-Queens	

+ Abstraction: back-tracking	
+ isValid: same column cannot have queen; for i = 0; i < row, Math.abs(i - row) != Math.abs(checker[i]  - col).	

Maximum Subarray 	

+ Abstraction: Dynamic Programming	
+ maxEndingHere = Math.max(maxEndingHere + A[i], A[i]); maxSofar = Math.max(maxEndingHere, maxSofar)	

Jump Game	

+ Abstraction: Same as Jump Game II. Greedy Strategy	

Merge Intervals	

+ Abstraction: A little bit like merge step in merge sort.	
+ Sort the intervals based on interval start, then merge the overlapped intervals one by one.	

Insert Interval	

+ Abstraction: Similar to Merge Intervals.	
+ Do a small insertion sort first, insert newInterval to a correct position. Then start merge like merge intervals.	

Length of Last Word	

+ Abstraction: Decide if current status is in a word.	

Spiral Matrix II	

+ Abstraction: Set up top, down, left, right four limitations and iterate.	

Permutation Sequence	

+ Abstraction: Like memory address calculation, group by group. Append each number indexed by group number to the result.	

Rotate List	

+ Abstraction: Connect the list tail to head, and find the relative rotation number by n % count, then move head to (count - n % count)th node, then detach it's previous node.

Unique Paths	

+ Abstraction: pathCount[i][j] = pathCount[i-1][j] + pathCount[i][j - 1]	

Minimum Path Sum	

+ Abstraction: costs[i][j] = min(costs[i - 1][j], costs[i][j - 1]) + grid[i][j];	

Merge Two Sorted Lists	

+ Abstraction: Merge step in MergeSort

Repeated DNA Sequences	

+ Abstraction: Search, caching, hash.	
+ Basically, we should count how many appearance for each substring at length 10 in the whole string. However, we will count many strings again and again, so we need a set to store all the substring that has been searched. Then, what we care about is the substring that appear more than once, so we do not need to count how many appearance for each substring, we use another set to store all the substrings that appear twice. If it in the set that appear once but not in the set that appear twice, then add it to the result. 	
	We need utilize hash to compress substring into an integer. For DNA sequences, there are four units: A,C,G,T, which can fit into 00, 01, 10, 11. So we shift left an integer and do OR operation with the corresponding value of the unit, then we can get a hash value for a substring of length 10.


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