---
title: "assignment 6"
author: Zheni Huang
date: November 5,2018
output:
  html_document:
    keep_md: true
    toc: true
    toc_depth: 2
    theme: readable
---


# Writing Functions


```r
suppressPackageStartupMessages(library(gapminder))
suppressPackageStartupMessages(library(tidyverse))
suppressPackageStartupMessages(library(ggplot2)) 
suppressPackageStartupMessages(library(dplyr))
suppressPackageStartupMessages(library(MASS))
suppressPackageStartupMessages(library(singer))
suppressPackageStartupMessages(library(ggmap))
suppressPackageStartupMessages(library(singer)) 
```



# Task 1 Character Data

In this task, I have read and worked the exercises in the __Strings chapter__ or R for Data Science.

First we will load the library needed for this exercise


```r
suppressPackageStartupMessages(library(tidyverse))
library(stringr)
```

## 14.2.5 Exrercise 

1. In code that doesn’t use stringr, you’ll often see `paste()` and `paste0()`. What’s the difference between the two functions? What stringr function are they equivalent to? How do the functions differ in their handling of NA?

We try to see the difference of these two functions by simple examples.

```r
paste("Merry","Christmas")
```

```
## [1] "Merry Christmas"
```

```r
paste0("Merry","Christmas")
```

```
## [1] "MerryChristmas"
```
We can clearly see that if we use `paste()` to combine two strings, it will return a combined string with a space in between each string, while using `paste0()` return a combined string with no space in between.

The `paste0()` is equavalent to the `str_c()` function, we can prove this by the following example:
we first use the function `paste0()` and also look at the structure of the result.

```r
(ex1 <- paste0("hello","world"))
```

```
## [1] "helloworld"
```

```r
str(ex1)
```

```
##  chr "helloworld"
```
Then we perform the same thing with `str_c()`, which return the same result with the same structure.

```r
(ex2 <- str_c("hello","world"))
```

```
## [1] "helloworld"
```

```r
str(ex2)
```

```
##  chr "helloworld"
```

To get the equavalent results with the function `paste()`, we can perform it with the `str_c()` function again:
Here is the `paste()`

```r
(ex3 <- paste("hello","world"))
```

```
## [1] "hello world"
```

```r
str(ex3)
```

```
##  chr "hello world"
```
And now compare with the `str_c()`, separated with a space

```r
(ex4 <- str_c("hello","world", sep = " "))
```

```
## [1] "hello world"
```

```r
str(ex4)
```

```
##  chr "hello world"
```

Handle with NA:

While using `paste()` and `paste0()`, in this case it returns a string with the NA append to it.

```r
paste("hello","world",NA)
```

```
## [1] "hello world NA"
```

```r
paste0("hello","world", NA)
```

```
## [1] "helloworldNA"
```
If we put the NA at the front of the string, it has the similar results

```r
paste(NA,"hello","world")
```

```
## [1] "NA hello world"
```

```r
paste0(NA,"hello","world")
```

```
## [1] "NAhelloworld"
```

For `str_c()` function, the result is shown below:

```r
str_c("hello","world", NA,sep = " ")
```

```
## [1] NA
```

```r
str_c(NA,"hello","world", sep = " ")
```

```
## [1] NA
```
If there is one string which is NA, then the `str_c()` function will return the NA.



2. In your own words, describe the difference between the `sep` and `collapse` arguments to `str_c()`
In brief, `collapse` is used to combined vectors of strings while `sep` is used to combined two or more strings. This can be better illustrated by an example.

```r
str_c(c("a", "b", "c"), collapse= "&")
```

```
## [1] "a&b&c"
```

```r
str_c("a","b","c",sep= ",")
```

```
## [1] "a,b,c"
```
we can also use `collapse` and `sep` together:
by repeating the string `apple` and combined it with each element in the vector.

```r
str_c(c("I","He","She"), "apple",  sep= " ate ", collapse = " & " )
```

```
## [1] "I ate apple & He ate apple & She ate apple"
```

3. Use str_length() and str_sub() to extract the middle character from a string. What will you do if the string has an even number of characters?
we can use some simple maths to achieve this:
for odd number:

```r
num <- str_length("paparazzi")
str_sub("paparazzi",ceiling(num/2),ceiling(num/2))
```

```
## [1] "r"
```
for even number:

```r
num1 <- str_length("baseball")
str_sub("baseball",(num1/2), (num1/2+1))
```

```
## [1] "eb"
```
This will return the middle 2 characters.


## 14.3.1.1 Exercises

1. Explain why each of these strings don’t match a \: "\", "\\", "\\\".
To explore this, we can look at the below example, so a single "\" don't match since a single backslash symbol is used to escape special behaviour.
Since \ is used as the escape character, then first we need to use a \ to escape it to form a regular expression \\. Then if we create a string we need to use the escape \ again. So overall we will need 4 backslashs


```r
str_view(c("abc", "a.c", "a\\c","\\"), "\\\\")
```

<!--html_preserve--><div id="htmlwidget-406bb12557a063faf0a4" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-406bb12557a063faf0a4">{"x":{"html":"<ul>\n  <li>abc<\/li>\n  <li>a.c<\/li>\n  <li>a<span class='match'>\\<\/span>c<\/li>\n  <li><span class='match'>\\<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->
so in breif, `\\` for the normal expression `\`, then extra `\` to escape the special function, then last `\` used to create a new string, then overall 4 `\`.

2.How would you match the sequence "'\?
first we create a string with this sequence, then we try to match it:

```r
x <- "\"\'\\"
writeLines(x)
```

```
## "'\
```

```r
str_view(x, "\\\"\\'\\\\") 
```

<!--html_preserve--><div id="htmlwidget-416a8fcca98ef705a7a0" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-416a8fcca98ef705a7a0">{"x":{"html":"<ul>\n  <li><span class='match'>\"'\\<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->
the first three backslash is to escape the special behabiour of " and create a string, then middle 2 is for the string `'` and the last 4 backslashs are used to create `\`.

## 14.3.2.1 Exercises

1, How would you match the literal string "$^$"?
again, first we create the string: ```
each `\\` is used to create the string and escape its special functions.

```r
x1 <- "$^$"
writeLines(x1)
```

```
## $^$
```

```r
str_view(x1, "\\$\\^\\$")
```

<!--html_preserve--><div id="htmlwidget-2990980eb833931bb50b" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-2990980eb833931bb50b">{"x":{"html":"<ul>\n  <li><span class='match'>$^$<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

2. Given the corpus of common words in stringr::words, create regular expressions that find all words that:

* Start with “y”.


```r
str_view(stringr::words, pattern = "^y", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-47d4320fc32cb5175749" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-47d4320fc32cb5175749">{"x":{"html":"<ul>\n  <li><span class='match'>y<\/span>ear<\/li>\n  <li><span class='match'>y<\/span>es<\/li>\n  <li><span class='match'>y<\/span>esterday<\/li>\n  <li><span class='match'>y<\/span>et<\/li>\n  <li><span class='match'>y<\/span>ou<\/li>\n  <li><span class='match'>y<\/span>oung<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

* End with “x”


```r
str_view(stringr::words, pattern = "x$", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-9b24cd9904785d6a3280" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-9b24cd9904785d6a3280">{"x":{"html":"<ul>\n  <li>bo<span class='match'>x<\/span><\/li>\n  <li>se<span class='match'>x<\/span><\/li>\n  <li>si<span class='match'>x<\/span><\/li>\n  <li>ta<span class='match'>x<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

* Are exactly three letters long. (Don’t cheat by using str_length()!)

```r
# since there are too many word which satisfy this condition, we are going to show only some of them
str_view(stringr::words[1:50], pattern = "^.{3}$", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-ae2dbaea44e5da4221b2" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-ae2dbaea44e5da4221b2">{"x":{"html":"<ul>\n  <li><span class='match'>act<\/span><\/li>\n  <li><span class='match'>add<\/span><\/li>\n  <li><span class='match'>age<\/span><\/li>\n  <li><span class='match'>ago<\/span><\/li>\n  <li><span class='match'>air<\/span><\/li>\n  <li><span class='match'>all<\/span><\/li>\n  <li><span class='match'>and<\/span><\/li>\n  <li><span class='match'>any<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

* Have seven letters or more.


```r
# this can be used to return certain length of strings
str_view(stringr::words[1:50], pattern = "^.{4,7}$", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-c4653236c44113e8b3a7" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-c4653236c44113e8b3a7">{"x":{"html":"<ul>\n  <li><span class='match'>able<\/span><\/li>\n  <li><span class='match'>about<\/span><\/li>\n  <li><span class='match'>accept<\/span><\/li>\n  <li><span class='match'>account<\/span><\/li>\n  <li><span class='match'>achieve<\/span><\/li>\n  <li><span class='match'>across<\/span><\/li>\n  <li><span class='match'>active<\/span><\/li>\n  <li><span class='match'>actual<\/span><\/li>\n  <li><span class='match'>address<\/span><\/li>\n  <li><span class='match'>admit<\/span><\/li>\n  <li><span class='match'>affect<\/span><\/li>\n  <li><span class='match'>afford<\/span><\/li>\n  <li><span class='match'>after<\/span><\/li>\n  <li><span class='match'>again<\/span><\/li>\n  <li><span class='match'>against<\/span><\/li>\n  <li><span class='match'>agent<\/span><\/li>\n  <li><span class='match'>agree<\/span><\/li>\n  <li><span class='match'>allow<\/span><\/li>\n  <li><span class='match'>almost<\/span><\/li>\n  <li><span class='match'>along<\/span><\/li>\n  <li><span class='match'>already<\/span><\/li>\n  <li><span class='match'>alright<\/span><\/li>\n  <li><span class='match'>also<\/span><\/li>\n  <li><span class='match'>always<\/span><\/li>\n  <li><span class='match'>america<\/span><\/li>\n  <li><span class='match'>amount<\/span><\/li>\n  <li><span class='match'>another<\/span><\/li>\n  <li><span class='match'>answer<\/span><\/li>\n  <li><span class='match'>apart<\/span><\/li>\n  <li><span class='match'>appear<\/span><\/li>\n  <li><span class='match'>apply<\/span><\/li>\n  <li><span class='match'>appoint<\/span><\/li>\n  <li><span class='match'>area<\/span><\/li>\n  <li><span class='match'>argue<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

```r
# leave the second argument blank to return seven letter or more
str_view(stringr::words[1:50], pattern = "^.{7,}$", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-7aac79910fd2bdd54244" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-7aac79910fd2bdd54244">{"x":{"html":"<ul>\n  <li><span class='match'>absolute<\/span><\/li>\n  <li><span class='match'>account<\/span><\/li>\n  <li><span class='match'>achieve<\/span><\/li>\n  <li><span class='match'>address<\/span><\/li>\n  <li><span class='match'>advertise<\/span><\/li>\n  <li><span class='match'>afternoon<\/span><\/li>\n  <li><span class='match'>against<\/span><\/li>\n  <li><span class='match'>already<\/span><\/li>\n  <li><span class='match'>alright<\/span><\/li>\n  <li><span class='match'>although<\/span><\/li>\n  <li><span class='match'>america<\/span><\/li>\n  <li><span class='match'>another<\/span><\/li>\n  <li><span class='match'>apparent<\/span><\/li>\n  <li><span class='match'>appoint<\/span><\/li>\n  <li><span class='match'>approach<\/span><\/li>\n  <li><span class='match'>appropriate<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->


## 14.3.3.1 Exercises
1. Create regular expressions to find all words that:

we can assess the same data set stringr::words. 

*Start with a vowel.

```r
str_view(stringr::words[1:50], "^[aeiou]", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-520339c7e5c9e2ef0f53" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-520339c7e5c9e2ef0f53">{"x":{"html":"<ul>\n  <li><span class='match'>a<\/span><\/li>\n  <li><span class='match'>a<\/span>ble<\/li>\n  <li><span class='match'>a<\/span>bout<\/li>\n  <li><span class='match'>a<\/span>bsolute<\/li>\n  <li><span class='match'>a<\/span>ccept<\/li>\n  <li><span class='match'>a<\/span>ccount<\/li>\n  <li><span class='match'>a<\/span>chieve<\/li>\n  <li><span class='match'>a<\/span>cross<\/li>\n  <li><span class='match'>a<\/span>ct<\/li>\n  <li><span class='match'>a<\/span>ctive<\/li>\n  <li><span class='match'>a<\/span>ctual<\/li>\n  <li><span class='match'>a<\/span>dd<\/li>\n  <li><span class='match'>a<\/span>ddress<\/li>\n  <li><span class='match'>a<\/span>dmit<\/li>\n  <li><span class='match'>a<\/span>dvertise<\/li>\n  <li><span class='match'>a<\/span>ffect<\/li>\n  <li><span class='match'>a<\/span>fford<\/li>\n  <li><span class='match'>a<\/span>fter<\/li>\n  <li><span class='match'>a<\/span>fternoon<\/li>\n  <li><span class='match'>a<\/span>gain<\/li>\n  <li><span class='match'>a<\/span>gainst<\/li>\n  <li><span class='match'>a<\/span>ge<\/li>\n  <li><span class='match'>a<\/span>gent<\/li>\n  <li><span class='match'>a<\/span>go<\/li>\n  <li><span class='match'>a<\/span>gree<\/li>\n  <li><span class='match'>a<\/span>ir<\/li>\n  <li><span class='match'>a<\/span>ll<\/li>\n  <li><span class='match'>a<\/span>llow<\/li>\n  <li><span class='match'>a<\/span>lmost<\/li>\n  <li><span class='match'>a<\/span>long<\/li>\n  <li><span class='match'>a<\/span>lready<\/li>\n  <li><span class='match'>a<\/span>lright<\/li>\n  <li><span class='match'>a<\/span>lso<\/li>\n  <li><span class='match'>a<\/span>lthough<\/li>\n  <li><span class='match'>a<\/span>lways<\/li>\n  <li><span class='match'>a<\/span>merica<\/li>\n  <li><span class='match'>a<\/span>mount<\/li>\n  <li><span class='match'>a<\/span>nd<\/li>\n  <li><span class='match'>a<\/span>nother<\/li>\n  <li><span class='match'>a<\/span>nswer<\/li>\n  <li><span class='match'>a<\/span>ny<\/li>\n  <li><span class='match'>a<\/span>part<\/li>\n  <li><span class='match'>a<\/span>pparent<\/li>\n  <li><span class='match'>a<\/span>ppear<\/li>\n  <li><span class='match'>a<\/span>pply<\/li>\n  <li><span class='match'>a<\/span>ppoint<\/li>\n  <li><span class='match'>a<\/span>pproach<\/li>\n  <li><span class='match'>a<\/span>ppropriate<\/li>\n  <li><span class='match'>a<\/span>rea<\/li>\n  <li><span class='match'>a<\/span>rgue<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->


*That only contain consonants. (Hint: thinking about matching “not”-vowels.)
that means we do not want any vowel in the words, then we can search for words with vowel and set match to be FALSE.

```r
str_view(stringr::words[1:400], "[aeiou]", match = FALSE)
```

<!--html_preserve--><div id="htmlwidget-850ff50bbbcedf21680e" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-850ff50bbbcedf21680e">{"x":{"html":"<ul>\n  <li>by<\/li>\n  <li>dry<\/li>\n  <li>fly<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->


*End with ed, but not with eed.

```r
str_view(stringr::words, "[^e]ed$", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-1b1a5864a87bc9a8c88f" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-1b1a5864a87bc9a8c88f">{"x":{"html":"<ul>\n  <li><span class='match'>bed<\/span><\/li>\n  <li>hund<span class='match'>red<\/span><\/li>\n  <li><span class='match'>red<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->


*End with ing or ise.

```r
str_view(stringr::words, "ise$|ing$", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-8a015e90c071be6715d1" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-8a015e90c071be6715d1">{"x":{"html":"<ul>\n  <li>advert<span class='match'>ise<\/span><\/li>\n  <li>br<span class='match'>ing<\/span><\/li>\n  <li>dur<span class='match'>ing<\/span><\/li>\n  <li>even<span class='match'>ing<\/span><\/li>\n  <li>exerc<span class='match'>ise<\/span><\/li>\n  <li>k<span class='match'>ing<\/span><\/li>\n  <li>mean<span class='match'>ing<\/span><\/li>\n  <li>morn<span class='match'>ing<\/span><\/li>\n  <li>otherw<span class='match'>ise<\/span><\/li>\n  <li>pract<span class='match'>ise<\/span><\/li>\n  <li>ra<span class='match'>ise<\/span><\/li>\n  <li>real<span class='match'>ise<\/span><\/li>\n  <li>r<span class='match'>ing<\/span><\/li>\n  <li>r<span class='match'>ise<\/span><\/li>\n  <li>s<span class='match'>ing<\/span><\/li>\n  <li>surpr<span class='match'>ise<\/span><\/li>\n  <li>th<span class='match'>ing<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->


2. Empirically verify the rule “i before e except after c”.
This indicates that the words with combination of `ie` or `cei` are way more than the one with  `ei` or `cie`

```r
str_view(stringr::words, pattern = "[^c]ie|cei", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-c700aaa204b5b70d35d5" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-c700aaa204b5b70d35d5">{"x":{"html":"<ul>\n  <li>ac<span class='match'>hie<\/span>ve<\/li>\n  <li>be<span class='match'>lie<\/span>ve<\/li>\n  <li>b<span class='match'>rie<\/span>f<\/li>\n  <li>c<span class='match'>lie<\/span>nt<\/li>\n  <li><span class='match'>die<\/span><\/li>\n  <li>expe<span class='match'>rie<\/span>nce<\/li>\n  <li><span class='match'>fie<\/span>ld<\/li>\n  <li>f<span class='match'>rie<\/span>nd<\/li>\n  <li><span class='match'>lie<\/span><\/li>\n  <li><span class='match'>pie<\/span>ce<\/li>\n  <li>q<span class='match'>uie<\/span>t<\/li>\n  <li>re<span class='match'>cei<\/span>ve<\/li>\n  <li><span class='match'>tie<\/span><\/li>\n  <li><span class='match'>vie<\/span>w<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

```r
str_view(stringr::words, pattern = "[^c]ei|cie", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-293349ea9b40d710c5d1" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-293349ea9b40d710c5d1">{"x":{"html":"<ul>\n  <li>s<span class='match'>cie<\/span>nce<\/li>\n  <li>so<span class='match'>cie<\/span>ty<\/li>\n  <li><span class='match'>wei<\/span>gh<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->
We can see from the above results that the combinaion of `ie` and `cei` are much more than the combination of `ei` and `cie`, which verify the rule.

3. Is “q” always followed by a “u”?

we try to return any words with a combination of q followed by a non-u letter: which returns no word. Therefore, we can concluded that the "q" is always followed by a "u".

```r
 str_subset(stringr::words, pattern = "q[^u]")
```

```
## character(0)
```

4. Write a regular expression that matches a word if it’s probably written in British English, not American English.

in general, words like "analyse"(British) and "analyze"(American) has difference in "se"/"sa" and "ze"/"za". We create some test words to show this matching.


```r
test <- c("analyse","analyze","organization","organisation","realise","realize")
# to match only British English,
str_view(test, pattern = "sa|se", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-ef1e33eac6196a365e2d" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-ef1e33eac6196a365e2d">{"x":{"html":"<ul>\n  <li>analy<span class='match'>se<\/span><\/li>\n  <li>organi<span class='match'>sa<\/span>tion<\/li>\n  <li>reali<span class='match'>se<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

## 14.3.4.1 Exercises

1. Describe the equivalents of ?, +, * in {m,n} form.

This controlling how many times a pattern matches:
`?` equivalent to `{,1}`
`+` equivalent to `{1,}`
`*` equivalent to `{0,}`

2. Describe in words what these regular expressions match: (read carefully to see if I’m using a regular expression or a string that defines a regular expression.)

* `^.*$` this can be used to match any string

* "\\{.+\\}" this will match a string that with a {} around the string which is not empty. We can test this with the following:


```r
str_view("{apple}","\\{.+\\}", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-9e62050175b4c436967c" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-9e62050175b4c436967c">{"x":{"html":"<ul>\n  <li><span class='match'>{apple}<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->


* \d{4}-\d{2}-\d{2} this will match a sries of number with the following format "1111-11-11".

* "\\\\{4}" \\\\ represents a back slash, then \\\\{4} is used to match 4 backslashes.


3. Create regular expressions to find all words that:

* Start with three consonants.

we use the following to match words starting with at least three consonants

```r
str_view(stringr::words, pattern = "^[^aeiou]{3,}", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-eac2bcb97770a8b84860" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-eac2bcb97770a8b84860">{"x":{"html":"<ul>\n  <li><span class='match'>Chr<\/span>ist<\/li>\n  <li><span class='match'>Chr<\/span>istmas<\/li>\n  <li><span class='match'>dry<\/span><\/li>\n  <li><span class='match'>fly<\/span><\/li>\n  <li><span class='match'>mrs<\/span><\/li>\n  <li><span class='match'>sch<\/span>eme<\/li>\n  <li><span class='match'>sch<\/span>ool<\/li>\n  <li><span class='match'>str<\/span>aight<\/li>\n  <li><span class='match'>str<\/span>ategy<\/li>\n  <li><span class='match'>str<\/span>eet<\/li>\n  <li><span class='match'>str<\/span>ike<\/li>\n  <li><span class='match'>str<\/span>ong<\/li>\n  <li><span class='match'>str<\/span>ucture<\/li>\n  <li><span class='match'>syst<\/span>em<\/li>\n  <li><span class='match'>thr<\/span>ee<\/li>\n  <li><span class='match'>thr<\/span>ough<\/li>\n  <li><span class='match'>thr<\/span>ow<\/li>\n  <li><span class='match'>try<\/span><\/li>\n  <li><span class='match'>typ<\/span>e<\/li>\n  <li><span class='match'>why<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

* Have three or more vowels in a row.

```r
str_view(stringr::words, pattern = "[aeiou]{3,}", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-c035419dbe7c31bc6f58" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-c035419dbe7c31bc6f58">{"x":{"html":"<ul>\n  <li>b<span class='match'>eau<\/span>ty<\/li>\n  <li>obv<span class='match'>iou<\/span>s<\/li>\n  <li>prev<span class='match'>iou<\/span>s<\/li>\n  <li>q<span class='match'>uie<\/span>t<\/li>\n  <li>ser<span class='match'>iou<\/span>s<\/li>\n  <li>var<span class='match'>iou<\/span>s<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->


* Have two or more vowel-consonant pairs in a row.

```r
str_view(stringr::words, pattern = "[aeiou][^aeiou]{2,}", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-77540cca51e37507e5f7" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-77540cca51e37507e5f7">{"x":{"html":"<ul>\n  <li><span class='match'>abl<\/span>e<\/li>\n  <li><span class='match'>abs<\/span>olute<\/li>\n  <li><span class='match'>acc<\/span>ept<\/li>\n  <li><span class='match'>acc<\/span>ount<\/li>\n  <li><span class='match'>ach<\/span>ieve<\/li>\n  <li><span class='match'>acr<\/span>oss<\/li>\n  <li><span class='match'>act<\/span><\/li>\n  <li><span class='match'>act<\/span>ive<\/li>\n  <li><span class='match'>act<\/span>ual<\/li>\n  <li><span class='match'>add<\/span><\/li>\n  <li><span class='match'>addr<\/span>ess<\/li>\n  <li><span class='match'>adm<\/span>it<\/li>\n  <li><span class='match'>adv<\/span>ertise<\/li>\n  <li><span class='match'>aff<\/span>ect<\/li>\n  <li><span class='match'>aff<\/span>ord<\/li>\n  <li><span class='match'>aft<\/span>er<\/li>\n  <li><span class='match'>aft<\/span>ernoon<\/li>\n  <li>aga<span class='match'>inst<\/span><\/li>\n  <li>ag<span class='match'>ent<\/span><\/li>\n  <li><span class='match'>agr<\/span>ee<\/li>\n  <li><span class='match'>all<\/span><\/li>\n  <li><span class='match'>all<\/span>ow<\/li>\n  <li><span class='match'>alm<\/span>ost<\/li>\n  <li>al<span class='match'>ong<\/span><\/li>\n  <li><span class='match'>alr<\/span>eady<\/li>\n  <li><span class='match'>alr<\/span>ight<\/li>\n  <li><span class='match'>als<\/span>o<\/li>\n  <li><span class='match'>alth<\/span>ough<\/li>\n  <li><span class='match'>alw<\/span>ays<\/li>\n  <li>amo<span class='match'>unt<\/span><\/li>\n  <li><span class='match'>and<\/span><\/li>\n  <li>an<span class='match'>oth<\/span>er<\/li>\n  <li><span class='match'>answ<\/span>er<\/li>\n  <li><span class='match'>any<\/span><\/li>\n  <li>ap<span class='match'>art<\/span><\/li>\n  <li><span class='match'>app<\/span>arent<\/li>\n  <li><span class='match'>app<\/span>ear<\/li>\n  <li><span class='match'>apply<\/span><\/li>\n  <li><span class='match'>app<\/span>oint<\/li>\n  <li><span class='match'>appr<\/span>oach<\/li>\n  <li><span class='match'>appr<\/span>opriate<\/li>\n  <li><span class='match'>arg<\/span>ue<\/li>\n  <li><span class='match'>arm<\/span><\/li>\n  <li>aro<span class='match'>und<\/span><\/li>\n  <li><span class='match'>arr<\/span>ange<\/li>\n  <li><span class='match'>art<\/span><\/li>\n  <li><span class='match'>ask<\/span><\/li>\n  <li><span class='match'>ass<\/span>ociate<\/li>\n  <li><span class='match'>ass<\/span>ume<\/li>\n  <li><span class='match'>att<\/span>end<\/li>\n  <li>a<span class='match'>uth<\/span>ority<\/li>\n  <li>avail<span class='match'>abl<\/span>e<\/li>\n  <li><span class='match'>awf<\/span>ul<\/li>\n  <li>b<span class='match'>aby<\/span><\/li>\n  <li>b<span class='match'>ack<\/span><\/li>\n  <li>bal<span class='match'>anc<\/span>e<\/li>\n  <li>b<span class='match'>all<\/span><\/li>\n  <li>b<span class='match'>ank<\/span><\/li>\n  <li>bea<span class='match'>uty<\/span><\/li>\n  <li>beh<span class='match'>ind<\/span><\/li>\n  <li>b<span class='match'>est<\/span><\/li>\n  <li>b<span class='match'>etw<\/span>een<\/li>\n  <li>b<span class='match'>ill<\/span><\/li>\n  <li>b<span class='match'>irth<\/span><\/li>\n  <li>bl<span class='match'>ack<\/span><\/li>\n  <li>bo<span class='match'>ard<\/span><\/li>\n  <li>b<span class='match'>ody<\/span><\/li>\n  <li>b<span class='match'>oth<\/span><\/li>\n  <li>b<span class='match'>oth<\/span>er<\/li>\n  <li>b<span class='match'>ottl<\/span>e<\/li>\n  <li>b<span class='match'>ott<\/span>om<\/li>\n  <li>br<span class='match'>ill<\/span>iant<\/li>\n  <li>br<span class='match'>ing<\/span><\/li>\n  <li>br<span class='match'>oth<\/span>er<\/li>\n  <li>b<span class='match'>udg<\/span>et<\/li>\n  <li>bu<span class='match'>ild<\/span><\/li>\n  <li>busin<span class='match'>ess<\/span><\/li>\n  <li>b<span class='match'>usy<\/span><\/li>\n  <li>c<span class='match'>all<\/span><\/li>\n  <li>c<span class='match'>ard<\/span><\/li>\n  <li>c<span class='match'>arry<\/span><\/li>\n  <li>c<span class='match'>atch<\/span><\/li>\n  <li>c<span class='match'>ent<\/span><\/li>\n  <li>c<span class='match'>entr<\/span>e<\/li>\n  <li>c<span class='match'>ert<\/span>ain<\/li>\n  <li>cha<span class='match'>irm<\/span>an<\/li>\n  <li>ch<span class='match'>anc<\/span>e<\/li>\n  <li>ch<span class='match'>ang<\/span>e<\/li>\n  <li>char<span class='match'>act<\/span>er<\/li>\n  <li>ch<span class='match'>arg<\/span>e<\/li>\n  <li>ch<span class='match'>eck<\/span><\/li>\n  <li>ch<span class='match'>ild<\/span><\/li>\n  <li>Chr<span class='match'>ist<\/span><\/li>\n  <li>Chr<span class='match'>istm<\/span>as<\/li>\n  <li>ch<span class='match'>urch<\/span><\/li>\n  <li>c<span class='match'>ity<\/span><\/li>\n  <li>cl<span class='match'>ass<\/span><\/li>\n  <li>cli<span class='match'>ent<\/span><\/li>\n  <li>cl<span class='match'>ock<\/span><\/li>\n  <li>cl<span class='match'>oth<\/span>e<\/li>\n  <li>c<span class='match'>off<\/span>ee<\/li>\n  <li>c<span class='match'>old<\/span><\/li>\n  <li>c<span class='match'>oll<\/span>eague<\/li>\n  <li>c<span class='match'>oll<\/span>ect<\/li>\n  <li>c<span class='match'>oll<\/span>ege<\/li>\n  <li>c<span class='match'>omm<\/span>ent<\/li>\n  <li>c<span class='match'>omm<\/span>it<\/li>\n  <li>c<span class='match'>omm<\/span>ittee<\/li>\n  <li>c<span class='match'>omm<\/span>on<\/li>\n  <li>c<span class='match'>omm<\/span>unity<\/li>\n  <li>c<span class='match'>omp<\/span>any<\/li>\n  <li>c<span class='match'>omp<\/span>are<\/li>\n  <li>c<span class='match'>ompl<\/span>ete<\/li>\n  <li>c<span class='match'>omp<\/span>ute<\/li>\n  <li>c<span class='match'>onc<\/span>ern<\/li>\n  <li>c<span class='match'>ond<\/span>ition<\/li>\n  <li>c<span class='match'>onf<\/span>er<\/li>\n  <li>c<span class='match'>ons<\/span>ider<\/li>\n  <li>c<span class='match'>ons<\/span>ult<\/li>\n  <li>c<span class='match'>ont<\/span>act<\/li>\n  <li>c<span class='match'>ont<\/span>inue<\/li>\n  <li>c<span class='match'>ontr<\/span>act<\/li>\n  <li>c<span class='match'>ontr<\/span>ol<\/li>\n  <li>c<span class='match'>onv<\/span>erse<\/li>\n  <li>c<span class='match'>opy<\/span><\/li>\n  <li>c<span class='match'>orn<\/span>er<\/li>\n  <li>c<span class='match'>orr<\/span>ect<\/li>\n  <li>c<span class='match'>ost<\/span><\/li>\n  <li>co<span class='match'>uld<\/span><\/li>\n  <li>co<span class='match'>unc<\/span>il<\/li>\n  <li>co<span class='match'>unt<\/span><\/li>\n  <li>co<span class='match'>untry<\/span><\/li>\n  <li>co<span class='match'>unty<\/span><\/li>\n  <li>co<span class='match'>upl<\/span>e<\/li>\n  <li>co<span class='match'>urs<\/span>e<\/li>\n  <li>co<span class='match'>urt<\/span><\/li>\n  <li>cr<span class='match'>oss<\/span><\/li>\n  <li>c<span class='match'>urr<\/span>ent<\/li>\n  <li>d<span class='match'>ang<\/span>er<\/li>\n  <li>d<span class='match'>egr<\/span>ee<\/li>\n  <li>dep<span class='match'>artm<\/span>ent<\/li>\n  <li>dep<span class='match'>end<\/span><\/li>\n  <li>d<span class='match'>escr<\/span>ibe<\/li>\n  <li>des<span class='match'>ign<\/span><\/li>\n  <li>d<span class='match'>iff<\/span>erence<\/li>\n  <li>d<span class='match'>iff<\/span>icult<\/li>\n  <li>d<span class='match'>inn<\/span>er<\/li>\n  <li>dir<span class='match'>ect<\/span><\/li>\n  <li>d<span class='match'>isc<\/span>uss<\/li>\n  <li>d<span class='match'>istr<\/span>ict<\/li>\n  <li>d<span class='match'>oct<\/span>or<\/li>\n  <li>docum<span class='match'>ent<\/span><\/li>\n  <li>do<span class='match'>ubl<\/span>e<\/li>\n  <li>do<span class='match'>ubt<\/span><\/li>\n  <li>d<span class='match'>own<\/span><\/li>\n  <li>dr<span class='match'>ess<\/span><\/li>\n  <li>dr<span class='match'>ink<\/span><\/li>\n  <li>dur<span class='match'>ing<\/span><\/li>\n  <li>e<span class='match'>ach<\/span><\/li>\n  <li>e<span class='match'>arly<\/span><\/li>\n  <li>e<span class='match'>ast<\/span><\/li>\n  <li>e<span class='match'>asy<\/span><\/li>\n  <li>econ<span class='match'>omy<\/span><\/li>\n  <li><span class='match'>eff<\/span>ect<\/li>\n  <li><span class='match'>egg<\/span><\/li>\n  <li>e<span class='match'>ight<\/span><\/li>\n  <li>e<span class='match'>ith<\/span>er<\/li>\n  <li>el<span class='match'>ect<\/span><\/li>\n  <li>el<span class='match'>ectr<\/span>ic<\/li>\n  <li><span class='match'>els<\/span>e<\/li>\n  <li><span class='match'>empl<\/span>oy<\/li>\n  <li><span class='match'>enc<\/span>ourage<\/li>\n  <li><span class='match'>end<\/span><\/li>\n  <li><span class='match'>eng<\/span>ine<\/li>\n  <li><span class='match'>engl<\/span>ish<\/li>\n  <li><span class='match'>enj<\/span>oy<\/li>\n  <li>eno<span class='match'>ugh<\/span><\/li>\n  <li><span class='match'>ent<\/span>er<\/li>\n  <li><span class='match'>env<\/span>ironment<\/li>\n  <li><span class='match'>esp<\/span>ecial<\/li>\n  <li>even<span class='match'>ing<\/span><\/li>\n  <li>ev<span class='match'>ery<\/span><\/li>\n  <li>evid<span class='match'>enc<\/span>e<\/li>\n  <li>ex<span class='match'>act<\/span><\/li>\n  <li>ex<span class='match'>ampl<\/span>e<\/li>\n  <li><span class='match'>exc<\/span>ept<\/li>\n  <li><span class='match'>exc<\/span>use<\/li>\n  <li>ex<span class='match'>erc<\/span>ise<\/li>\n  <li>ex<span class='match'>ist<\/span><\/li>\n  <li><span class='match'>exp<\/span>ect<\/li>\n  <li><span class='match'>exp<\/span>ense<\/li>\n  <li><span class='match'>exp<\/span>erience<\/li>\n  <li><span class='match'>expl<\/span>ain<\/li>\n  <li><span class='match'>expr<\/span>ess<\/li>\n  <li><span class='match'>extr<\/span>a<\/li>\n  <li>f<span class='match'>act<\/span><\/li>\n  <li>f<span class='match'>all<\/span><\/li>\n  <li>fam<span class='match'>ily<\/span><\/li>\n  <li>f<span class='match'>arm<\/span><\/li>\n  <li>f<span class='match'>ast<\/span><\/li>\n  <li>f<span class='match'>ath<\/span>er<\/li>\n  <li>fi<span class='match'>eld<\/span><\/li>\n  <li>f<span class='match'>ight<\/span><\/li>\n  <li>f<span class='match'>ill<\/span><\/li>\n  <li>f<span class='match'>ilm<\/span><\/li>\n  <li>fin<span class='match'>anc<\/span>e<\/li>\n  <li>f<span class='match'>ind<\/span><\/li>\n  <li>fin<span class='match'>ish<\/span><\/li>\n  <li>f<span class='match'>irst<\/span><\/li>\n  <li>f<span class='match'>ish<\/span><\/li>\n  <li>f<span class='match'>oll<\/span>ow<\/li>\n  <li>f<span class='match'>orc<\/span>e<\/li>\n  <li>f<span class='match'>org<\/span>et<\/li>\n  <li>f<span class='match'>orm<\/span><\/li>\n  <li>f<span class='match'>ort<\/span>une<\/li>\n  <li>f<span class='match'>orw<\/span>ard<\/li>\n  <li>fr<span class='match'>anc<\/span>e<\/li>\n  <li>fri<span class='match'>end<\/span><\/li>\n  <li>fr<span class='match'>ont<\/span><\/li>\n  <li>f<span class='match'>ull<\/span><\/li>\n  <li>f<span class='match'>unct<\/span>ion<\/li>\n  <li>f<span class='match'>und<\/span><\/li>\n  <li>f<span class='match'>urth<\/span>er<\/li>\n  <li>g<span class='match'>ard<\/span>en<\/li>\n  <li>g<span class='match'>erm<\/span>any<\/li>\n  <li>g<span class='match'>irl<\/span><\/li>\n  <li>gl<span class='match'>ass<\/span><\/li>\n  <li>go<span class='match'>odby<\/span>e<\/li>\n  <li>gov<span class='match'>ern<\/span><\/li>\n  <li>gr<span class='match'>and<\/span><\/li>\n  <li>gr<span class='match'>ant<\/span><\/li>\n  <li>gro<span class='match'>und<\/span><\/li>\n  <li>gu<span class='match'>ess<\/span><\/li>\n  <li>h<span class='match'>alf<\/span><\/li>\n  <li>h<span class='match'>all<\/span><\/li>\n  <li>h<span class='match'>and<\/span><\/li>\n  <li>h<span class='match'>ang<\/span><\/li>\n  <li>h<span class='match'>app<\/span>en<\/li>\n  <li>h<span class='match'>appy<\/span><\/li>\n  <li>h<span class='match'>ard<\/span><\/li>\n  <li>he<span class='match'>alth<\/span><\/li>\n  <li>he<span class='match'>art<\/span><\/li>\n  <li>he<span class='match'>avy<\/span><\/li>\n  <li>h<span class='match'>ell<\/span><\/li>\n  <li>h<span class='match'>elp<\/span><\/li>\n  <li>h<span class='match'>igh<\/span><\/li>\n  <li>h<span class='match'>ist<\/span>ory<\/li>\n  <li>h<span class='match'>old<\/span><\/li>\n  <li>hon<span class='match'>est<\/span><\/li>\n  <li>h<span class='match'>ors<\/span>e<\/li>\n  <li>h<span class='match'>osp<\/span>ital<\/li>\n  <li>h<span class='match'>ull<\/span>o<\/li>\n  <li>h<span class='match'>undr<\/span>ed<\/li>\n  <li>h<span class='match'>usb<\/span>and<\/li>\n  <li>id<span class='match'>ent<\/span>ify<\/li>\n  <li><span class='match'>imp<\/span>ortant<\/li>\n  <li><span class='match'>impr<\/span>ove<\/li>\n  <li><span class='match'>incl<\/span>ude<\/li>\n  <li><span class='match'>inc<\/span>ome<\/li>\n  <li><span class='match'>incr<\/span>ease<\/li>\n  <li><span class='match'>ind<\/span>eed<\/li>\n  <li><span class='match'>ind<\/span>ividual<\/li>\n  <li><span class='match'>ind<\/span>ustry<\/li>\n  <li><span class='match'>inf<\/span>orm<\/li>\n  <li><span class='match'>ins<\/span>ide<\/li>\n  <li><span class='match'>inst<\/span>ead<\/li>\n  <li><span class='match'>ins<\/span>ure<\/li>\n  <li><span class='match'>int<\/span>erest<\/li>\n  <li><span class='match'>int<\/span>o<\/li>\n  <li><span class='match'>intr<\/span>oduce<\/li>\n  <li><span class='match'>inv<\/span>est<\/li>\n  <li><span class='match'>inv<\/span>olve<\/li>\n  <li><span class='match'>iss<\/span>ue<\/li>\n  <li>j<span class='match'>udg<\/span>e<\/li>\n  <li>j<span class='match'>ump<\/span><\/li>\n  <li>j<span class='match'>ust<\/span><\/li>\n  <li>k<span class='match'>ill<\/span><\/li>\n  <li>k<span class='match'>ind<\/span><\/li>\n  <li>k<span class='match'>ing<\/span><\/li>\n  <li>k<span class='match'>itch<\/span>en<\/li>\n  <li>kn<span class='match'>ock<\/span><\/li>\n  <li>l<span class='match'>ady<\/span><\/li>\n  <li>l<span class='match'>and<\/span><\/li>\n  <li>l<span class='match'>ang<\/span>uage<\/li>\n  <li>l<span class='match'>arg<\/span>e<\/li>\n  <li>l<span class='match'>ast<\/span><\/li>\n  <li>la<span class='match'>ugh<\/span><\/li>\n  <li>le<span class='match'>arn<\/span><\/li>\n  <li>l<span class='match'>eft<\/span><\/li>\n  <li>l<span class='match'>ess<\/span><\/li>\n  <li>l<span class='match'>ett<\/span>er<\/li>\n  <li>l<span class='match'>ight<\/span><\/li>\n  <li>lik<span class='match'>ely<\/span><\/li>\n  <li>l<span class='match'>ink<\/span><\/li>\n  <li>l<span class='match'>ist<\/span><\/li>\n  <li>l<span class='match'>ist<\/span>en<\/li>\n  <li>l<span class='match'>ittl<\/span>e<\/li>\n  <li>l<span class='match'>ock<\/span><\/li>\n  <li>l<span class='match'>ond<\/span>on<\/li>\n  <li>l<span class='match'>ong<\/span><\/li>\n  <li>l<span class='match'>ord<\/span><\/li>\n  <li>l<span class='match'>uck<\/span><\/li>\n  <li>l<span class='match'>unch<\/span><\/li>\n  <li>m<span class='match'>ach<\/span>ine<\/li>\n  <li>m<span class='match'>any<\/span><\/li>\n  <li>m<span class='match'>ark<\/span><\/li>\n  <li>m<span class='match'>ark<\/span>et<\/li>\n  <li>m<span class='match'>arry<\/span><\/li>\n  <li>m<span class='match'>atch<\/span><\/li>\n  <li>m<span class='match'>att<\/span>er<\/li>\n  <li>m<span class='match'>ayb<\/span>e<\/li>\n  <li>mean<span class='match'>ing<\/span><\/li>\n  <li>m<span class='match'>emb<\/span>er<\/li>\n  <li>m<span class='match'>ent<\/span>ion<\/li>\n  <li>m<span class='match'>iddl<\/span>e<\/li>\n  <li>m<span class='match'>ight<\/span><\/li>\n  <li>m<span class='match'>ilk<\/span><\/li>\n  <li>m<span class='match'>ill<\/span>ion<\/li>\n  <li>m<span class='match'>ind<\/span><\/li>\n  <li>min<span class='match'>ist<\/span>er<\/li>\n  <li>m<span class='match'>iss<\/span><\/li>\n  <li>m<span class='match'>ist<\/span>er<\/li>\n  <li>mom<span class='match'>ent<\/span><\/li>\n  <li>m<span class='match'>ond<\/span>ay<\/li>\n  <li>m<span class='match'>onth<\/span><\/li>\n  <li>m<span class='match'>orn<\/span>ing<\/li>\n  <li>m<span class='match'>ost<\/span><\/li>\n  <li>m<span class='match'>oth<\/span>er<\/li>\n  <li>m<span class='match'>uch<\/span><\/li>\n  <li>m<span class='match'>ust<\/span><\/li>\n  <li>nec<span class='match'>ess<\/span>ary<\/li>\n  <li>n<span class='match'>ews<\/span><\/li>\n  <li>n<span class='match'>ext<\/span><\/li>\n  <li>n<span class='match'>ight<\/span><\/li>\n  <li>n<span class='match'>orm<\/span>al<\/li>\n  <li>n<span class='match'>orth<\/span><\/li>\n  <li>n<span class='match'>umb<\/span>er<\/li>\n  <li><span class='match'>obv<\/span>ious<\/li>\n  <li><span class='match'>occ<\/span>asion<\/li>\n  <li><span class='match'>odd<\/span><\/li>\n  <li><span class='match'>off<\/span><\/li>\n  <li><span class='match'>off<\/span>er<\/li>\n  <li><span class='match'>off<\/span>ice<\/li>\n  <li><span class='match'>oft<\/span>en<\/li>\n  <li><span class='match'>old<\/span><\/li>\n  <li><span class='match'>onc<\/span>e<\/li>\n  <li><span class='match'>only<\/span><\/li>\n  <li><span class='match'>opp<\/span>ortunity<\/li>\n  <li><span class='match'>opp<\/span>ose<\/li>\n  <li><span class='match'>ord<\/span>er<\/li>\n  <li><span class='match'>org<\/span>anize<\/li>\n  <li><span class='match'>oth<\/span>er<\/li>\n  <li><span class='match'>oth<\/span>erwise<\/li>\n  <li>o<span class='match'>ught<\/span><\/li>\n  <li><span class='match'>own<\/span><\/li>\n  <li>p<span class='match'>ack<\/span><\/li>\n  <li>pa<span class='match'>int<\/span><\/li>\n  <li>par<span class='match'>agr<\/span>aph<\/li>\n  <li>p<span class='match'>ard<\/span>on<\/li>\n  <li>par<span class='match'>ent<\/span><\/li>\n  <li>p<span class='match'>ark<\/span><\/li>\n  <li>p<span class='match'>art<\/span><\/li>\n  <li>p<span class='match'>art<\/span>icular<\/li>\n  <li>p<span class='match'>arty<\/span><\/li>\n  <li>p<span class='match'>ass<\/span><\/li>\n  <li>p<span class='match'>ast<\/span><\/li>\n  <li>p<span class='match'>enc<\/span>e<\/li>\n  <li>p<span class='match'>ens<\/span>ion<\/li>\n  <li>pe<span class='match'>opl<\/span>e<\/li>\n  <li>p<span class='match'>erc<\/span>ent<\/li>\n  <li>p<span class='match'>erf<\/span>ect<\/li>\n  <li>p<span class='match'>erh<\/span>aps<\/li>\n  <li>p<span class='match'>ers<\/span>on<\/li>\n  <li>phot<span class='match'>ogr<\/span>aph<\/li>\n  <li>p<span class='match'>ick<\/span><\/li>\n  <li>p<span class='match'>ict<\/span>ure<\/li>\n  <li>po<span class='match'>int<\/span><\/li>\n  <li>pol<span class='match'>icy<\/span><\/li>\n  <li>p<span class='match'>oss<\/span>ible<\/li>\n  <li>p<span class='match'>ost<\/span><\/li>\n  <li>po<span class='match'>und<\/span><\/li>\n  <li>pr<span class='match'>act<\/span>ise<\/li>\n  <li>pres<span class='match'>ent<\/span><\/li>\n  <li>pr<span class='match'>ess<\/span><\/li>\n  <li>pr<span class='match'>ess<\/span>ure<\/li>\n  <li>pr<span class='match'>etty<\/span><\/li>\n  <li>pr<span class='match'>int<\/span><\/li>\n  <li>prob<span class='match'>abl<\/span>e<\/li>\n  <li>pr<span class='match'>obl<\/span>em<\/li>\n  <li>proc<span class='match'>ess<\/span><\/li>\n  <li>prod<span class='match'>uct<\/span><\/li>\n  <li>pr<span class='match'>ogr<\/span>amme<\/li>\n  <li>proj<span class='match'>ect<\/span><\/li>\n  <li>prot<span class='match'>ect<\/span><\/li>\n  <li>p<span class='match'>ubl<\/span>ic<\/li>\n  <li>p<span class='match'>ull<\/span><\/li>\n  <li>p<span class='match'>urp<\/span>ose<\/li>\n  <li>p<span class='match'>ush<\/span><\/li>\n  <li>qual<span class='match'>ity<\/span><\/li>\n  <li>qu<span class='match'>art<\/span>er<\/li>\n  <li>qu<span class='match'>est<\/span>ion<\/li>\n  <li>qu<span class='match'>ick<\/span><\/li>\n  <li>r<span class='match'>ang<\/span>e<\/li>\n  <li>r<span class='match'>ath<\/span>er<\/li>\n  <li>re<span class='match'>ady<\/span><\/li>\n  <li>re<span class='match'>ally<\/span><\/li>\n  <li>rec<span class='match'>ent<\/span><\/li>\n  <li>r<span class='match'>eck<\/span>on<\/li>\n  <li>rec<span class='match'>ogn<\/span>ize<\/li>\n  <li>rec<span class='match'>omm<\/span>end<\/li>\n  <li>rec<span class='match'>ord<\/span><\/li>\n  <li>reg<span class='match'>ard<\/span><\/li>\n  <li>rem<span class='match'>emb<\/span>er<\/li>\n  <li>rep<span class='match'>ort<\/span><\/li>\n  <li>r<span class='match'>epr<\/span>esent<\/li>\n  <li>rese<span class='match'>arch<\/span><\/li>\n  <li>reso<span class='match'>urc<\/span>e<\/li>\n  <li>r<span class='match'>esp<\/span>ect<\/li>\n  <li>r<span class='match'>esp<\/span>onsible<\/li>\n  <li>r<span class='match'>est<\/span><\/li>\n  <li>res<span class='match'>ult<\/span><\/li>\n  <li>ret<span class='match'>urn<\/span><\/li>\n  <li>r<span class='match'>ight<\/span><\/li>\n  <li>r<span class='match'>ing<\/span><\/li>\n  <li>r<span class='match'>oll<\/span><\/li>\n  <li>ro<span class='match'>und<\/span><\/li>\n  <li>sat<span class='match'>urd<\/span>ay<\/li>\n  <li>sci<span class='match'>enc<\/span>e<\/li>\n  <li>sc<span class='match'>otl<\/span>and<\/li>\n  <li>sec<span class='match'>ond<\/span><\/li>\n  <li>s<span class='match'>ecr<\/span>etary<\/li>\n  <li>s<span class='match'>ect<\/span>ion<\/li>\n  <li>s<span class='match'>elf<\/span><\/li>\n  <li>s<span class='match'>ell<\/span><\/li>\n  <li>s<span class='match'>end<\/span><\/li>\n  <li>s<span class='match'>ens<\/span>e<\/li>\n  <li>s<span class='match'>erv<\/span>e<\/li>\n  <li>s<span class='match'>erv<\/span>ice<\/li>\n  <li>s<span class='match'>ettl<\/span>e<\/li>\n  <li>sh<span class='match'>all<\/span><\/li>\n  <li>sh<span class='match'>ort<\/span><\/li>\n  <li>sho<span class='match'>uld<\/span><\/li>\n  <li>s<span class='match'>ick<\/span><\/li>\n  <li>s<span class='match'>ign<\/span><\/li>\n  <li>s<span class='match'>impl<\/span>e<\/li>\n  <li>s<span class='match'>inc<\/span>e<\/li>\n  <li>s<span class='match'>ing<\/span><\/li>\n  <li>s<span class='match'>ingl<\/span>e<\/li>\n  <li>s<span class='match'>ist<\/span>er<\/li>\n  <li>sl<span class='match'>ight<\/span><\/li>\n  <li>sm<span class='match'>all<\/span><\/li>\n  <li>soci<span class='match'>ety<\/span><\/li>\n  <li>s<span class='match'>orry<\/span><\/li>\n  <li>s<span class='match'>ort<\/span><\/li>\n  <li>so<span class='match'>und<\/span><\/li>\n  <li>so<span class='match'>uth<\/span><\/li>\n  <li>sp<span class='match'>ell<\/span><\/li>\n  <li>sp<span class='match'>end<\/span><\/li>\n  <li>st<span class='match'>aff<\/span><\/li>\n  <li>sta<span class='match'>irs<\/span><\/li>\n  <li>st<span class='match'>and<\/span><\/li>\n  <li>st<span class='match'>and<\/span>ard<\/li>\n  <li>st<span class='match'>art<\/span><\/li>\n  <li>st<span class='match'>ick<\/span><\/li>\n  <li>st<span class='match'>ill<\/span><\/li>\n  <li>st<span class='match'>ory<\/span><\/li>\n  <li>stra<span class='match'>ight<\/span><\/li>\n  <li>strat<span class='match'>egy<\/span><\/li>\n  <li>str<span class='match'>ong<\/span><\/li>\n  <li>str<span class='match'>uct<\/span>ure<\/li>\n  <li>stud<span class='match'>ent<\/span><\/li>\n  <li>st<span class='match'>udy<\/span><\/li>\n  <li>st<span class='match'>uff<\/span><\/li>\n  <li>s<span class='match'>ubj<\/span>ect<\/li>\n  <li>s<span class='match'>ucc<\/span>eed<\/li>\n  <li>s<span class='match'>uch<\/span><\/li>\n  <li>s<span class='match'>udd<\/span>en<\/li>\n  <li>s<span class='match'>ugg<\/span>est<\/li>\n  <li>s<span class='match'>umm<\/span>er<\/li>\n  <li>s<span class='match'>und<\/span>ay<\/li>\n  <li>s<span class='match'>upply<\/span><\/li>\n  <li>s<span class='match'>upp<\/span>ort<\/li>\n  <li>s<span class='match'>upp<\/span>ose<\/li>\n  <li>s<span class='match'>urpr<\/span>ise<\/li>\n  <li>sw<span class='match'>itch<\/span><\/li>\n  <li>t<span class='match'>abl<\/span>e<\/li>\n  <li>t<span class='match'>alk<\/span><\/li>\n  <li>te<span class='match'>ach<\/span><\/li>\n  <li>tel<span class='match'>eph<\/span>one<\/li>\n  <li>t<span class='match'>ell<\/span><\/li>\n  <li>t<span class='match'>end<\/span><\/li>\n  <li>t<span class='match'>erm<\/span><\/li>\n  <li>t<span class='match'>err<\/span>ible<\/li>\n  <li>t<span class='match'>est<\/span><\/li>\n  <li>th<span class='match'>ank<\/span><\/li>\n  <li>th<span class='match'>ing<\/span><\/li>\n  <li>th<span class='match'>ink<\/span><\/li>\n  <li>th<span class='match'>irt<\/span>een<\/li>\n  <li>th<span class='match'>irty<\/span><\/li>\n  <li>tho<span class='match'>ugh<\/span><\/li>\n  <li>thous<span class='match'>and<\/span><\/li>\n  <li>thro<span class='match'>ugh<\/span><\/li>\n  <li>th<span class='match'>ursd<\/span>ay<\/li>\n  <li>tog<span class='match'>eth<\/span>er<\/li>\n  <li>tom<span class='match'>orr<\/span>ow<\/li>\n  <li>ton<span class='match'>ight<\/span><\/li>\n  <li>to<span class='match'>uch<\/span><\/li>\n  <li>tow<span class='match'>ard<\/span><\/li>\n  <li>t<span class='match'>own<\/span><\/li>\n  <li>tr<span class='match'>aff<\/span>ic<\/li>\n  <li>tr<span class='match'>ansp<\/span>ort<\/li>\n  <li>tro<span class='match'>ubl<\/span>e<\/li>\n  <li>tr<span class='match'>ust<\/span><\/li>\n  <li>tu<span class='match'>esd<\/span>ay<\/li>\n  <li>t<span class='match'>urn<\/span><\/li>\n  <li>tw<span class='match'>elv<\/span>e<\/li>\n  <li>tw<span class='match'>enty<\/span><\/li>\n  <li><span class='match'>und<\/span>er<\/li>\n  <li><span class='match'>und<\/span>erstand<\/li>\n  <li>univ<span class='match'>ers<\/span>ity<\/li>\n  <li><span class='match'>unl<\/span>ess<\/li>\n  <li><span class='match'>unt<\/span>il<\/li>\n  <li>v<span class='match'>ery<\/span><\/li>\n  <li>v<span class='match'>ill<\/span>age<\/li>\n  <li>w<span class='match'>alk<\/span><\/li>\n  <li>w<span class='match'>all<\/span><\/li>\n  <li>w<span class='match'>ant<\/span><\/li>\n  <li>w<span class='match'>arm<\/span><\/li>\n  <li>w<span class='match'>ash<\/span><\/li>\n  <li>w<span class='match'>ast<\/span>e<\/li>\n  <li>w<span class='match'>atch<\/span><\/li>\n  <li>w<span class='match'>edn<\/span>esday<\/li>\n  <li>we<span class='match'>igh<\/span><\/li>\n  <li>w<span class='match'>elc<\/span>ome<\/li>\n  <li>w<span class='match'>ell<\/span><\/li>\n  <li>w<span class='match'>est<\/span><\/li>\n  <li>wh<span class='match'>eth<\/span>er<\/li>\n  <li>wh<span class='match'>ich<\/span><\/li>\n  <li>w<span class='match'>ill<\/span><\/li>\n  <li>w<span class='match'>ind<\/span><\/li>\n  <li>w<span class='match'>ind<\/span>ow<\/li>\n  <li>w<span class='match'>ish<\/span><\/li>\n  <li>w<span class='match'>ith<\/span><\/li>\n  <li>w<span class='match'>ith<\/span>in<\/li>\n  <li>w<span class='match'>ith<\/span>out<\/li>\n  <li>w<span class='match'>ond<\/span>er<\/li>\n  <li>w<span class='match'>ord<\/span><\/li>\n  <li>w<span class='match'>ork<\/span><\/li>\n  <li>w<span class='match'>orld<\/span><\/li>\n  <li>w<span class='match'>orry<\/span><\/li>\n  <li>w<span class='match'>ors<\/span>e<\/li>\n  <li>w<span class='match'>orth<\/span><\/li>\n  <li>wo<span class='match'>uld<\/span><\/li>\n  <li>wr<span class='match'>ong<\/span><\/li>\n  <li>y<span class='match'>est<\/span>erday<\/li>\n  <li>yo<span class='match'>ung<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

## 14.3.5.1 Exercises

1. Describe, in words, what these expressions will match:

* (.)\1\1 this is used to matched an identical character appeared 3 times in a row.


* "(.)(.)\\2\\1" this is matching a two character and the reverse of these two character, such as abba

```r
str_view(stringr::words, "(.)(.)\\2\\1", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-0f2d0e2ba420e7fba7d1" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-0f2d0e2ba420e7fba7d1">{"x":{"html":"<ul>\n  <li>after<span class='match'>noon<\/span><\/li>\n  <li><span class='match'>appa<\/span>rent<\/li>\n  <li><span class='match'>arra<\/span>nge<\/li>\n  <li>b<span class='match'>otto<\/span>m<\/li>\n  <li>br<span class='match'>illi<\/span>ant<\/li>\n  <li>c<span class='match'>ommo<\/span>n<\/li>\n  <li>d<span class='match'>iffi<\/span>cult<\/li>\n  <li><span class='match'>effe<\/span>ct<\/li>\n  <li>f<span class='match'>ollo<\/span>w<\/li>\n  <li>in<span class='match'>deed<\/span><\/li>\n  <li>l<span class='match'>ette<\/span>r<\/li>\n  <li>m<span class='match'>illi<\/span>on<\/li>\n  <li><span class='match'>oppo<\/span>rtunity<\/li>\n  <li><span class='match'>oppo<\/span>se<\/li>\n  <li>tom<span class='match'>orro<\/span>w<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

* (..)\1 this match two characters that appears twice such as abab format

* "(.).\\1.\\1" match string with format such as "axaya"  3 repeated character in a row with 2 different inserted.
* "(.)(.)(.).*\\3\\2\\1" match string with format such as "abc...cba" the character in between the "abc" and "cba" should have a length more than 0.

2. Construct regular expressions to match words that:

* Start and end with the same character.

```r
str_view(stringr::words,"^(.).*\\1$", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-6a23e8c7c4baadde4e9e" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-6a23e8c7c4baadde4e9e">{"x":{"html":"<ul>\n  <li><span class='match'>america<\/span><\/li>\n  <li><span class='match'>area<\/span><\/li>\n  <li><span class='match'>dad<\/span><\/li>\n  <li><span class='match'>dead<\/span><\/li>\n  <li><span class='match'>depend<\/span><\/li>\n  <li><span class='match'>educate<\/span><\/li>\n  <li><span class='match'>else<\/span><\/li>\n  <li><span class='match'>encourage<\/span><\/li>\n  <li><span class='match'>engine<\/span><\/li>\n  <li><span class='match'>europe<\/span><\/li>\n  <li><span class='match'>evidence<\/span><\/li>\n  <li><span class='match'>example<\/span><\/li>\n  <li><span class='match'>excuse<\/span><\/li>\n  <li><span class='match'>exercise<\/span><\/li>\n  <li><span class='match'>expense<\/span><\/li>\n  <li><span class='match'>experience<\/span><\/li>\n  <li><span class='match'>eye<\/span><\/li>\n  <li><span class='match'>health<\/span><\/li>\n  <li><span class='match'>high<\/span><\/li>\n  <li><span class='match'>knock<\/span><\/li>\n  <li><span class='match'>level<\/span><\/li>\n  <li><span class='match'>local<\/span><\/li>\n  <li><span class='match'>nation<\/span><\/li>\n  <li><span class='match'>non<\/span><\/li>\n  <li><span class='match'>rather<\/span><\/li>\n  <li><span class='match'>refer<\/span><\/li>\n  <li><span class='match'>remember<\/span><\/li>\n  <li><span class='match'>serious<\/span><\/li>\n  <li><span class='match'>stairs<\/span><\/li>\n  <li><span class='match'>test<\/span><\/li>\n  <li><span class='match'>tonight<\/span><\/li>\n  <li><span class='match'>transport<\/span><\/li>\n  <li><span class='match'>treat<\/span><\/li>\n  <li><span class='match'>trust<\/span><\/li>\n  <li><span class='match'>window<\/span><\/li>\n  <li><span class='match'>yesterday<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->


* Contain a repeated pair of letters (e.g. “church” contains “ch” repeated twice.)

```r
str_view(stringr::words,"(.)(.)\\1\\2", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-5d88b9270bbbcdeaf98b" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-5d88b9270bbbcdeaf98b">{"x":{"html":"<ul>\n  <li>r<span class='match'>emem<\/span>ber<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->


* Contain one letter repeated in at least three places (e.g. “eleven” contains three “e”s.)

```r
str_view(stringr::words,"(.).*\\1.*\\1", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-ec1d6fc191352da33fde" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-ec1d6fc191352da33fde">{"x":{"html":"<ul>\n  <li>a<span class='match'>pprop<\/span>riate<\/li>\n  <li><span class='match'>availa<\/span>ble<\/li>\n  <li>b<span class='match'>elieve<\/span><\/li>\n  <li>b<span class='match'>etwee<\/span>n<\/li>\n  <li>bu<span class='match'>siness<\/span><\/li>\n  <li>d<span class='match'>egree<\/span><\/li>\n  <li>diff<span class='match'>erence<\/span><\/li>\n  <li>di<span class='match'>scuss<\/span><\/li>\n  <li><span class='match'>eleve<\/span>n<\/li>\n  <li>e<span class='match'>nvironmen<\/span>t<\/li>\n  <li><span class='match'>evidence<\/span><\/li>\n  <li><span class='match'>exercise<\/span><\/li>\n  <li><span class='match'>expense<\/span><\/li>\n  <li><span class='match'>experience<\/span><\/li>\n  <li><span class='match'>indivi<\/span>dual<\/li>\n  <li>p<span class='match'>aragra<\/span>ph<\/li>\n  <li>r<span class='match'>eceive<\/span><\/li>\n  <li>r<span class='match'>emembe<\/span>r<\/li>\n  <li>r<span class='match'>eprese<\/span>nt<\/li>\n  <li>t<span class='match'>elephone<\/span><\/li>\n  <li>th<span class='match'>erefore<\/span><\/li>\n  <li>t<span class='match'>omorro<\/span>w<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->


## 14.4.2 Exercises

1. For each of the following challenges, try solving it by using both a single regular expression, and a combination of multiple str_detect() calls.

* Find all words that start or end with x.

```r
# single regular expression
str_view_all(stringr::words, "x$|^x", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-5fec068866c2cf331986" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-5fec068866c2cf331986">{"x":{"html":"<ul>\n  <li>bo<span class='match'>x<\/span><\/li>\n  <li>se<span class='match'>x<\/span><\/li>\n  <li>si<span class='match'>x<\/span><\/li>\n  <li>ta<span class='match'>x<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

```r
# combination of multiple expression
st <- str_detect(stringr::words, pattern = "^x")
end <- str_detect(stringr::words, pattern = "x$")
# find subset that start or end with x
stringr::words %>% 
  `[`(st | end)
```

```
## [1] "box" "sex" "six" "tax"
```


* Find all words that start with a vowel and end with a consonant.

```r
# single regular expression
str_view_all(stringr::words, "^[aeiou].*[^aeiou]$", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-abc98676aa7a32a518e6" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-abc98676aa7a32a518e6">{"x":{"html":"<ul>\n  <li><span class='match'>about<\/span><\/li>\n  <li><span class='match'>accept<\/span><\/li>\n  <li><span class='match'>account<\/span><\/li>\n  <li><span class='match'>across<\/span><\/li>\n  <li><span class='match'>act<\/span><\/li>\n  <li><span class='match'>actual<\/span><\/li>\n  <li><span class='match'>add<\/span><\/li>\n  <li><span class='match'>address<\/span><\/li>\n  <li><span class='match'>admit<\/span><\/li>\n  <li><span class='match'>affect<\/span><\/li>\n  <li><span class='match'>afford<\/span><\/li>\n  <li><span class='match'>after<\/span><\/li>\n  <li><span class='match'>afternoon<\/span><\/li>\n  <li><span class='match'>again<\/span><\/li>\n  <li><span class='match'>against<\/span><\/li>\n  <li><span class='match'>agent<\/span><\/li>\n  <li><span class='match'>air<\/span><\/li>\n  <li><span class='match'>all<\/span><\/li>\n  <li><span class='match'>allow<\/span><\/li>\n  <li><span class='match'>almost<\/span><\/li>\n  <li><span class='match'>along<\/span><\/li>\n  <li><span class='match'>already<\/span><\/li>\n  <li><span class='match'>alright<\/span><\/li>\n  <li><span class='match'>although<\/span><\/li>\n  <li><span class='match'>always<\/span><\/li>\n  <li><span class='match'>amount<\/span><\/li>\n  <li><span class='match'>and<\/span><\/li>\n  <li><span class='match'>another<\/span><\/li>\n  <li><span class='match'>answer<\/span><\/li>\n  <li><span class='match'>any<\/span><\/li>\n  <li><span class='match'>apart<\/span><\/li>\n  <li><span class='match'>apparent<\/span><\/li>\n  <li><span class='match'>appear<\/span><\/li>\n  <li><span class='match'>apply<\/span><\/li>\n  <li><span class='match'>appoint<\/span><\/li>\n  <li><span class='match'>approach<\/span><\/li>\n  <li><span class='match'>arm<\/span><\/li>\n  <li><span class='match'>around<\/span><\/li>\n  <li><span class='match'>art<\/span><\/li>\n  <li><span class='match'>as<\/span><\/li>\n  <li><span class='match'>ask<\/span><\/li>\n  <li><span class='match'>at<\/span><\/li>\n  <li><span class='match'>attend<\/span><\/li>\n  <li><span class='match'>authority<\/span><\/li>\n  <li><span class='match'>away<\/span><\/li>\n  <li><span class='match'>awful<\/span><\/li>\n  <li><span class='match'>each<\/span><\/li>\n  <li><span class='match'>early<\/span><\/li>\n  <li><span class='match'>east<\/span><\/li>\n  <li><span class='match'>easy<\/span><\/li>\n  <li><span class='match'>eat<\/span><\/li>\n  <li><span class='match'>economy<\/span><\/li>\n  <li><span class='match'>effect<\/span><\/li>\n  <li><span class='match'>egg<\/span><\/li>\n  <li><span class='match'>eight<\/span><\/li>\n  <li><span class='match'>either<\/span><\/li>\n  <li><span class='match'>elect<\/span><\/li>\n  <li><span class='match'>electric<\/span><\/li>\n  <li><span class='match'>eleven<\/span><\/li>\n  <li><span class='match'>employ<\/span><\/li>\n  <li><span class='match'>end<\/span><\/li>\n  <li><span class='match'>english<\/span><\/li>\n  <li><span class='match'>enjoy<\/span><\/li>\n  <li><span class='match'>enough<\/span><\/li>\n  <li><span class='match'>enter<\/span><\/li>\n  <li><span class='match'>environment<\/span><\/li>\n  <li><span class='match'>equal<\/span><\/li>\n  <li><span class='match'>especial<\/span><\/li>\n  <li><span class='match'>even<\/span><\/li>\n  <li><span class='match'>evening<\/span><\/li>\n  <li><span class='match'>ever<\/span><\/li>\n  <li><span class='match'>every<\/span><\/li>\n  <li><span class='match'>exact<\/span><\/li>\n  <li><span class='match'>except<\/span><\/li>\n  <li><span class='match'>exist<\/span><\/li>\n  <li><span class='match'>expect<\/span><\/li>\n  <li><span class='match'>explain<\/span><\/li>\n  <li><span class='match'>express<\/span><\/li>\n  <li><span class='match'>identify<\/span><\/li>\n  <li><span class='match'>if<\/span><\/li>\n  <li><span class='match'>important<\/span><\/li>\n  <li><span class='match'>in<\/span><\/li>\n  <li><span class='match'>indeed<\/span><\/li>\n  <li><span class='match'>individual<\/span><\/li>\n  <li><span class='match'>industry<\/span><\/li>\n  <li><span class='match'>inform<\/span><\/li>\n  <li><span class='match'>instead<\/span><\/li>\n  <li><span class='match'>interest<\/span><\/li>\n  <li><span class='match'>invest<\/span><\/li>\n  <li><span class='match'>it<\/span><\/li>\n  <li><span class='match'>item<\/span><\/li>\n  <li><span class='match'>obvious<\/span><\/li>\n  <li><span class='match'>occasion<\/span><\/li>\n  <li><span class='match'>odd<\/span><\/li>\n  <li><span class='match'>of<\/span><\/li>\n  <li><span class='match'>off<\/span><\/li>\n  <li><span class='match'>offer<\/span><\/li>\n  <li><span class='match'>often<\/span><\/li>\n  <li><span class='match'>okay<\/span><\/li>\n  <li><span class='match'>old<\/span><\/li>\n  <li><span class='match'>on<\/span><\/li>\n  <li><span class='match'>only<\/span><\/li>\n  <li><span class='match'>open<\/span><\/li>\n  <li><span class='match'>opportunity<\/span><\/li>\n  <li><span class='match'>or<\/span><\/li>\n  <li><span class='match'>order<\/span><\/li>\n  <li><span class='match'>original<\/span><\/li>\n  <li><span class='match'>other<\/span><\/li>\n  <li><span class='match'>ought<\/span><\/li>\n  <li><span class='match'>out<\/span><\/li>\n  <li><span class='match'>over<\/span><\/li>\n  <li><span class='match'>own<\/span><\/li>\n  <li><span class='match'>under<\/span><\/li>\n  <li><span class='match'>understand<\/span><\/li>\n  <li><span class='match'>union<\/span><\/li>\n  <li><span class='match'>unit<\/span><\/li>\n  <li><span class='match'>university<\/span><\/li>\n  <li><span class='match'>unless<\/span><\/li>\n  <li><span class='match'>until<\/span><\/li>\n  <li><span class='match'>up<\/span><\/li>\n  <li><span class='match'>upon<\/span><\/li>\n  <li><span class='match'>usual<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

```r
# multiple expression
st <- str_detect(stringr::words, pattern = "^[aeiou]")
end <- str_detect(stringr::words, pattern = "[^aeiou]$")
# find subset that start or end with x
stringr::words %>% 
  `[`(st | end) %>% 
  head()
```

```
## [1] "a"        "able"     "about"    "absolute" "accept"   "account"
```


* Are there any words that contain at least one of each different vowel?

```r
str_view(stringr::words, pattern = "[aeiou].*[aeiou]", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-a11e862ec8b2760ab7aa" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-a11e862ec8b2760ab7aa">{"x":{"html":"<ul>\n  <li><span class='match'>able<\/span><\/li>\n  <li><span class='match'>abou<\/span>t<\/li>\n  <li><span class='match'>absolute<\/span><\/li>\n  <li><span class='match'>acce<\/span>pt<\/li>\n  <li><span class='match'>accou<\/span>nt<\/li>\n  <li><span class='match'>achieve<\/span><\/li>\n  <li><span class='match'>acro<\/span>ss<\/li>\n  <li><span class='match'>active<\/span><\/li>\n  <li><span class='match'>actua<\/span>l<\/li>\n  <li><span class='match'>addre<\/span>ss<\/li>\n  <li><span class='match'>admi<\/span>t<\/li>\n  <li><span class='match'>advertise<\/span><\/li>\n  <li><span class='match'>affe<\/span>ct<\/li>\n  <li><span class='match'>affo<\/span>rd<\/li>\n  <li><span class='match'>afte<\/span>r<\/li>\n  <li><span class='match'>afternoo<\/span>n<\/li>\n  <li><span class='match'>agai<\/span>n<\/li>\n  <li><span class='match'>agai<\/span>nst<\/li>\n  <li><span class='match'>age<\/span><\/li>\n  <li><span class='match'>age<\/span>nt<\/li>\n  <li><span class='match'>ago<\/span><\/li>\n  <li><span class='match'>agree<\/span><\/li>\n  <li><span class='match'>ai<\/span>r<\/li>\n  <li><span class='match'>allo<\/span>w<\/li>\n  <li><span class='match'>almo<\/span>st<\/li>\n  <li><span class='match'>alo<\/span>ng<\/li>\n  <li><span class='match'>alrea<\/span>dy<\/li>\n  <li><span class='match'>alri<\/span>ght<\/li>\n  <li><span class='match'>also<\/span><\/li>\n  <li><span class='match'>althou<\/span>gh<\/li>\n  <li><span class='match'>alwa<\/span>ys<\/li>\n  <li><span class='match'>america<\/span><\/li>\n  <li><span class='match'>amou<\/span>nt<\/li>\n  <li><span class='match'>anothe<\/span>r<\/li>\n  <li><span class='match'>answe<\/span>r<\/li>\n  <li><span class='match'>apa<\/span>rt<\/li>\n  <li><span class='match'>appare<\/span>nt<\/li>\n  <li><span class='match'>appea<\/span>r<\/li>\n  <li><span class='match'>appoi<\/span>nt<\/li>\n  <li><span class='match'>approa<\/span>ch<\/li>\n  <li><span class='match'>appropriate<\/span><\/li>\n  <li><span class='match'>area<\/span><\/li>\n  <li><span class='match'>argue<\/span><\/li>\n  <li><span class='match'>arou<\/span>nd<\/li>\n  <li><span class='match'>arrange<\/span><\/li>\n  <li><span class='match'>associate<\/span><\/li>\n  <li><span class='match'>assume<\/span><\/li>\n  <li><span class='match'>atte<\/span>nd<\/li>\n  <li><span class='match'>authori<\/span>ty<\/li>\n  <li><span class='match'>available<\/span><\/li>\n  <li><span class='match'>aware<\/span><\/li>\n  <li><span class='match'>awa<\/span>y<\/li>\n  <li><span class='match'>awfu<\/span>l<\/li>\n  <li>b<span class='match'>alance<\/span><\/li>\n  <li>b<span class='match'>ase<\/span><\/li>\n  <li>b<span class='match'>asi<\/span>s<\/li>\n  <li>b<span class='match'>ea<\/span>r<\/li>\n  <li>b<span class='match'>ea<\/span>t<\/li>\n  <li>b<span class='match'>eau<\/span>ty<\/li>\n  <li>b<span class='match'>ecause<\/span><\/li>\n  <li>b<span class='match'>ecome<\/span><\/li>\n  <li>b<span class='match'>efore<\/span><\/li>\n  <li>b<span class='match'>egi<\/span>n<\/li>\n  <li>b<span class='match'>ehi<\/span>nd<\/li>\n  <li>b<span class='match'>elieve<\/span><\/li>\n  <li>b<span class='match'>enefi<\/span>t<\/li>\n  <li>b<span class='match'>etwee<\/span>n<\/li>\n  <li>bl<span class='match'>oke<\/span><\/li>\n  <li>bl<span class='match'>oo<\/span>d<\/li>\n  <li>bl<span class='match'>ue<\/span><\/li>\n  <li>b<span class='match'>oa<\/span>rd<\/li>\n  <li>b<span class='match'>oa<\/span>t<\/li>\n  <li>b<span class='match'>oo<\/span>k<\/li>\n  <li>b<span class='match'>othe<\/span>r<\/li>\n  <li>b<span class='match'>ottle<\/span><\/li>\n  <li>b<span class='match'>otto<\/span>m<\/li>\n  <li>br<span class='match'>ea<\/span>k<\/li>\n  <li>br<span class='match'>ie<\/span>f<\/li>\n  <li>br<span class='match'>illia<\/span>nt<\/li>\n  <li>br<span class='match'>itai<\/span>n<\/li>\n  <li>br<span class='match'>othe<\/span>r<\/li>\n  <li>b<span class='match'>udge<\/span>t<\/li>\n  <li>b<span class='match'>ui<\/span>ld<\/li>\n  <li>b<span class='match'>usine<\/span>ss<\/li>\n  <li>c<span class='match'>ake<\/span><\/li>\n  <li>c<span class='match'>are<\/span><\/li>\n  <li>c<span class='match'>ase<\/span><\/li>\n  <li>c<span class='match'>ause<\/span><\/li>\n  <li>c<span class='match'>entre<\/span><\/li>\n  <li>c<span class='match'>ertai<\/span>n<\/li>\n  <li>ch<span class='match'>ai<\/span>r<\/li>\n  <li>ch<span class='match'>airma<\/span>n<\/li>\n  <li>ch<span class='match'>ance<\/span><\/li>\n  <li>ch<span class='match'>ange<\/span><\/li>\n  <li>ch<span class='match'>aracte<\/span>r<\/li>\n  <li>ch<span class='match'>arge<\/span><\/li>\n  <li>ch<span class='match'>ea<\/span>p<\/li>\n  <li>ch<span class='match'>oice<\/span><\/li>\n  <li>ch<span class='match'>oose<\/span><\/li>\n  <li>Chr<span class='match'>istma<\/span>s<\/li>\n  <li>cl<span class='match'>ai<\/span>m<\/li>\n  <li>cl<span class='match'>ea<\/span>n<\/li>\n  <li>cl<span class='match'>ea<\/span>r<\/li>\n  <li>cl<span class='match'>ie<\/span>nt<\/li>\n  <li>cl<span class='match'>ose<\/span><\/li>\n  <li>cl<span class='match'>ose<\/span>s<\/li>\n  <li>cl<span class='match'>othe<\/span><\/li>\n  <li>c<span class='match'>offee<\/span><\/li>\n  <li>c<span class='match'>olleague<\/span><\/li>\n  <li>c<span class='match'>olle<\/span>ct<\/li>\n  <li>c<span class='match'>ollege<\/span><\/li>\n  <li>c<span class='match'>olou<\/span>r<\/li>\n  <li>c<span class='match'>ome<\/span><\/li>\n  <li>c<span class='match'>omme<\/span>nt<\/li>\n  <li>c<span class='match'>ommi<\/span>t<\/li>\n  <li>c<span class='match'>ommittee<\/span><\/li>\n  <li>c<span class='match'>ommo<\/span>n<\/li>\n  <li>c<span class='match'>ommuni<\/span>ty<\/li>\n  <li>c<span class='match'>ompa<\/span>ny<\/li>\n  <li>c<span class='match'>ompare<\/span><\/li>\n  <li>c<span class='match'>omplete<\/span><\/li>\n  <li>c<span class='match'>ompute<\/span><\/li>\n  <li>c<span class='match'>once<\/span>rn<\/li>\n  <li>c<span class='match'>onditio<\/span>n<\/li>\n  <li>c<span class='match'>onfe<\/span>r<\/li>\n  <li>c<span class='match'>onside<\/span>r<\/li>\n  <li>c<span class='match'>onsu<\/span>lt<\/li>\n  <li>c<span class='match'>onta<\/span>ct<\/li>\n  <li>c<span class='match'>ontinue<\/span><\/li>\n  <li>c<span class='match'>ontra<\/span>ct<\/li>\n  <li>c<span class='match'>ontro<\/span>l<\/li>\n  <li>c<span class='match'>onverse<\/span><\/li>\n  <li>c<span class='match'>oo<\/span>k<\/li>\n  <li>c<span class='match'>orne<\/span>r<\/li>\n  <li>c<span class='match'>orre<\/span>ct<\/li>\n  <li>c<span class='match'>ou<\/span>ld<\/li>\n  <li>c<span class='match'>ounci<\/span>l<\/li>\n  <li>c<span class='match'>ou<\/span>nt<\/li>\n  <li>c<span class='match'>ou<\/span>ntry<\/li>\n  <li>c<span class='match'>ou<\/span>nty<\/li>\n  <li>c<span class='match'>ouple<\/span><\/li>\n  <li>c<span class='match'>ourse<\/span><\/li>\n  <li>c<span class='match'>ou<\/span>rt<\/li>\n  <li>c<span class='match'>ove<\/span>r<\/li>\n  <li>cr<span class='match'>eate<\/span><\/li>\n  <li>c<span class='match'>urre<\/span>nt<\/li>\n  <li>d<span class='match'>ange<\/span>r<\/li>\n  <li>d<span class='match'>ate<\/span><\/li>\n  <li>d<span class='match'>ea<\/span>d<\/li>\n  <li>d<span class='match'>ea<\/span>l<\/li>\n  <li>d<span class='match'>ea<\/span>r<\/li>\n  <li>d<span class='match'>ebate<\/span><\/li>\n  <li>d<span class='match'>ecide<\/span><\/li>\n  <li>d<span class='match'>ecisio<\/span>n<\/li>\n  <li>d<span class='match'>ee<\/span>p<\/li>\n  <li>d<span class='match'>efinite<\/span><\/li>\n  <li>d<span class='match'>egree<\/span><\/li>\n  <li>d<span class='match'>epartme<\/span>nt<\/li>\n  <li>d<span class='match'>epe<\/span>nd<\/li>\n  <li>d<span class='match'>escribe<\/span><\/li>\n  <li>d<span class='match'>esi<\/span>gn<\/li>\n  <li>d<span class='match'>etai<\/span>l<\/li>\n  <li>d<span class='match'>evelo<\/span>p<\/li>\n  <li>d<span class='match'>ie<\/span><\/li>\n  <li>d<span class='match'>ifference<\/span><\/li>\n  <li>d<span class='match'>ifficu<\/span>lt<\/li>\n  <li>d<span class='match'>inne<\/span>r<\/li>\n  <li>d<span class='match'>ire<\/span>ct<\/li>\n  <li>d<span class='match'>iscu<\/span>ss<\/li>\n  <li>d<span class='match'>istri<\/span>ct<\/li>\n  <li>d<span class='match'>ivide<\/span><\/li>\n  <li>d<span class='match'>octo<\/span>r<\/li>\n  <li>d<span class='match'>ocume<\/span>nt<\/li>\n  <li>d<span class='match'>oo<\/span>r<\/li>\n  <li>d<span class='match'>ouble<\/span><\/li>\n  <li>d<span class='match'>ou<\/span>bt<\/li>\n  <li>dr<span class='match'>ive<\/span><\/li>\n  <li>d<span class='match'>ue<\/span><\/li>\n  <li>d<span class='match'>uri<\/span>ng<\/li>\n  <li><span class='match'>ea<\/span>ch<\/li>\n  <li><span class='match'>ea<\/span>rly<\/li>\n  <li><span class='match'>ea<\/span>st<\/li>\n  <li><span class='match'>ea<\/span>sy<\/li>\n  <li><span class='match'>ea<\/span>t<\/li>\n  <li><span class='match'>econo<\/span>my<\/li>\n  <li><span class='match'>educate<\/span><\/li>\n  <li><span class='match'>effe<\/span>ct<\/li>\n  <li><span class='match'>ei<\/span>ght<\/li>\n  <li><span class='match'>eithe<\/span>r<\/li>\n  <li><span class='match'>ele<\/span>ct<\/li>\n  <li><span class='match'>electri<\/span>c<\/li>\n  <li><span class='match'>eleve<\/span>n<\/li>\n  <li><span class='match'>else<\/span><\/li>\n  <li><span class='match'>emplo<\/span>y<\/li>\n  <li><span class='match'>encourage<\/span><\/li>\n  <li><span class='match'>engine<\/span><\/li>\n  <li><span class='match'>engli<\/span>sh<\/li>\n  <li><span class='match'>enjo<\/span>y<\/li>\n  <li><span class='match'>enou<\/span>gh<\/li>\n  <li><span class='match'>ente<\/span>r<\/li>\n  <li><span class='match'>environme<\/span>nt<\/li>\n  <li><span class='match'>equa<\/span>l<\/li>\n  <li><span class='match'>especia<\/span>l<\/li>\n  <li><span class='match'>europe<\/span><\/li>\n  <li><span class='match'>eve<\/span>n<\/li>\n  <li><span class='match'>eveni<\/span>ng<\/li>\n  <li><span class='match'>eve<\/span>r<\/li>\n  <li><span class='match'>eve<\/span>ry<\/li>\n  <li><span class='match'>evidence<\/span><\/li>\n  <li><span class='match'>exa<\/span>ct<\/li>\n  <li><span class='match'>example<\/span><\/li>\n  <li><span class='match'>exce<\/span>pt<\/li>\n  <li><span class='match'>excuse<\/span><\/li>\n  <li><span class='match'>exercise<\/span><\/li>\n  <li><span class='match'>exi<\/span>st<\/li>\n  <li><span class='match'>expe<\/span>ct<\/li>\n  <li><span class='match'>expense<\/span><\/li>\n  <li><span class='match'>experience<\/span><\/li>\n  <li><span class='match'>explai<\/span>n<\/li>\n  <li><span class='match'>expre<\/span>ss<\/li>\n  <li><span class='match'>extra<\/span><\/li>\n  <li><span class='match'>eye<\/span><\/li>\n  <li>f<span class='match'>ace<\/span><\/li>\n  <li>f<span class='match'>ai<\/span>r<\/li>\n  <li>f<span class='match'>ami<\/span>ly<\/li>\n  <li>f<span class='match'>athe<\/span>r<\/li>\n  <li>f<span class='match'>avou<\/span>r<\/li>\n  <li>f<span class='match'>ee<\/span>d<\/li>\n  <li>f<span class='match'>ee<\/span>l<\/li>\n  <li>f<span class='match'>ie<\/span>ld<\/li>\n  <li>f<span class='match'>igure<\/span><\/li>\n  <li>f<span class='match'>ile<\/span><\/li>\n  <li>f<span class='match'>ina<\/span>l<\/li>\n  <li>f<span class='match'>inance<\/span><\/li>\n  <li>f<span class='match'>ine<\/span><\/li>\n  <li>f<span class='match'>ini<\/span>sh<\/li>\n  <li>f<span class='match'>ire<\/span><\/li>\n  <li>f<span class='match'>ive<\/span><\/li>\n  <li>fl<span class='match'>oo<\/span>r<\/li>\n  <li>f<span class='match'>ollo<\/span>w<\/li>\n  <li>f<span class='match'>oo<\/span>d<\/li>\n  <li>f<span class='match'>oo<\/span>t<\/li>\n  <li>f<span class='match'>orce<\/span><\/li>\n  <li>f<span class='match'>orge<\/span>t<\/li>\n  <li>f<span class='match'>ortune<\/span><\/li>\n  <li>f<span class='match'>orwa<\/span>rd<\/li>\n  <li>f<span class='match'>ou<\/span>r<\/li>\n  <li>fr<span class='match'>ance<\/span><\/li>\n  <li>fr<span class='match'>ee<\/span><\/li>\n  <li>fr<span class='match'>ida<\/span>y<\/li>\n  <li>fr<span class='match'>ie<\/span>nd<\/li>\n  <li>f<span class='match'>unctio<\/span>n<\/li>\n  <li>f<span class='match'>urthe<\/span>r<\/li>\n  <li>f<span class='match'>uture<\/span><\/li>\n  <li>g<span class='match'>ame<\/span><\/li>\n  <li>g<span class='match'>arde<\/span>n<\/li>\n  <li>g<span class='match'>enera<\/span>l<\/li>\n  <li>g<span class='match'>erma<\/span>ny<\/li>\n  <li>g<span class='match'>ive<\/span><\/li>\n  <li>g<span class='match'>oo<\/span>d<\/li>\n  <li>g<span class='match'>oodbye<\/span><\/li>\n  <li>g<span class='match'>ove<\/span>rn<\/li>\n  <li>gr<span class='match'>ea<\/span>t<\/li>\n  <li>gr<span class='match'>ee<\/span>n<\/li>\n  <li>gr<span class='match'>ou<\/span>nd<\/li>\n  <li>gr<span class='match'>ou<\/span>p<\/li>\n  <li>g<span class='match'>ue<\/span>ss<\/li>\n  <li>h<span class='match'>ai<\/span>r<\/li>\n  <li>h<span class='match'>appe<\/span>n<\/li>\n  <li>h<span class='match'>ate<\/span><\/li>\n  <li>h<span class='match'>ave<\/span><\/li>\n  <li>h<span class='match'>ea<\/span>d<\/li>\n  <li>h<span class='match'>ea<\/span>lth<\/li>\n  <li>h<span class='match'>ea<\/span>r<\/li>\n  <li>h<span class='match'>ea<\/span>rt<\/li>\n  <li>h<span class='match'>ea<\/span>t<\/li>\n  <li>h<span class='match'>ea<\/span>vy<\/li>\n  <li>h<span class='match'>ere<\/span><\/li>\n  <li>h<span class='match'>isto<\/span>ry<\/li>\n  <li>h<span class='match'>olida<\/span>y<\/li>\n  <li>h<span class='match'>ome<\/span><\/li>\n  <li>h<span class='match'>one<\/span>st<\/li>\n  <li>h<span class='match'>ope<\/span><\/li>\n  <li>h<span class='match'>orse<\/span><\/li>\n  <li>h<span class='match'>ospita<\/span>l<\/li>\n  <li>h<span class='match'>ou<\/span>r<\/li>\n  <li>h<span class='match'>ouse<\/span><\/li>\n  <li>h<span class='match'>oweve<\/span>r<\/li>\n  <li>h<span class='match'>ullo<\/span><\/li>\n  <li>h<span class='match'>undre<\/span>d<\/li>\n  <li>h<span class='match'>usba<\/span>nd<\/li>\n  <li><span class='match'>idea<\/span><\/li>\n  <li><span class='match'>identi<\/span>fy<\/li>\n  <li><span class='match'>imagine<\/span><\/li>\n  <li><span class='match'>importa<\/span>nt<\/li>\n  <li><span class='match'>improve<\/span><\/li>\n  <li><span class='match'>include<\/span><\/li>\n  <li><span class='match'>income<\/span><\/li>\n  <li><span class='match'>increase<\/span><\/li>\n  <li><span class='match'>indee<\/span>d<\/li>\n  <li><span class='match'>individua<\/span>l<\/li>\n  <li><span class='match'>indu<\/span>stry<\/li>\n  <li><span class='match'>info<\/span>rm<\/li>\n  <li><span class='match'>inside<\/span><\/li>\n  <li><span class='match'>instea<\/span>d<\/li>\n  <li><span class='match'>insure<\/span><\/li>\n  <li><span class='match'>intere<\/span>st<\/li>\n  <li><span class='match'>into<\/span><\/li>\n  <li><span class='match'>introduce<\/span><\/li>\n  <li><span class='match'>inve<\/span>st<\/li>\n  <li><span class='match'>involve<\/span><\/li>\n  <li><span class='match'>issue<\/span><\/li>\n  <li><span class='match'>ite<\/span>m<\/li>\n  <li>j<span class='match'>esu<\/span>s<\/li>\n  <li>j<span class='match'>oi<\/span>n<\/li>\n  <li>j<span class='match'>udge<\/span><\/li>\n  <li>k<span class='match'>ee<\/span>p<\/li>\n  <li>k<span class='match'>itche<\/span>n<\/li>\n  <li>l<span class='match'>abou<\/span>r<\/li>\n  <li>l<span class='match'>anguage<\/span><\/li>\n  <li>l<span class='match'>arge<\/span><\/li>\n  <li>l<span class='match'>ate<\/span><\/li>\n  <li>l<span class='match'>au<\/span>gh<\/li>\n  <li>l<span class='match'>ea<\/span>d<\/li>\n  <li>l<span class='match'>ea<\/span>rn<\/li>\n  <li>l<span class='match'>eave<\/span><\/li>\n  <li>l<span class='match'>ette<\/span>r<\/li>\n  <li>l<span class='match'>eve<\/span>l<\/li>\n  <li>l<span class='match'>ie<\/span><\/li>\n  <li>l<span class='match'>ife<\/span><\/li>\n  <li>l<span class='match'>ike<\/span><\/li>\n  <li>l<span class='match'>ike<\/span>ly<\/li>\n  <li>l<span class='match'>imi<\/span>t<\/li>\n  <li>l<span class='match'>ine<\/span><\/li>\n  <li>l<span class='match'>iste<\/span>n<\/li>\n  <li>l<span class='match'>ittle<\/span><\/li>\n  <li>l<span class='match'>ive<\/span><\/li>\n  <li>l<span class='match'>oa<\/span>d<\/li>\n  <li>l<span class='match'>oca<\/span>l<\/li>\n  <li>l<span class='match'>ondo<\/span>n<\/li>\n  <li>l<span class='match'>oo<\/span>k<\/li>\n  <li>l<span class='match'>ose<\/span><\/li>\n  <li>l<span class='match'>ove<\/span><\/li>\n  <li>m<span class='match'>achine<\/span><\/li>\n  <li>m<span class='match'>ai<\/span>n<\/li>\n  <li>m<span class='match'>ajo<\/span>r<\/li>\n  <li>m<span class='match'>ake<\/span><\/li>\n  <li>m<span class='match'>anage<\/span><\/li>\n  <li>m<span class='match'>arke<\/span>t<\/li>\n  <li>m<span class='match'>atte<\/span>r<\/li>\n  <li>m<span class='match'>aybe<\/span><\/li>\n  <li>m<span class='match'>ea<\/span>n<\/li>\n  <li>m<span class='match'>eani<\/span>ng<\/li>\n  <li>m<span class='match'>easure<\/span><\/li>\n  <li>m<span class='match'>ee<\/span>t<\/li>\n  <li>m<span class='match'>embe<\/span>r<\/li>\n  <li>m<span class='match'>entio<\/span>n<\/li>\n  <li>m<span class='match'>iddle<\/span><\/li>\n  <li>m<span class='match'>ile<\/span><\/li>\n  <li>m<span class='match'>illio<\/span>n<\/li>\n  <li>m<span class='match'>iniste<\/span>r<\/li>\n  <li>m<span class='match'>inu<\/span>s<\/li>\n  <li>m<span class='match'>inute<\/span><\/li>\n  <li>m<span class='match'>iste<\/span>r<\/li>\n  <li>m<span class='match'>ome<\/span>nt<\/li>\n  <li>m<span class='match'>onda<\/span>y<\/li>\n  <li>m<span class='match'>one<\/span>y<\/li>\n  <li>m<span class='match'>ore<\/span><\/li>\n  <li>m<span class='match'>orni<\/span>ng<\/li>\n  <li>m<span class='match'>othe<\/span>r<\/li>\n  <li>m<span class='match'>otio<\/span>n<\/li>\n  <li>m<span class='match'>ove<\/span><\/li>\n  <li>m<span class='match'>usi<\/span>c<\/li>\n  <li>n<span class='match'>ame<\/span><\/li>\n  <li>n<span class='match'>atio<\/span>n<\/li>\n  <li>n<span class='match'>ature<\/span><\/li>\n  <li>n<span class='match'>ea<\/span>r<\/li>\n  <li>n<span class='match'>ecessa<\/span>ry<\/li>\n  <li>n<span class='match'>ee<\/span>d<\/li>\n  <li>n<span class='match'>eve<\/span>r<\/li>\n  <li>n<span class='match'>ice<\/span><\/li>\n  <li>n<span class='match'>ine<\/span><\/li>\n  <li>n<span class='match'>one<\/span><\/li>\n  <li>n<span class='match'>orma<\/span>l<\/li>\n  <li>n<span class='match'>ote<\/span><\/li>\n  <li>n<span class='match'>otice<\/span><\/li>\n  <li>n<span class='match'>umbe<\/span>r<\/li>\n  <li><span class='match'>obviou<\/span>s<\/li>\n  <li><span class='match'>occasio<\/span>n<\/li>\n  <li><span class='match'>offe<\/span>r<\/li>\n  <li><span class='match'>office<\/span><\/li>\n  <li><span class='match'>ofte<\/span>n<\/li>\n  <li><span class='match'>oka<\/span>y<\/li>\n  <li><span class='match'>once<\/span><\/li>\n  <li><span class='match'>one<\/span><\/li>\n  <li><span class='match'>ope<\/span>n<\/li>\n  <li><span class='match'>operate<\/span><\/li>\n  <li><span class='match'>opportuni<\/span>ty<\/li>\n  <li><span class='match'>oppose<\/span><\/li>\n  <li><span class='match'>orde<\/span>r<\/li>\n  <li><span class='match'>organize<\/span><\/li>\n  <li><span class='match'>origina<\/span>l<\/li>\n  <li><span class='match'>othe<\/span>r<\/li>\n  <li><span class='match'>otherwise<\/span><\/li>\n  <li><span class='match'>ou<\/span>ght<\/li>\n  <li><span class='match'>ou<\/span>t<\/li>\n  <li><span class='match'>ove<\/span>r<\/li>\n  <li>p<span class='match'>age<\/span><\/li>\n  <li>p<span class='match'>ai<\/span>nt<\/li>\n  <li>p<span class='match'>ai<\/span>r<\/li>\n  <li>p<span class='match'>ape<\/span>r<\/li>\n  <li>p<span class='match'>aragra<\/span>ph<\/li>\n  <li>p<span class='match'>ardo<\/span>n<\/li>\n  <li>p<span class='match'>are<\/span>nt<\/li>\n  <li>p<span class='match'>articula<\/span>r<\/li>\n  <li>p<span class='match'>ence<\/span><\/li>\n  <li>p<span class='match'>ensio<\/span>n<\/li>\n  <li>p<span class='match'>eople<\/span><\/li>\n  <li>p<span class='match'>erce<\/span>nt<\/li>\n  <li>p<span class='match'>erfe<\/span>ct<\/li>\n  <li>p<span class='match'>erha<\/span>ps<\/li>\n  <li>p<span class='match'>erio<\/span>d<\/li>\n  <li>p<span class='match'>erso<\/span>n<\/li>\n  <li>ph<span class='match'>otogra<\/span>ph<\/li>\n  <li>p<span class='match'>icture<\/span><\/li>\n  <li>p<span class='match'>iece<\/span><\/li>\n  <li>pl<span class='match'>ace<\/span><\/li>\n  <li>pl<span class='match'>ease<\/span><\/li>\n  <li>p<span class='match'>oi<\/span>nt<\/li>\n  <li>p<span class='match'>olice<\/span><\/li>\n  <li>p<span class='match'>oli<\/span>cy<\/li>\n  <li>p<span class='match'>oliti<\/span>c<\/li>\n  <li>p<span class='match'>oo<\/span>r<\/li>\n  <li>p<span class='match'>ositio<\/span>n<\/li>\n  <li>p<span class='match'>ositive<\/span><\/li>\n  <li>p<span class='match'>ossible<\/span><\/li>\n  <li>p<span class='match'>ou<\/span>nd<\/li>\n  <li>p<span class='match'>owe<\/span>r<\/li>\n  <li>pr<span class='match'>actise<\/span><\/li>\n  <li>pr<span class='match'>epare<\/span><\/li>\n  <li>pr<span class='match'>ese<\/span>nt<\/li>\n  <li>pr<span class='match'>essure<\/span><\/li>\n  <li>pr<span class='match'>esume<\/span><\/li>\n  <li>pr<span class='match'>eviou<\/span>s<\/li>\n  <li>pr<span class='match'>ice<\/span><\/li>\n  <li>pr<span class='match'>ivate<\/span><\/li>\n  <li>pr<span class='match'>obable<\/span><\/li>\n  <li>pr<span class='match'>oble<\/span>m<\/li>\n  <li>pr<span class='match'>ocee<\/span>d<\/li>\n  <li>pr<span class='match'>oce<\/span>ss<\/li>\n  <li>pr<span class='match'>oduce<\/span><\/li>\n  <li>pr<span class='match'>odu<\/span>ct<\/li>\n  <li>pr<span class='match'>ogramme<\/span><\/li>\n  <li>pr<span class='match'>oje<\/span>ct<\/li>\n  <li>pr<span class='match'>ope<\/span>r<\/li>\n  <li>pr<span class='match'>opose<\/span><\/li>\n  <li>pr<span class='match'>ote<\/span>ct<\/li>\n  <li>pr<span class='match'>ovide<\/span><\/li>\n  <li>p<span class='match'>ubli<\/span>c<\/li>\n  <li>p<span class='match'>urpose<\/span><\/li>\n  <li>q<span class='match'>uali<\/span>ty<\/li>\n  <li>q<span class='match'>uarte<\/span>r<\/li>\n  <li>q<span class='match'>uestio<\/span>n<\/li>\n  <li>q<span class='match'>ui<\/span>ck<\/li>\n  <li>q<span class='match'>ui<\/span>d<\/li>\n  <li>q<span class='match'>uie<\/span>t<\/li>\n  <li>q<span class='match'>uite<\/span><\/li>\n  <li>r<span class='match'>adio<\/span><\/li>\n  <li>r<span class='match'>ai<\/span>l<\/li>\n  <li>r<span class='match'>aise<\/span><\/li>\n  <li>r<span class='match'>ange<\/span><\/li>\n  <li>r<span class='match'>ate<\/span><\/li>\n  <li>r<span class='match'>athe<\/span>r<\/li>\n  <li>r<span class='match'>ea<\/span>d<\/li>\n  <li>r<span class='match'>ea<\/span>dy<\/li>\n  <li>r<span class='match'>ea<\/span>l<\/li>\n  <li>r<span class='match'>ealise<\/span><\/li>\n  <li>r<span class='match'>ea<\/span>lly<\/li>\n  <li>r<span class='match'>easo<\/span>n<\/li>\n  <li>r<span class='match'>eceive<\/span><\/li>\n  <li>r<span class='match'>ece<\/span>nt<\/li>\n  <li>r<span class='match'>ecko<\/span>n<\/li>\n  <li>r<span class='match'>ecognize<\/span><\/li>\n  <li>r<span class='match'>ecomme<\/span>nd<\/li>\n  <li>r<span class='match'>eco<\/span>rd<\/li>\n  <li>r<span class='match'>educe<\/span><\/li>\n  <li>r<span class='match'>efe<\/span>r<\/li>\n  <li>r<span class='match'>ega<\/span>rd<\/li>\n  <li>r<span class='match'>egio<\/span>n<\/li>\n  <li>r<span class='match'>elatio<\/span>n<\/li>\n  <li>r<span class='match'>emembe<\/span>r<\/li>\n  <li>r<span class='match'>epo<\/span>rt<\/li>\n  <li>r<span class='match'>eprese<\/span>nt<\/li>\n  <li>r<span class='match'>equire<\/span><\/li>\n  <li>r<span class='match'>esea<\/span>rch<\/li>\n  <li>r<span class='match'>esource<\/span><\/li>\n  <li>r<span class='match'>espe<\/span>ct<\/li>\n  <li>r<span class='match'>esponsible<\/span><\/li>\n  <li>r<span class='match'>esu<\/span>lt<\/li>\n  <li>r<span class='match'>etu<\/span>rn<\/li>\n  <li>r<span class='match'>ise<\/span><\/li>\n  <li>r<span class='match'>oa<\/span>d<\/li>\n  <li>r<span class='match'>ole<\/span><\/li>\n  <li>r<span class='match'>oo<\/span>m<\/li>\n  <li>r<span class='match'>ou<\/span>nd<\/li>\n  <li>r<span class='match'>ule<\/span><\/li>\n  <li>s<span class='match'>afe<\/span><\/li>\n  <li>s<span class='match'>ale<\/span><\/li>\n  <li>s<span class='match'>ame<\/span><\/li>\n  <li>s<span class='match'>aturda<\/span>y<\/li>\n  <li>s<span class='match'>ave<\/span><\/li>\n  <li>sch<span class='match'>eme<\/span><\/li>\n  <li>sch<span class='match'>oo<\/span>l<\/li>\n  <li>sc<span class='match'>ience<\/span><\/li>\n  <li>sc<span class='match'>ore<\/span><\/li>\n  <li>sc<span class='match'>otla<\/span>nd<\/li>\n  <li>s<span class='match'>ea<\/span>t<\/li>\n  <li>s<span class='match'>eco<\/span>nd<\/li>\n  <li>s<span class='match'>ecreta<\/span>ry<\/li>\n  <li>s<span class='match'>ectio<\/span>n<\/li>\n  <li>s<span class='match'>ecure<\/span><\/li>\n  <li>s<span class='match'>ee<\/span><\/li>\n  <li>s<span class='match'>ee<\/span>m<\/li>\n  <li>s<span class='match'>ense<\/span><\/li>\n  <li>s<span class='match'>eparate<\/span><\/li>\n  <li>s<span class='match'>eriou<\/span>s<\/li>\n  <li>s<span class='match'>erve<\/span><\/li>\n  <li>s<span class='match'>ervice<\/span><\/li>\n  <li>s<span class='match'>ettle<\/span><\/li>\n  <li>s<span class='match'>eve<\/span>n<\/li>\n  <li>sh<span class='match'>are<\/span><\/li>\n  <li>sh<span class='match'>ee<\/span>t<\/li>\n  <li>sh<span class='match'>oe<\/span><\/li>\n  <li>sh<span class='match'>oo<\/span>t<\/li>\n  <li>sh<span class='match'>ou<\/span>ld<\/li>\n  <li>s<span class='match'>ide<\/span><\/li>\n  <li>s<span class='match'>imila<\/span>r<\/li>\n  <li>s<span class='match'>imple<\/span><\/li>\n  <li>s<span class='match'>ince<\/span><\/li>\n  <li>s<span class='match'>ingle<\/span><\/li>\n  <li>s<span class='match'>iste<\/span>r<\/li>\n  <li>s<span class='match'>ite<\/span><\/li>\n  <li>s<span class='match'>ituate<\/span><\/li>\n  <li>s<span class='match'>ize<\/span><\/li>\n  <li>sl<span class='match'>ee<\/span>p<\/li>\n  <li>sm<span class='match'>oke<\/span><\/li>\n  <li>s<span class='match'>ocia<\/span>l<\/li>\n  <li>s<span class='match'>ocie<\/span>ty<\/li>\n  <li>s<span class='match'>ome<\/span><\/li>\n  <li>s<span class='match'>oo<\/span>n<\/li>\n  <li>s<span class='match'>ou<\/span>nd<\/li>\n  <li>s<span class='match'>ou<\/span>th<\/li>\n  <li>sp<span class='match'>ace<\/span><\/li>\n  <li>sp<span class='match'>ea<\/span>k<\/li>\n  <li>sp<span class='match'>ecia<\/span>l<\/li>\n  <li>sp<span class='match'>ecifi<\/span>c<\/li>\n  <li>sp<span class='match'>ee<\/span>d<\/li>\n  <li>sq<span class='match'>uare<\/span><\/li>\n  <li>st<span class='match'>age<\/span><\/li>\n  <li>st<span class='match'>ai<\/span>rs<\/li>\n  <li>st<span class='match'>anda<\/span>rd<\/li>\n  <li>st<span class='match'>ate<\/span><\/li>\n  <li>st<span class='match'>atio<\/span>n<\/li>\n  <li>str<span class='match'>ai<\/span>ght<\/li>\n  <li>str<span class='match'>ate<\/span>gy<\/li>\n  <li>str<span class='match'>ee<\/span>t<\/li>\n  <li>str<span class='match'>ike<\/span><\/li>\n  <li>str<span class='match'>ucture<\/span><\/li>\n  <li>st<span class='match'>ude<\/span>nt<\/li>\n  <li>st<span class='match'>upi<\/span>d<\/li>\n  <li>s<span class='match'>ubje<\/span>ct<\/li>\n  <li>s<span class='match'>uccee<\/span>d<\/li>\n  <li>s<span class='match'>udde<\/span>n<\/li>\n  <li>s<span class='match'>ugge<\/span>st<\/li>\n  <li>s<span class='match'>ui<\/span>t<\/li>\n  <li>s<span class='match'>umme<\/span>r<\/li>\n  <li>s<span class='match'>unda<\/span>y<\/li>\n  <li>s<span class='match'>uppo<\/span>rt<\/li>\n  <li>s<span class='match'>uppose<\/span><\/li>\n  <li>s<span class='match'>ure<\/span><\/li>\n  <li>s<span class='match'>urprise<\/span><\/li>\n  <li>t<span class='match'>able<\/span><\/li>\n  <li>t<span class='match'>ake<\/span><\/li>\n  <li>t<span class='match'>ape<\/span><\/li>\n  <li>t<span class='match'>ea<\/span><\/li>\n  <li>t<span class='match'>ea<\/span>ch<\/li>\n  <li>t<span class='match'>ea<\/span>m<\/li>\n  <li>t<span class='match'>elephone<\/span><\/li>\n  <li>t<span class='match'>elevisio<\/span>n<\/li>\n  <li>t<span class='match'>errible<\/span><\/li>\n  <li>th<span class='match'>ere<\/span><\/li>\n  <li>th<span class='match'>erefore<\/span><\/li>\n  <li>th<span class='match'>irtee<\/span>n<\/li>\n  <li>th<span class='match'>ou<\/span><\/li>\n  <li>th<span class='match'>ou<\/span>gh<\/li>\n  <li>th<span class='match'>ousa<\/span>nd<\/li>\n  <li>thr<span class='match'>ee<\/span><\/li>\n  <li>thr<span class='match'>ou<\/span>gh<\/li>\n  <li>th<span class='match'>ursda<\/span>y<\/li>\n  <li>t<span class='match'>ie<\/span><\/li>\n  <li>t<span class='match'>ime<\/span><\/li>\n  <li>t<span class='match'>oda<\/span>y<\/li>\n  <li>t<span class='match'>ogethe<\/span>r<\/li>\n  <li>t<span class='match'>omorro<\/span>w<\/li>\n  <li>t<span class='match'>oni<\/span>ght<\/li>\n  <li>t<span class='match'>oo<\/span><\/li>\n  <li>t<span class='match'>ota<\/span>l<\/li>\n  <li>t<span class='match'>ou<\/span>ch<\/li>\n  <li>t<span class='match'>owa<\/span>rd<\/li>\n  <li>tr<span class='match'>ade<\/span><\/li>\n  <li>tr<span class='match'>affi<\/span>c<\/li>\n  <li>tr<span class='match'>ai<\/span>n<\/li>\n  <li>tr<span class='match'>anspo<\/span>rt<\/li>\n  <li>tr<span class='match'>ave<\/span>l<\/li>\n  <li>tr<span class='match'>ea<\/span>t<\/li>\n  <li>tr<span class='match'>ee<\/span><\/li>\n  <li>tr<span class='match'>ouble<\/span><\/li>\n  <li>tr<span class='match'>ue<\/span><\/li>\n  <li>t<span class='match'>uesda<\/span>y<\/li>\n  <li>tw<span class='match'>elve<\/span><\/li>\n  <li><span class='match'>unde<\/span>r<\/li>\n  <li><span class='match'>understa<\/span>nd<\/li>\n  <li><span class='match'>unio<\/span>n<\/li>\n  <li><span class='match'>uni<\/span>t<\/li>\n  <li><span class='match'>unite<\/span><\/li>\n  <li><span class='match'>universi<\/span>ty<\/li>\n  <li><span class='match'>unle<\/span>ss<\/li>\n  <li><span class='match'>unti<\/span>l<\/li>\n  <li><span class='match'>upo<\/span>n<\/li>\n  <li><span class='match'>use<\/span><\/li>\n  <li><span class='match'>usua<\/span>l<\/li>\n  <li>v<span class='match'>alue<\/span><\/li>\n  <li>v<span class='match'>ariou<\/span>s<\/li>\n  <li>v<span class='match'>ideo<\/span><\/li>\n  <li>v<span class='match'>ie<\/span>w<\/li>\n  <li>v<span class='match'>illage<\/span><\/li>\n  <li>v<span class='match'>isi<\/span>t<\/li>\n  <li>v<span class='match'>ote<\/span><\/li>\n  <li>w<span class='match'>age<\/span><\/li>\n  <li>w<span class='match'>ai<\/span>t<\/li>\n  <li>w<span class='match'>aste<\/span><\/li>\n  <li>w<span class='match'>ate<\/span>r<\/li>\n  <li>w<span class='match'>ea<\/span>r<\/li>\n  <li>w<span class='match'>ednesda<\/span>y<\/li>\n  <li>w<span class='match'>ee<\/span><\/li>\n  <li>w<span class='match'>ee<\/span>k<\/li>\n  <li>w<span class='match'>ei<\/span>gh<\/li>\n  <li>w<span class='match'>elcome<\/span><\/li>\n  <li>wh<span class='match'>ere<\/span><\/li>\n  <li>wh<span class='match'>ethe<\/span>r<\/li>\n  <li>wh<span class='match'>ile<\/span><\/li>\n  <li>wh<span class='match'>ite<\/span><\/li>\n  <li>wh<span class='match'>ole<\/span><\/li>\n  <li>w<span class='match'>ide<\/span><\/li>\n  <li>w<span class='match'>ife<\/span><\/li>\n  <li>w<span class='match'>indo<\/span>w<\/li>\n  <li>w<span class='match'>ithi<\/span>n<\/li>\n  <li>w<span class='match'>ithou<\/span>t<\/li>\n  <li>w<span class='match'>oma<\/span>n<\/li>\n  <li>w<span class='match'>onde<\/span>r<\/li>\n  <li>w<span class='match'>oo<\/span>d<\/li>\n  <li>w<span class='match'>orse<\/span><\/li>\n  <li>w<span class='match'>ou<\/span>ld<\/li>\n  <li>wr<span class='match'>ite<\/span><\/li>\n  <li>y<span class='match'>ea<\/span>r<\/li>\n  <li>y<span class='match'>esterda<\/span>y<\/li>\n  <li>y<span class='match'>ou<\/span><\/li>\n  <li>y<span class='match'>ou<\/span>ng<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

## 14.4.3.1 Exercises

1. In the previous example, you might have noticed that the regular expression matched “flickered”, which is not a colour. Modify the regex to fix the problem.




## 14.4.4.1 Exercises




#Task 2


__1__
First we extract the data for Canada to work on the code

```r
chosen_country <- "Canada"
(chosen_data <- gapminder %>% 
  filter(country == chosen_country))
```

```
## # A tibble: 12 x 6
##    country continent  year lifeExp      pop gdpPercap
##    <fct>   <fct>     <int>   <dbl>    <int>     <dbl>
##  1 Canada  Americas   1952    68.8 14785584    11367.
##  2 Canada  Americas   1957    70.0 17010154    12490.
##  3 Canada  Americas   1962    71.3 18985849    13462.
##  4 Canada  Americas   1967    72.1 20819767    16077.
##  5 Canada  Americas   1972    72.9 22284500    18971.
##  6 Canada  Americas   1977    74.2 23796400    22091.
##  7 Canada  Americas   1982    75.8 25201900    22899.
##  8 Canada  Americas   1987    76.9 26549700    26627.
##  9 Canada  Americas   1992    78.0 28523502    26343.
## 10 Canada  Americas   1997    78.6 30305843    28955.
## 11 Canada  Americas   2002    79.8 31902268    33329.
## 12 Canada  Americas   2007    80.7 33390141    36319.
```
Then we plot the graph of gdp per capita against population
and we can use a polynomial function instead of a linear function to better fit the data. 

```r
p <- ggplot(chosen_data, aes(x = pop, y = gdpPercap))
p + geom_point() + geom_smooth(method = 'lm', se = FALSE)
```

![](assignment_6_files/figure-html/unnamed-chunk-41-1.png)<!-- -->

We fit the data with a cubic function, and the coefficient of x with different degree is shown: 

```r
cub_fit <- rlm(gdpPercap ~ year+I(year^2)+I(year^3),chosen_data)
coef(cub_fit)
```

```
##   (Intercept)          year     I(year^2)     I(year^3) 
## -1.888527e+08  2.895614e+05 -1.481768e+02  2.530862e-02
```
we can then use then plot the cubic function 

```r
 curve(predict(cub_fit,data.frame(year=x)),col='blue',lwd=2) 
```

![](assignment_6_files/figure-html/unnamed-chunk-43-1.png)<!-- -->


Then we now sum up the above codes to become a function and try out the data above:
with the below function, by inputing the corresponding country name, we could get the cubic regression for the gdp per capita against population for this country.


```r
cubic_curve_fit  <-  function (chosen_country){
  chosen_data <- gapminder %>% 
  filter(country == chosen_country)
  
  fit_curve <- rlm(gdpPercap ~ year+I(year^2)+I(year^3), chosen_data)
  setNames(coef(fit_curve),c("intercept","x","x^2","x^3"))
}

cubic_curve_fit("Canada")
```

```
##     intercept             x           x^2           x^3 
## -1.888527e+08  2.895614e+05 -1.481768e+02  2.530862e-02
```

Again we can try to use this on other countries:

```r
cubic_curve_fit("France")
```

```
##     intercept             x           x^2           x^3 
##  2.489145e+08 -3.793022e+05  1.924422e+02 -3.250638e-02
```

```r
cubic_curve_fit("Afghanistan")
```

```
##     intercept             x           x^2           x^3 
## -1.053021e+08  1.597388e+05 -8.076768e+01  1.361201e-02
```

```r
cubic_curve_fit("Japan")
```

```
##     intercept             x           x^2           x^3 
##  1.680453e+09 -2.550644e+06  1.290149e+03 -2.174665e-01
```


## Task 4 Work With the `singer` data

__Use purrr to map latitude and longitude into human readable information on the band’s origin places.__

First, we use head() function to visulise the singer_location dataframe. We realised that there are quite a lot of `NA` appeared in the entry. We need to get rid of these before tidy up the data. 

```r
head(singer_locations)
```

```
## # A tibble: 6 x 14
##   track_id title song_id release artist_id artist_name  year duration
##   <chr>    <chr> <chr>   <chr>   <chr>     <chr>       <int>    <dbl>
## 1 TRWICRA~ The ~ SOSURT~ Even I~ ARACDPV1~ Motion Cit~  2007     170.
## 2 TRXJANY~ Lone~ SODESQ~ The Du~ ARYBUAO1~ Gene Chand~  2004     107.
## 3 TRIKPCA~ Here~ SOQUYQ~ Improm~ AR4111G1~ Paul Horn    1998     528.
## 4 TRYEATD~ Rego~ SOEZGR~ Still ~ ARQDZP31~ Ronnie Ear~  1995     695.
## 5 TRBYYXH~ Games SOPIOC~ Afro-H~ AR75GYU1~ Dorothy As~  1968     237.
## 6 TRKFFKR~ More~ SOHQSP~ Six Ya~ ARCENE01~ Barleyjuice  2006     193.
## # ... with 6 more variables: artist_hotttnesss <dbl>,
## #   artist_familiarity <dbl>, latitude <dbl>, longitude <dbl>, name <chr>,
## #   city <chr>
```

```r
tidy_data <- singer_locations %>% 
  filter(!is.na(latitude)|!is.na(longitude)|!is.na(city)) %>% 
  dplyr::select(name,city,longitude,latitude)
# while we load the library MASS together with the dplyr, we need to use dplyr::select to call this function
head(tidy_data) %>% 
  knitr::kable()
```



name                       city             longitude   latitude
-------------------------  -------------  -----------  ---------
Gene Chandler              Chicago, IL      -87.63241   41.88415
Paul Horn                  New York, NY     -74.00712   40.71455
Dorothy Ashby              Detroit, MI      -83.04792   42.33168
Barleyjuice                Pennsylvania     -77.60454   40.99471
Madlib                     Oxnard, CA      -119.18044   34.20034
Seeed feat. Elephant Man   Bonn               7.10169   50.73230
next we are going to map the longitude and latitude to the band's origin place, and the function used in the mapping is `map2()` and the `revgeocode()` from `ggmap` is used to reverse a geo code location using Google Maps.

```r
# location <- map2_chr(tidy_data$longitude[30:100],tidy_data$latitude[30:100], ~revgeocode(as.numeric(c(.x,.y)), output ="more" , source = "google"))
```


