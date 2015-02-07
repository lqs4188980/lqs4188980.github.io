---
layout: post
title: "Dynamic Programming Summary"
categories: Notes
---	

Dynamic Programming Summary
==============================

The problems that can be solved using Dynamic Programming should have two properties:

1. The original problem can be divided into subproblems, and the solution to subproblems have overlapping.	
2. We can find an optimal substructure that can build up a solution using solved subproblems.	



Longest Common Subsequence Problem	
-------------------------------------------------	
For two sequence An and Bn, 	

+ If A[n] == B[n], then LCS(An, Bn) = LCS(An-1, Bn-1) append A[n] or B[n].	
+ If A[n] != B[n], then LCS(An, Bn) = max(LCS(An-1, Bn), LCS(An, Bn-1))	

[Reference](http://en.wikipedia.org/wiki/Longest_common_subsequence_problem).	

Ugly Numbers	
-------------------------	
[Source](http://www.geeksforgeeks.org/ugly-numbers/)
> Ugly numbers are numbers whose only prime factors are 2, 3 or 5. The sequence 
`1, 2, 3, 4, 5, 6, 8, 9, 10, 12, 15, …`	
shows the first 11 ugly numbers. By convention, 1 is included.	
Write a program to find and print the 150’th ugly number.	

Next Ugly Number = min(lastMultipleOfTwoUglyNum, lastMultipleOfThreeUglyNum, lastMultipleOfFiveUglyNum).	

Maximum Size square sub-matrix with all 1s	
----------------------------------------------------	
[Source] (http://www.geeksforgeeks.org/maximum-size-sub-matrix-with-all-1s-in-a-binary-matrix/)

Assume s[i][j] is the maximum square sub-matrix that the rightmost and bottommost at M[i][j].	

	s[i][j] = {
		if M[i][j] == 0, 0;
		if M[i][j] == 1, min(s[i][j - 1], s[i - 1][j], s[i - 1][j - 1]) + 1;
	}

Fibonacci number	
---------------------------	

Dynamic Programming with rolling array.

	loop:
		f3 = f1 + f2;
		f1 = f2;
		f2 = f3;

Longest Increasing Subsequence	
----------------------------------------	
[Source](http://www.geeksforgeeks.org/dynamic-programming-set-3-longest-increasing-subsequence/)	

Assume LIS(i) is the length of longest increasing subsequence that end at arr[i].

	LIS(i) = {
		if exists j that j < i && arr[j] < arr[i] && LIS(j) is the max among [LIS(0)...LIS(i - 1)], 1 + LIS(j);
		else, 1;
	}

Edit Distance	
---------------------	

Assume d[i][j] is the minimum edit distance between A[0...i] and B[0...j].	

	d[i][j] = {
		if (A[i] == B[i]), d[i-1][j-1];
		else, min(d[i-1][j] + 1, d[i][j-1] + 1,d[i-1][j-1] + 1)
	}		


Min Cost Path	
-----------------------	

[Source](http://www.geeksforgeeks.org/dynamic-programming-set-6-min-cost-path/)

Given a cost matrix and a position (m, n), find out the minimum cost path to reach (m, n) from (0, 0). There are three ways allow you to go: down, right and diagonal to lower right.

Optimal Substructure: 

	minimum(m, n) = min(minimum(m - 1, n), minimum(m, n - 1), minimum(m - 1, n - 1)) + cost[m][n]	

It has overlapping Subproblems, so it can use Dynamic Programming	

Length of the longest substring without repeating characters	
-------------------------------------------------------------------------	

Maintain a substring window that don't have repeating characters, and record the last position of each character. If the current character haven't been accessed or the last position is out of current window, then enlarge the window and record this position to corresponding slots, update the max length at the same time.