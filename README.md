R Markdown
----------

This is an R Markdown document. Markdown is a simple formatting syntax for authoring HTML, PDF, and MS Word documents. For more details on using R Markdown see <http://rmarkdown.rstudio.com>.

When you click the **Knit** button a document will be generated that includes both content as well as the output of any embedded R code chunks within the document. You can embed an R code chunk like this:

``` r
summary(cars)
```

    ##      speed           dist       
    ##  Min.   : 4.0   Min.   :  2.00  
    ##  1st Qu.:12.0   1st Qu.: 26.00  
    ##  Median :15.0   Median : 36.00  
    ##  Mean   :15.4   Mean   : 42.98  
    ##  3rd Qu.:19.0   3rd Qu.: 56.00  
    ##  Max.   :25.0   Max.   :120.00

This R Markdown document is set up to output a Markdown file -- it will be named README.md -- and this file will be displayed on the github repository. We will use it for the chapter exercises in this class as an experiment, to see if learning github and Markdown along with other classes is a worthwile approach to the entire bioinformatics Masters' program. So please give it a chance and give plenty of feedback about the experience.

For the exercises, just update this file and when you are done with the exercises, commit them to git and push the changes to github.

Pushing finished exercises to github
------------------------------------

You can commit and push your changes to github from RStudio. For the exercises in this repository you only need to change this one file, so you shouldn't have to worry about how to add new files -- we can deal with that later. You can see the changes you have made to the repository if you go to the **Git** tab in the upper right corner of RStudio.

![Git window](figures/git-1.png)

Newly added files are marked with `?` -- but you shouldn't see any -- and modified files are marked with `m`. To commit changes you need to *Stage* them. This just means that the staged files are the changes that will be committed next time you commit changes -- git prefers to make commits a two-stage process where you first pick what to commit and then commit it, the first step is staging the files. You can stage the files you want to commit by clicking on the check-boxes in the **Staged** column.

![Stage files](figures/git-2.png)

Now, to commit the files, just press the **Commit** button. This opens a window where you can see the changes you are about to commit and you can write a log message for the commit.

![Commiting](figures/git-3.png)

After you have committed, the **Git** window will no longer show the changes you commited. If you staged all the modified files, you will have an empty list of changes.

![After committing](figures/git-4.png)

There will be a message just below the toolbar, though, telling you that you are ahead of the `origin/master` branch. When you commit changes you are only modifying your local copy of the repository. You can work on the local copy with all the features provided by git and you can commit as often as you want, but the changes are not pushed upstream to GitHub until you press the **Push** button. This means that you can work on your code in private without having to interact with GitHub until you are ready to push, and then press **Push**. To hand in the exercises for this week, you will eventually have to **Push**, though.

Please, though, **before you push for the final commit, make sure that you knit the README.Rmd file**. This ensures that the most recent README.md file -- which is the one displayed on the GitHub repository page -- is reflecting the most recent README.Rmd file.

Exercises for chapther "More R programming"
-------------------------------------------

### Fibonacci numbers

The *Fibonacci* numbers are defined as follows: The first two Fibonacci numbers are 1, \(F_1=F_2=1\). For larger Fibonacci numbers they are defined as \(F_i=F_{i-1}+F_{i-2}\).

Implement a recursive function that computes the \(n\)'th Fibonacci number.

``` r
fibonacci = function(n){
  if (n==1 | n==2){
    fib=1
  } else {
    fib = fibonacci(n-1)+fibonacci(n-2)
  }
  return(fib)
}

#Testing:
fibonacci(5) #5
```

    ## [1] 5

``` r
fibonacci(9) #34
```

    ## [1] 34

The recursive function for Fibonacci numbers is usually quite inefficient because you are recomputing the same numbers several times in the recursive calls. So implement another version that computes the \(n\)'th Fibonacci number iteratively (that is, start from the bottom and compute the numbers up to \(n\) without calling recursively).

``` r
fibonacciIter = function(n){
  fib = c(rep(0,n))
  fib[1]=1
  if (n>1){
    fib[2]=1
    if (n>2){
      for (i in 3:n){
        fib[i] = fib[i-1]+fib[i-2]
      }
    }
  }
  return(fib[n])
}

#Testing:
fibonacciIter(2) # 1
```

    ## [1] 1

``` r
fibonacciIter(3) # 2
```

    ## [1] 2

``` r
fibonacciIter(12) # 144
```

    ## [1] 144

### Outer product

The outer product of two vectors, \(\mathbf{v}\) and \(\mathbf{w}\), is a matrix defined as

      v_1 * w_1, v_1 * w_2, v_1 * w_3, v_1 * w_4
      v_2 * w_1, v_2 * w_2, v_2 * w_3, v_2 * w_4
      v_3 * w_1, v_3 * w_2, v_3 * w_3, v_3 * w_4

Write a function that computes the outer product of two vectors.

There actually is a built-in function, `outer`, that you are overwriting here. You can get to it using the name `base::outer` even after you have overwritten it. You can use it to check that your own function is doing the right thing.

#### Solution

``` r
outerProduct = function(v, w){
  u = matrix(nrow=length(v), ncol=length(w))
  for (i in seq_along(v)){
    for (j in seq_along(w)){
      u[i,j]=v[i]*w[j]
    }
  }
  return(u)
}

#Testing:
outerProduct(c(1,2,3), c(2,3,4,5))
```

    ##      [,1] [,2] [,3] [,4]
    ## [1,]    2    3    4    5
    ## [2,]    4    6    8   10
    ## [3,]    6    9   12   15

``` r
outer(c(1,2,3), c(2,3,4,5))
```

    ##      [,1] [,2] [,3] [,4]
    ## [1,]    2    3    4    5
    ## [2,]    4    6    8   10
    ## [3,]    6    9   12   15

``` r
#They give the same result
```

### Linear time merge

The merge function we used above copies vectors in its recursive calls. This makes it slower than it has to be. Implement a linear time merge function.

Before you start, though, you should be aware of something. If you plan to append to a vector by writing something like

``` r
v <- c(v, element)
```

then you will end up with a quadratic time algorithm again. This is because when you do this, you are actually creating a new vector where you first copy all the elements in the old `v` vector into the first elements and then add the `element` at the end. If you do this \(n\) times, you have spent on average order \(n^2\) per operation. It is because people do something like this in loops, more than the R interpreter, that has given R its reputation for slow loops. You should never append to vectors unless there is no way to avoid it.

In the case of the merge function, we already know how long the result should be, so you can pre-allocate a result vector and copy single elements into it. You can create a vector of length \(n\) like this:

``` r
n <- 5
v <- vector(length = n)
```

Should you ever need it, you can make a list of length \(n\) like this

``` r
vector("list", length = n)
```

#### Solution

``` r
#My linear time merge function:
myMerge = function(x,y){
  n=length(x)+length(y)
  result = vector(length=n) # vector to store the results
  
  i=1 #Index in x
  j=1 #Index in y
  
  for (k in seq_along(result)){
    if(i > length(x)){
      #We are done with x
      result[-(1:k-1)] = y[j:length(y)]
      return(result)
    }
    if(j > length(y)){
      #We are done with y
      result[-(1:k-1)] = x[i:length(x)]
      return(result)
    }
    if (x[i] < y[j]){
      result[k] = x[i]
      i = i+1
    } else {
      result[k] = y[j]
      j = j+1
    }
  }
  return(result)
}

#Original merge function:
merge <- function(x, y) {
  if (length(x) == 0) return(y)
  if (length(y) == 0) return(x)
  if (x[1] < y[1]) {
    c(x[1], merge(x[-1], y))
  } else {
    c(y[1], merge(x, y[-1]))
  }
}

#I split the mergeSort in two for use when evaluating time consumption:
myMerge_sort <- function(x) {
  if (length(x) < 2){
    return(x)
  }
  n <- length(x)
  m <- n %/% 2

  myMerge(myMerge_sort(x[1:m]), myMerge_sort(x[(m+1):n]))
}

merge_sort <- function(x) {
  if (length(x) < 2){
    return(x)
  }
  n <- length(x)
  m <- n %/% 2

  merge(merge_sort(x[1:m]), merge_sort(x[(m+1):n]))
}

#Test of time:
timing = matrix(ncol=2, nrow=50, dimnames = list(seq(1,2500, 50), c("originalMerge", "myMerge"))) # matrix to store results

for (i in rownames(timing)){
  #Generate random sequence of numbers 1-100 with length i to sort:
  seqToSort = sample(1:100, i, replace=T)
  
  # Get timing for original merge function and insert into matrix:
  time1 = proc.time()
  merge_sort(seqToSort)
  timing[i,"originalMerge"]= (proc.time()-time1)[3]
  
  # Get timing for myMerge function and insert into matrix:
  time2 = proc.time()
  myMerge_sort(seqToSort)
  timing[i,"myMerge"]= (proc.time()-time2)[3]
}

# Do linear regression to check linearity of myMerge performance:
reg=lm(myMerge ~ as.numeric(rownames(timing)), data=as.data.frame(timing))
summary(reg)
```

    ## 
    ## Call:
    ## lm(formula = myMerge ~ as.numeric(rownames(timing)), data = as.data.frame(timing))
    ## 
    ## Residuals:
    ##       Min        1Q    Median        3Q       Max 
    ## -0.019309 -0.006205 -0.001067  0.003920  0.046772 
    ## 
    ## Coefficients:
    ##                                Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)                  -6.186e-03  3.253e-03  -1.902   0.0632 .  
    ## as.numeric(rownames(timing))  6.834e-05  2.287e-06  29.887   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.01167 on 48 degrees of freedom
    ## Multiple R-squared:  0.949,  Adjusted R-squared:  0.9479 
    ## F-statistic: 893.2 on 1 and 48 DF,  p-value: < 2.2e-16

``` r
# Residuals:
#       Min        1Q    Median        3Q       Max 
# -0.018400 -0.005541 -0.001600  0.004859  0.027812 
# 
# Coefficients:
#                                Estimate Std. Error t value Pr(>|t|)    
# (Intercept)                  -1.149e-03  2.574e-03  -0.446    0.657    
# as.numeric(rownames(timing))  6.635e-05  1.809e-06  36.675   <2e-16 ***
# ---
# Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
# 
# Residual standard error: 0.009231 on 48 degrees of freedom
# Multiple R-squared:  0.9655,  Adjusted R-squared:  0.9648 
# F-statistic:  1345 on 1 and 48 DF,  p-value: < 2.2e-16

#Plot the results:
plot(rownames(timing), timing[,"originalMerge"], type="l", col="blue", main="myMerge  (red) vs. merge performance (blue)", xlab="Length of sequence to sort", ylab="Time (s)")
points(rownames(timing), timing[,"myMerge"], col="red")
abline(reg, col="red")
```

![](README_files/figure-markdown_github/merge-1.png)

``` r
#With an R-squared of 0.9648, myMerge is in linear time. The old merge function is roughly linear when the length of the sequence to sort is < 1500, but after that the time usage accelerates even more with increasing sequence length. I tried to sort even longer sequences to see how prounounced this difference becomes but the old merge function fails with ' Error: protect(): protection stack overflow' even after I set options(expressions = 100000). myMerge is still linear up to length = 10000
```

### Binary search

Binary search is a classical algorithm for finding out if an element is contained in a sorted sequence. It is a simple recursive function. The basic case handles a sequence of one element. There you can directly compare the element you are searching for with the element in the sequence to determine if they are the same. If you have more than one element in the sequence, pick the middle one. If it is the element you are searching for, you are done and can return that the element is contained in the sequence. If it is smaller than the element you are searching for then, you know that *if* the element is in the list then it has to be in the last half of the sequence, and you can search there. If it is larger than the element you are searching for, then you know that if it is in the sequence, it must be in the first half of the sequence, and you search recursively there.

If you implement this exactly as described, you have to call recursively with a subsequence. This involves copying that subsequence for the function call which makes the implementation *much* less efficient than it needs to be. Try to implement binary search without this.

#### Solution

``` r
binary = function(x, sequence){
  seq = myMerge_sort(sequence)
  #Base case:
  if (length(seq)==1 & x==seq[1]){
    return(paste("The element", x, "is contained in the sequence"))
  }
  
}
```

### More sorting

In the merge sort we implemented above, we solve the sorting problem by splitting a sequence in two, sorting each subsequence, and then merging them. If implemented correctly this algorithm will run in time \(O(n \log n)\) which is optimal for sorting algorithms if we assume that the only operations we can do on the elements we sort are comparing them.

#### Bucket sort

If the elements we have are all integers between \(1\) and \(n\) and we have \(m\) of them, we can sort them in time \(O(n+m)\) using bucket sort instead. This algorithm first creates a vector of counts for each number between 1 and \(n\). This takes time \(O(n)\). It then runs through the \(m\) elements in our sequence, updating the counter for number \(i\) each time it sees \(i\). This runs in time \(O(m)\). Finally, it runs through these numbers from 1 up to \(n\) and outputting each number, the number of times indicated by the counters, in time \(O(n+m)\).

Implement bucket sort.

#### Solution

``` r
# WRITE YOUR IMPLEMENTATION AND TEST HERE.
```

#### Quick sort

Another algorithm that works by recursion, and that runs in expected time \(O(n \log n)\), is quick sort. It's worst case complexity is actual \(O(n^2)\) but on average it runs in time \(O(n \log n)\) and with a smaller overhead than merge sort (if you implement it correctly).

It works as follows: the basis case -- a single element -- is the same as merge sort. When you have more than one element you pick one of the elements in the sequence at random; call it the pivot. Now split the sequence into those elements that are smaller than the pivot, those that are equal to the pivot, and those that are larger. Sort the sequences of smaller and larger elements recursively. Then output all the sorted smaller elements, then the elements equal to the pivot, and then the sorted larger elements.

Implement quick sort.

#### Solution

``` r
# WRITE YOUR IMPLEMENTATION AND TEST HERE.
```

### Selecting the k smallest element

If you have \(n\) elements, and you want the \(k\) smallest, an easy solution is to sort the elements and then pick number \(k\). This works well and in most cases is easily fast enough, but it is actually possible to do it faster. See, we don't actually need to sort the elements completely, we just need to have the \(k\) smallest element moved to position \(k\) in the sequence.

The quick sort algorithm from the previous exercise can be modified to solve this problem. Whenever we split a sequence into those smaller than, equal to, and larger than the pivot, we sort the smaller and larger elements recursively. If we are only interested in finding the element that would eventually end up at position \(k\) in the sorted lists we don't need to sort the sequence that doesn't overlap this index. If we have \(m < k\) elements smaller than the pivot, we can just put them at the front of the sequence without sorting them. We need them there to make sure that the \(k\)'th smallest element ends up at the right index, but we don't need them sorted. Similar, if \(k < m\) we don't need to sort the larger elements. If we sorted them, they would all end up at indices larger than \(k\) and we don't really care about those. Of course, if there are \(m < k\) elements smaller than the pivot and \(l\) equal to the pivot, with \(m+l \geq k\), then the \(k\) smallest element is equal to the pivot, and we can return that.

Implement this algorithm.

#### Solution

``` r
# WRITE YOUR IMPLEMENTATION AND TEST HERE.
```
