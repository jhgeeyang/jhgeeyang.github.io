---
layout: category
title: Research
category: research
sidebar_sort_order: 1
---

I work primarily in the of computational geophysics
, which tries to explain wave phenomena with the capability of the high performance computing(HPC).
The primary goal of such research is
to improve the software and algorithms available
for the computer-aided design of modern devices,
control systems, and therapeutics.

### Monitoring the critical infrastructure with DAS

An integral equation method utilizes either an
exact or approximate inverse of a partial differential
equation to analytically precondition the problem
before it is discretized on a computer.
Such an approach results in high-order-accurate, robust,
and fast solvers for a range of PDEs. Research in
this area is focused on (1) expanding the number of
PDEs for which integral equation methods apply
(e.g. by developing new integral representations),
(2) developing quadrature rules to numerically
evaluate the necessary singular integrals in
the method, and (3) the fast solution of the
(typically) dense linear systems that result.

My Ph.D. thesis focused on integral-equation methods
for the solution of inhomogeneous elliptic partial
differential equations in complex geometry. It can be
downloaded [here](/assets/publications/pdf/askham2016integral.pdf),
with known errata [here](/thesis-errata).

{% for pub in site.publications %}
{% if pub.research_area == "inteq" and pub.featured == true %}
{% assign foundinteq = 1 %}
{% endif %}
{% endfor %}

{% if foundinteq %}
Featured publications:
<ul>
{% for pub in site.publications %}
{% if pub.research_area == "inteq" and pub.featured == true %}
   <li> <a href="{{ pub.url }}">{% if pub.title-short %}{{ pub.title-short }}{% else %}{{ pub.title }}{% endif %}</a> </li>
{% endif %}   
{% endfor %}
</ul>
{% endif %}

### Reduced order modeling

Direct numerical simulation of complex physical
systems, e.g. the fluid flow around an intricate
geometry, can
require an incredibly large number of degrees of
freedom to represent the solution. Often, however,
we are actually concerned with how the behavior of
such systems changes as a function of a small number
of system parameters.
The reduced order modeling paradigm is based on the
idea that because the space of system parameters
is lower dimensional, it is sometimes possible to
represent all possible solutions of interest in
terms of a relatively small set of basis functions.
This reduced basis can then be used to design
an approximately optimal control system or
to perform short term future-state prediction.

{% for pub in site.publications %}
{% if pub.research_area == "ROM" and pub.featured == true %}
{% assign foundROM = 1 %}
{% endif %}
{% endfor %}

{% if foundROM %}
Featured publications:
<ul>
{% for pub in site.publications %}
{% if pub.research_area == "ROM" and pub.featured == true %}
   <li> <a href="{{ pub.url }}">{% if pub.title-short %}{{ pub.title-short }}{% else %}{{ pub.title }}{% endif %}</a> </li>
{% endif %}   
{% endfor %}
</ul>
{% endif %}


