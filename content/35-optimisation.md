---
title: Optimising your analysis
nav: Optimising your analysis
---



### Python
Use the pandas and numpy packages, they are surprisingly fast. If possible store data as a numpy array.

<a href="https://blog.dominodatalab.com/multicore-data-science-r-python/" target="_blank">This tutorial</a> talks about data pipelining in your project. This can be very useful if you have large runs and are in the testing phase, it offers caching so you don't have to start a run from the beginning each time.

Use generator expressions are less memory intensive than : https://www.python.org/dev/peps/pep-0289/

### R
Vectorise your data, R is optimised for vector data.