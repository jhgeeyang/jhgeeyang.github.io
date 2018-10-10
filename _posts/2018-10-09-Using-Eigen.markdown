---
layout: post
title:  "Using Eigen"
date:   2018-10-09 21:55:42 -0600
categories: computation 
---

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

