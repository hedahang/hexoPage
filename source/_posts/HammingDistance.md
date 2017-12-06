---
title: Hamming Distance
date: 2016-09-03 20:28:10
categories: JS 算法
tags:
	- JS
	- 算法
---

<h1 style="color:#2dbe60"> Hamming Distance </h1>
The <a href="https://en.wikipedia.org/wiki/Hamming_distance" target="_blank" style="box-sizing: border-box; color: rgb(0, 136, 204); text-decoration: none; background-color: transparent;">Hamming distance</a> between two integers is the number of positions at which the corresponding bits are different.
Given two integers x and y, calculate the Hamming distance.
**Note:**
``` javascript
0 ≤ x, y < 2^31.
```
**Example:**
``` flow
Input: x = 1, y = 4

Output: 2

Explanation:
1   (0 0 0 1)
4   (0 1 0 0)
       ?   ?

The above arrows point to positions where the corresponding bits are different.
```
**代码如下:**
``` javascript
/**
 * @param {number} x
 * @param {number} y
 * @return {number}
 */
var  hammingDistance = function(x, y) {
         var z = x^y;
         z = z.toString(2).split("");
         var result = 0;
         z.forEach(function(val){
              if(val === '1'){
                    result++;
              }
         })
         return result;
};
```
-------------------

