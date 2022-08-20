---
layout: post
title: "如何从0-1数据得到列联表：R中table和xtabs函数的使用"
description: ""
category: 
tags: [R]
---
{% include JB/setup %}

>* This will become a table of contents (this text will be scraped).
>{:toc}

在处理0-1数据(binary data)中，我们经常需要将其转换为列联表(cross tabulation或称contingency table)。本次主角是`table`和`xtabs`函数。

## 生成测试数据

为了举例说明，我们首先生成0-1数据。`set.seed(1)`是为了设置种子，每次得到完全一样的“随机”数据。后面采用设置`dim`函数的方法建立matrix，可以避免使用`matrix`函数带来的拷贝一次mydata的额外操作。

~~~ R
> mydata <- {set.seed(1); # set a constant seed so that
>                         # we can get reproducible results
>            sample.int(2, size = 200, replace = T) - 1L} # generate binary ints
> dim(mydata) <- c(100,2) # convert a vector to a matrix
~~~

执行`head`函数，得到结果的是这样的。

~~~ R
> head(mydata)
     [,1] [,2]
[1,]    0    1
[2,]    0    0
[3,]    1    0
[4,]    1    1
[5,]    0    1
[6,]    1    0
~~~

## matrix类型

分别对mydata使用`table`函数和`xtabs`函数，试图建立连列表，结果如下。

~~~ R
> table(mydata)
mydata
  0   1 
 98 102 
> xtabs(~ V1 + V2, data = mydata)
   V2
V1   0  1
  0 23 29
  1 23 25
~~~

可见，使用`table`不能够对matrix类型建立连列表，而只是简单的对整个matrix中的0和1进行计数；`table`能够对data.frame类型建立连列表。

## data.frame类型

首先利用前面的matrix类型的mydata建立data.frame类型的mydata.df。

~~~ R
mydata.df <- as.data.frame(mydata)
~~~

执行`head`函数，得到结果的是这样的。

~~~ R
> head(mydata.df)
  V1 V2
1  0  1
2  0  0
3  1  0
4  1  1
5  0  1
6  1  0
~~~

这里和前面一样调用`table`和`xtabs`建立连列表。两个函数都能够得到我们想要的结果。

~~~ R
> table(mydata.df)
   V2
V1   0  1
  0 23 29
  1 23 25
> xtabs(~ V1 + V2, data = mydata.df)
   V2
V1   0  1
  0 23 29
  1 23 25
~~~

总结如下：如果想要得到连列表的话，除了`table`和matrix类型的数据这种组合不行，其他三种组合都是可以的。

## 其他

1. 在已经建立好的连列表的基础上，使用`ftable`函数，可以在多变量的情况下，绘制出我们主要关注的变量处于列的位置，而其他变量处于行的位置的连列表。

    例如，对于结构如下的Titanic数据,

        > str(Titanic)
         table [1:4, 1:2, 1:2, 1:2] 0 0 35 0 0 0 17 0 118 154 ...
         - attr(*, "dimnames")=List of 4
          ..$ Class   : chr [1:4] "1st" "2nd" "3rd" "Crew"
          ..$ Sex     : chr [1:2] "Male" "Female"
          ..$ Age     : chr [1:2] "Child" "Adult"
          ..$ Survived: chr [1:2] "No" "Yes"

    我们举一个help文档中的范例：

        > ftable(Titanic, row.vars = 2:1, col.vars = "Survived")
                     Survived  No Yes
            Sex    Class                 
            Male   1st            118  62
                   2nd            154  25
                   3rd            422  88
                   Crew           670 192
            Female 1st              4 141
                   2nd             13  93
                   3rd            106  90
                   Crew             3  20
               
2. `as.data.frame`可以看作是`xtabs`的逆函数，得到data.frame类型的表格。不过，最原始的0-1数据中，每一行的具体顺序的信息仍然丢失了，取而代之的，是新的记录频数的一列，称为Freq：

        > t <- xtabs(~ V1 + V2, data = mydata)
        > df <- as.data.frame(t)
        > df
          V1 V2 Freq
        1  0  0   23
        2  1  0   23
        3  0  1   29
        4  1  1   25
        > xtabs(Freq~V1+V2, data = t)
           V2
        V1   0  1
          0 23 29
          1 23 25

3. 制作连列表的时候需要注意，使用的两列数据的对应的行应当具有相同的行名，否则就会得到没有意义的结果。如果两列数据对应的行顺序不同，需要使用`order`函数对整个表格先进行排序。
