---
layout: post
title: "Algorithms Need Better UI"
date: 2013-08-12 09:34
comments: true
categories: [Algorithm, UI]
---

Since 2006, as I became a professional software engineer, I spent a fair amount of time reading books and online articles about software. Most topics were around Object Orientation, Web technologies, Software design, Testing, Automation, Agile and Lean, etc. Last year, I decided to take a break from such topics on purpose. So, I cleared all my RSS subscriptions.

Instead, I thought I would revisit some of the algorithms that I learned during my undergraduate courses. It's been a few years since I studied algorithms and I thought I was more seasoned to appreciate and solve some of the algorithm problems than in the past. So, I started with the dynamic programming problems such as: [Longest Common Subsequence (LCS)](http://en.wikipedia.org/wiki/Longest_common_subsequence_problem) and [Knapsack Problem](http://en.wikipedia.org/wiki/Knapsack_problem) and found this solution in Wikipedia:

```js Source code for finding the lenght of the Longest Common Subsequence
function LCSLength(X[1..m], Y[1..n])
    C = array(0..m, 0..n)
    for i := 0..m
       C[i,0] = 0
    for j := 0..n
       C[0,j] = 0
    for i := 1..m
        for j := 1..n
            if X[i] = Y[j]
                C[i,j] := C[i-1,j-1] + 1
            else
                C[i,j] := max(C[i,j-1], C[i-1,j])
    return C[m,n]
```

As a software developer, when I'm reading an article, if there is an accompanied source code, my eyes automatically scroll into it skipping any textual blurb. It was the same in this case, and I must say, I noticed a few things here:

1. The single literal variable names need some love.
2. The code can use some logical grouping and naming to easily communicate what's happening here.

I found this to be an UI problem. The algorithm itself is quite complicated for an average person to understand. However, we can probably reduce some noise with better naming/grouping. Here's an alternate version of the same code.

```js Modified source code, with descriptive names
function LCSLength(sequence1[1..sequence1Size], sequence2[1..sequence2Size])

    table = GetTableWithZerosInFirstRowAndColumn(sequence1Size, sequence2Size)

    for sequence1Index := 1..sequence1Size
        for sequence2Index := 1..sequence2Size

            if sequence1[sequence1Index] = sequence2[sequence2Index]
				IncrementLength(table, sequence1Index, sequence2Index)
            else
				UseCurrentLength(table, sequence1Index, sequence2Index)

    return table[sequence1Size, sequence2Size]

function GetTableWithZerosInFirstRowAndColumn(columns, rows)
  table = array(0..column, 0..rows)

  InitializeFirstRowWithZeros(table)
  InitializeFirstColumnWithZeros(table)

  return table

function InitializeFirstRowWithZeros(table[columns x rows])
  for columnIndex := 0..columns
       table[columnIndex, 0] = 0

function InitializeFirstColumnWithZeros(table[columns x rows])
  for rowIndex := 1..rows
       table[rowIndex, 0] = 0

function IncrementLength(table, columnIndex, rowIndex)
    table[columnIndex,rowIndex] := table[columnIndex-1,rowIndex-1] + 1

function UseCurrentLength(table, columnIndex, rowIndex)
    leftCell = table[columnIndex-1,rowIndex]
    topCell = table[columnIndex,rowIndex-1]
    table[columnIndex,rowIndex] := max(leftCell, topCell)

```

I know this modified code is verbose, but I find it self explanatory. Using descriptive names are not a new concept in software engineering at all. I wish we had our algorithm books with annotated source code like this, where it's readable by humans.

Let's use uglifiers and minifiers to do the machinification for us. 