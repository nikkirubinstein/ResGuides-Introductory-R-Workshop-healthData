


# Control flow



<!--sec data-title="Learning Objectives" data-id="obj" data-show=true data-collapse=false ces-->

* Write conditional statements with `if` and `else`.
* Write and understand `for` loops.

<!--endsec-->

<br>

---

**Table of Contents**

<!-- toc -->

<br>

---

## Conditionals

Often when we're coding we want to control the flow of our actions. This can be done
by setting actions to occur only if a condition or a set of conditions are met.
Alternatively, we can also set an action to occur a particular number of times.

There are several ways you can control flow in R.
For conditional statements, the most commonly used approaches are the constructs:


~~~sourcecode
# if
if (condition is true) {
  perform action
}

# if ... else
if (condition is true) {
  perform action
} else {  # that is, if the condition is false,
  perform alternative action
}
~~~

Say, for example, that we want R to print a message if a variable `x` has a particular value:


~~~sourcecode
# sample a random number from a Poisson distribution
# with a mean (lambda) of 8

x <- rpois(1, lambda=8)

if (x >= 10) {
  print("x is greater than or equal to 10")
}

x
~~~



~~~output
[1] 8

~~~

Note you may not get the same output as your neighbour because
you may be sampling different random numbers from the same distribution.

Let's set a seed so that we all generate the same 'pseudo-random'
number, and then print more information:


~~~sourcecode
x <- rpois(1, lambda=8)

if (x >= 10) {
  print("x is greater than or equal to 10")
} else if (x > 5) {
  print("x is greater than 5")
} else {
  print("x is less than 5")
}
~~~



~~~output
[1] "x is greater than 5"

~~~

<!--sec data-title="Tip: Pseudo-random numbers" data-id="tip1" data-show=true data-collapse=true ces-->

In the above case, the function `rpois` generates a random number following a Poisson distribution with a mean (i.e. lambda) of 8. The function `set.seed` guarantees that all machines will generate the exact same 'pseudo-random' number ([more about pseudo-random numbers](http://en.wikibooks.org/wiki/R_Programming/Random_Number_Generation)).
So if we `set.seed(10)`, we see that `x` takes the value 8. You should get the exact same number.

<!--endsec-->

**Important:** when R evaluates the condition inside `if` statements, it is
looking for a logical element, i.e., `TRUE` or `FALSE`. This can cause some
headaches for beginners. For example:


~~~sourcecode
x  <-  4 == 3
if (x) {
  "4 equals 3"
}
~~~

As we can see, the message was not printed because the vector x is `FALSE`


~~~sourcecode
x <- 4 == 3
x
~~~



~~~output
[1] FALSE

~~~

<!--sec data-title="Challenge 1" data-id="ch1" data-show=true data-collapse=false ces-->

Use an `if` statement to print a suitable message reporting whether there are any years of birth from 1812 in the `healthData` dataset. Now do the same for 1910.

<!--endsec-->

Did anyone get a warning message like this?


~~~err
Warning in if (healthData$birthYear == 1812) {: the condition has length >
1 and only the first element will be used

~~~

If your condition evaluates to a vector with more than one logical element,
the function `if` will still run, but will only evaluate the condition in the first
element. Here you need to make sure your condition is of length 1.

<!--sec data-title="Tip: any and all" data-id="tip2" data-show=true data-collapse=true ces-->

The `any` function will return TRUE if at least one TRUE value is found within a vector, otherwise it will return `FALSE`. This can be used in a similar way to the `%in%` operator. The function `all`, as the name suggests, will only return `TRUE` if all values in the vector are `TRUE`.

<!--endsec-->

<br>

---

## Repeating operations

 

If you want to iterate over
a set of values, when the order of iteration is important, and perform the
same operation on each, a `for` loop will do the job.
We saw `for` loops in the shell lessons earlier. This is the most
flexible of looping operations, but therefore also the hardest to use
correctly. Avoid using `for` loops unless the order of iteration is important:
i.e. the calculation at each iteration depends on the results of previous iterations.

The basic structure of a `for` loop is:


~~~sourcecode
for(iterator in set of values){
  do a thing
}
~~~

For example:


~~~sourcecode
for(i in 1:10){
  print(i)
}
~~~



~~~output
[1] 1
[1] 2
[1] 3
[1] 4
[1] 5
[1] 6
[1] 7
[1] 8
[1] 9
[1] 10

~~~

The `1:10` bit creates a vector on the fly; you can iterate
over any other vector as well.

We can use a `for` loop nested within another `for` loop to iterate over two things at
once.


~~~sourcecode
for (i in 1:5){
  for(j in c('a', 'b', 'c', 'd', 'e')){
    print(paste(i,j))
  }
}
~~~



~~~output
[1] "1 a"
[1] "1 b"
[1] "1 c"
[1] "1 d"
[1] "1 e"
[1] "2 a"
[1] "2 b"
[1] "2 c"
[1] "2 d"
[1] "2 e"
[1] "3 a"
[1] "3 b"
[1] "3 c"
[1] "3 d"
[1] "3 e"
[1] "4 a"
[1] "4 b"
[1] "4 c"
[1] "4 d"
[1] "4 e"
[1] "5 a"
[1] "5 b"
[1] "5 c"
[1] "5 d"
[1] "5 e"

~~~

Rather than printing the results, we could write the loop output to a new object.


~~~sourcecode
output_vector <- c()
for (i in 1:5){
  for(j in c('a', 'b', 'c', 'd', 'e')){
    temp_output <- paste(i, j)
    output_vector <- c(output_vector, temp_output)
  }
}
output_vector
~~~



~~~output
 [1] "1 a" "1 b" "1 c" "1 d" "1 e" "2 a" "2 b" "2 c" "2 d" "2 e" "3 a"
[12] "3 b" "3 c" "3 d" "3 e" "4 a" "4 b" "4 c" "4 d" "4 e" "5 a" "5 b"
[23] "5 c" "5 d" "5 e"

~~~

This approach can be useful, but 'growing your results' (building
the result object incrementally) is computationally inefficient, so avoid
it when you are iterating through a lot of values.

<!--sec data-title="Tip: Don't grow your results" data-id="tip3" data-show=true data-collapse=true ces-->

One of the biggest things that trips up novices and experienced R users alike, is building a results object (vector, list, matrix, data frame) as your for loop progresses. Computers are very bad at handling this, so your calculations can very quickly slow to a crawl. It's much better to define an empty results object before hand of the appropriate dimensions. So if you know the end result will be stored in a matrix like above, create an empty matrix with 5 row and 5 columns, then at each iteration store the results in the appropriate location.

<!--endsec-->

A better way is to define your (empty) output object before filling in the values.
For this example, it looks more involved, but is still more efficient.


~~~sourcecode
output_matrix <- matrix(nrow=5, ncol=5)
j_vector <- c('a', 'b', 'c', 'd', 'e')
for (i in 1:5){
  for(j in 1:5){
    temp_j_value <- j_vector[j]
    temp_output <- paste(i, temp_j_value)
    output_matrix[i, j] <- temp_output
  }
}
output_vector2 <- as.vector(output_matrix)
output_vector2
~~~



~~~output
 [1] "1 a" "2 a" "3 a" "4 a" "5 a" "1 b" "2 b" "3 b" "4 b" "5 b" "1 c"
[12] "2 c" "3 c" "4 c" "5 c" "1 d" "2 d" "3 d" "4 d" "5 d" "1 e" "2 e"
[23] "3 e" "4 e" "5 e"

~~~

<!--sec data-title="Tip: While loops" data-id="tip4" data-show=true data-collapse=true ces-->

Sometimes you will find yourself needing to repeat an operation until a certain condition is met. You can do this with a `while` loop.


~~~sourcecode
while(this condition is true){
   do a thing
}
~~~
 
As an example, here's a while loop  that generates random numbers from a uniform distribution (the `runif` function) between 0 and 1 until it gets one that's less than 0.1.

~~~ {.r}
z <- 1
while(z > 0.1){
   z <- runif(1)
   print(z)
}
~~~
 
`while` loops will not always be appropriate. You have to be particularly careful that you don't end up in an infinite loop because your condition is never met.

<!--endsec-->

<!--sec data-title="Challenge 2" data-id="ch2" data-show=true data-collapse=false ces-->

Compare the objects output_vector and output_vector2. Are they the same? If not, why not? How would you change the last block of code to make output_vector2 the same as output_vector?

<!--endsec-->

<!--sec data-title="Challenge 3" data-id="ch3" data-show=true data-collapse=false ces-->

Write a script that loops through the `healthData` data by illness level and prints  out whether the mean health measure is smaller or larger than 8 units. Hint: you may want to check out the functions na.rm(), is.na() and unique()

<!--endsec-->

<!--sec data-title="Challenge 4" data-id="ch4" data-show=true data-collapse=false ces-->

Modify the script from Challenge 4 to also loop over each study. This time print out whether the health measure is smaller than 5, between 5 and 8, or greater than 8.

<!--endsec-->

<br>

---

## Challenge solutions

<!--sec data-title="Solution to Challenge 1" data-id="ch1sol" data-show=true data-collapse=true ces-->

Use an `if` statement to print a suitable message reporting whether there are any years of birth from 1812 in the `healthData` dataset. Now do the same for 1910.


~~~sourcecode
if (any(healthData$birthYear == 1812)){
  print("There was at least one person born in 1812 in the dataset")
} else {
  print("There are no people in the dataset who were born in 1812")
}
~~~



~~~output
[1] "There are no people in the dataset who were born in 1812"

~~~



~~~sourcecode
if (any(healthData$birthYear == 1910)){
  print("There was at least one person born in 1910 in the dataset")
} else {
  print("There are no people in the dataset who were born in 1910")
}
~~~



~~~output
[1] "There was at least one person born in 1910 in the dataset"

~~~

<!--endsec-->

<!--sec data-title="Solution to Challenge 2" data-id="ch2sol" data-show=true data-collapse=true ces-->

Compare the objects output_vector and output_vector2. Are they the same? If not, why not? How would you change the last block of code to make output_vector2 the same as output_vector?

The rows and columns have been swapped between output_vector and output_vector2

~~~sourcecode
output_matrix <- matrix(nrow=5, ncol=5)
j_vector <- c('a', 'b', 'c', 'd', 'e')
for (i in 1:5){
  for(j in 1:5){
    temp_j_value <- j_vector[j]
    temp_output <- paste(i, temp_j_value)
    output_matrix[j, i] <- temp_output
  }
}
output_vector2 <- as.vector(output_matrix)
output_vector2
~~~



~~~output
 [1] "1 a" "1 b" "1 c" "1 d" "1 e" "2 a" "2 b" "2 c" "2 d" "2 e" "3 a"
[12] "3 b" "3 c" "3 d" "3 e" "4 a" "4 b" "4 c" "4 d" "4 e" "5 a" "5 b"
[23] "5 c" "5 d" "5 e"

~~~

<!--endsec-->

<!--sec data-title="Solution to Challenge 3" data-id="ch3sol" data-show=true data-collapse=true ces-->

Write a script that loops through the `healthData` data by illness level and prints out whether the mean health measure is smaller or larger than 8 units.


~~~sourcecode
for (illness in sort(unique(healthData$illnessReversed[!is.na(healthData$illnessReversed)]))){
  if ((mean(healthData$health[healthData$illnessReversed == illness], na.rm=T) > 8)){
  print(paste("The mean health measure for people with", illness, "illness is greater than 8 units"))
  } else {
  print(paste("The mean health measure for people with", illness, "illness is less than 8 units"))
  }
}
~~~



~~~output
[1] "The mean health measure for people with 1 illness is less than 8 units"
[1] "The mean health measure for people with 2 illness is less than 8 units"
[1] "The mean health measure for people with 3 illness is less than 8 units"
[1] "The mean health measure for people with 4 illness is greater than 8 units"
[1] "The mean health measure for people with 5 illness is greater than 8 units"

~~~

<!--endsec-->

<!--sec data-title="Solution to Challenge 4" data-id="ch4sol" data-show=true data-collapse=true ces-->

Modify the script from Challenge 4 to also loop over each study. This time print out whether the health measure is smaller than 5, between 5 and 8, or greater than 8.


~~~sourcecode
for (illness in sort(unique(healthData$illnessReversed[!is.na(healthData$illnessReversed)]))){
  for (group in unique(healthData$HIGroup)){
    if ((ans <- mean(healthData$health[healthData$illnessReversed == illness & healthData$HIGroup == group], na.rm=T)) > 8){
      print(paste("The mean health measure for people with", illness, "illness in group", group, "is greater than 8 units"))
    } else if (ans < 5){
      print(paste("The mean health measure for people with", illness, "illness in group", group, "is less than 5 units"))
    } else {
      print(paste("The mean health measure for people with", illness, "illness in group", group, "is between 5 and 8 units"))
    }
  }
}
~~~



~~~output
[1] "The mean health measure for people with 1 illness in group Group 1 is less than 5 units"
[1] "The mean health measure for people with 1 illness in group Group 2 is less than 5 units"
[1] "The mean health measure for people with 2 illness in group Group 1 is between 5 and 8 units"
[1] "The mean health measure for people with 2 illness in group Group 2 is between 5 and 8 units"
[1] "The mean health measure for people with 3 illness in group Group 1 is between 5 and 8 units"
[1] "The mean health measure for people with 3 illness in group Group 2 is between 5 and 8 units"
[1] "The mean health measure for people with 4 illness in group Group 1 is greater than 8 units"
[1] "The mean health measure for people with 4 illness in group Group 2 is greater than 8 units"
[1] "The mean health measure for people with 5 illness in group Group 1 is greater than 8 units"
[1] "The mean health measure for people with 5 illness in group Group 2 is greater than 8 units"

~~~

<!--endsec-->
