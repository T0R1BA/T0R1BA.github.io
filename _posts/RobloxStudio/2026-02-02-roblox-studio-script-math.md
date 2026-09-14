---
title: "로블록스 스튜디오 스크립트: math"
categories: RobloxStudio
# excerpt: ""
---




# 📌 round, floor, ceil
{: .notice}

<span class="color-keyword">math.round(number)</span> Rounds the number to the nearest whole number

<span class="color-comment">math.round(1.5) -- Result : 2</span>

<br>

<span class="color-keyword">math.floor(number)</span> Returns the largest integer less than or equal to the given value

<span class="color-comment">math.floor(11.99) -- Result : 11</span>

<br>

<span class="color-keyword">math.ceil(number)</span> Returns the smallest integer greater than or equal to x

<span class="color-comment">math.ceil(11.01) -- Result : 12</span>



# 📌 abs, sqrt, pow
{: .notice}

<span class="color-keyword">math.abs(number)</span> Returns the absolute value of the number

<span class="color-comment">math.abs(-10) -- Result : 10</span>

<br>

<span class="color-keyword">math.sqrt(number)</span> Returns the root of the number

<span class="color-comment">math.sqrt(100) -- Result : 10</span>

<br>

<span class="color-keyword">math.pow(number, exponent)</span> Returns the first number to the power of the second

<span class="color-comment">math.pow(2, 10) -- Result : 1024</span>




# 📌 deg, rad
{: .notice}

<span class="color-keyword">math.deg(number)</span> Converts Radians to Degrees

<span class="color-comment">math.deg(math.pi) -- Result : 180</span>

<br>

<span class="color-keyword">math.rad(number)</span> Converts Degrees to Radians

<span class="color-comment">math.rad(180) -- Result : 3.141592653589793</span>




# 📌 min, max
{: .notice}

<span class="color-keyword">math.min(number, ...)</span> Returns the smallest value

<span class="color-comment">math.min(3, 5, 1, 10) -- Result : 1</span>

<br>

<span class="color-keyword">math.max(number, ...)</span> Returns the largest value

<span class="color-comment">math.max(3, 5, 1, 10) -- Result : 10</span>




# 📌 random, randomseed
{: .notice}

<span class="color-keyword">math.random(min, max)</span> Returns a random number between first and last

<br>

<span class="color-keyword">math.randomseed(number)</span> Sets the seed for the random number generator
