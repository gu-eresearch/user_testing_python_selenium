---
title: Common python behave functions that are highly recycable
nav: Common
---

Many user testing operations are identical in the operation, and only differ in how the element is identified. For example, waiting for a button to be clickable based on when the xpath, id or selector becomes visible. Therefore to reduce repitition in code its useful to setup some functions that define how the element is accessed.

The following is a python file that contains regularaly used functions. Notice the @when, @then and @given decorators, they are used by python to link the behave tests with pyhton code used for execution.

I create a python file that contains all the commonly used step functions, that I call throughout multiple tests. This reduces code duplication and increases readability. 

Have a look at my commonly used steps (here)[https://github.com/gu-eresearch/selenium-python-code/blob/main/uat/steps/common.py]
