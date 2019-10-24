---
layout  : wiki
title   : Using Eigen
summary : 
date    : 2019-10-21 13:32:07 +0900
updated : 2019-10-21 15:43:36 +0900
tag     : Eigen
toc     : true
public  : true
parent  : Useful Libraries
latex   : false
---
* TOC
{:toc}


## How to Start
1. Just Clone the Eigen repository
- [Eigen Repo](https://github.com/eigenteam/eigen-git-mirror)
    2. Build the code including Eigen header

    [Eigen Start Guide](https://eigen.tuxfamily.org/dox/GettingStarted.html)

    {% highlight cpp %}
#include <iostream>
#include <Eigen/Dense>
using Eigen::MatrixXd;
int main()
{
MatrixXd m(2,2);
m(0,0) = 3;
m(1,0) = 2.5;
m(0,1) = -1;
m(1,1) = m(1,0) + m(0,1);
std::cout << m << std::endl;
}
{% endhighlight %}

