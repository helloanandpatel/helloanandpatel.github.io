---
layout: post
title:  "Welcome to Jekyll!"
date:   2015-09-15 00:29:03
categories: misc
---

Let S be the set of all elements and R be the relation defined on it. So, for every pair of elements in a,b E S, a R b is either true or false. If it is true, we say that a and b are related, otherwise they are not related. A relation R is called an equivalence relation if it satisfies the following properties:

	1.	Reflexive - for every element a E S, a R a is true.
	2.	Symmetric - for any two elements a,b E S, if a R b is true then b R a is true.
	3.	Transitive - for any three elements a,b,c E S, if  a R b and b R c is true then a R c is true.

Examples of non-equivalence relations
	1.	S = { students in a class }, R = “is friend of”
	◦	Transitive property does not hold true here. If Nandan is friend of Adarsh and Adarsh is friend of karan, that does not mean that Nandan and Karan are also friends. They might or might not be friends. 
	2.	S = { people in a wedding }, R = “knows”
	◦	Symmetric property does not hold true here. I and Mukesh Ambani attends same wedding. I know Mukesh Ambani but he does not know me.

Examples of equivalence relations
	1.	S = { people in the world }, R = “is in same country”.
	2.	S = { cities in a state }, R = “is connected to”.
	3.	S = { students in a class }, R = “is sibling of”.

When you have equivalence relation, the set of all elements gets partitioned into number of disjoint (non-overlapping) sets called equivalence classes. Because of transitive property of equivalence relation, there exist equivalence classes which partitions all elements into multiple disjoint sets.

If S1, S2, ….,Sn are equivalence classes of elements of set S, then following are some properties of equivalence classes:

	1.	S = S1 U S2 U…….. U Sn (each element belongs to only one class)
	2.	null = S1 intersection S2 intersection ….. intersection Sn (because equivalence classes are non-overlapping)

Since the intersection of two equivalence classes is empty, the equivalence classes are sometimes called disjoint sets.To decide if a R b, we just need to check whether a and b are in the same equivalence class or not.

In computer science, a disjoint set data structure is a data structure that tracks a set of elements partitioned into a number of disjoint (non-overlapping) subsets - Wikipedia.

 Equivalence classes can be implemented using disjoint sets data structure. Usually, there is a need to perform following operations on disjoint sets data structure:

	1.	There is a need to merge two disjoint sets into one because of transitive property. This is also called as “Union” or “Merge”
	2.	Find the disjoint set to which an element belongs to. This is also called as “Find”. 

“Union-Find” and “Merge-Find” are another names for disjoint set data structure. Disjoint set data structure can be used to solve many problems where there is a concept of equivalence relation and equivalence classes.

API for UnionFind
class UnionFind {
    public void union(int i, int j); //merges the disjoint set of i with disjoint set of j
    public int find(int i); //returns the disjoint set of i
}

Conceptual
Invocation
State
-
{1}, {2}, {3}, {4}, {5}, {6}, {7}, {8}, {9}, {10}
union(3,9)
{1}, {2}, {3, 9}, {4}, {5}, {6}, {7}, {8}, {10}
union(1,4)
{1, 4}, {2}, {3, 9}, {5}, {6}, {7}, {8}, {10}
union(4,2)
S1={1, 4, 2}, S2={3, 9}, S3={5}, S4={6}, S5={7}, S6={8}, S7={10}
find(2)
S1

UnionFind - Array Based Implementation (aka quick find)
In this, an array of size n is initialized using a[i] = i, where i=1,2,3,…,n. Each element starts in its own disjoint sets. The sets keep on merging due to union operation.

class UnionFind {
    private int arr[];

    public UnionFind(int n) {
        //Start with n disjoint sets. Each element belongs to a separate disjoint set
        //a[0] is a waste. We want to avoid mistakes that can occur while calculating subscripts.
        arr = new int[n+1];
        for (int i = 0; i <= n; i++)
            arr[i] = i;
    }

    //O(n) - as it iterates over the array
    public void union(int i, int j) {
        int iSet = find(i); //find the set to which i belongs
        int jSet = find(j); //find the set to which j belongs
        if (iSet == jSet)
            return;

        for (int a = 1; a < arr.length; a++)
        {
            //Elements which are in the set of i, change them to the set of j,
            //giving a feeling of merge operation
            if (arr[i] == iSet)
                arr[i] = jSet;
        }
    }

    //O(1)
    public int find(int i)
    {
        return arr[i];
    }
}

Above Algorithm
Invocation
State
-
arr index = 0 1 2 3 4 5 6 7 8 9 10 (n = 10)
arr value = 0 1 2 3 4 5 6 7 8 9 10
union(3,9)
arr index = 0 1 2 3 4 5 6 7 8 9 10
arr value = 0 1 2 9 4 5 6 7 8 9 10
union(1,4)
arr index = 0 1 2 3 4 5 6 7 8 9 10
arr value = 0 4 2 9 4 5 6 7 8 9 10
union(4,2)
arr index = 0 1 2 3 4 5 6 7 8 9 10
arr value = 0 2 2 9 2 5 6 7 8 9 10
find(2)
2


Quick-Union - Forest based implementation using Arrays


