---
title: Common python behave functions that are highly recycable
nav: Setup common functions for reusability
---

Many user scenarios contain identical operations that only differ in how/what element is identified. For example, clicking on a button using its xpath, id or selector. Therefore to reduce repitition in code its useful to setup some functions that define how the element is accessed.

The following is a python file that contains regularaly used functions. Notice the @when, @then and @given decorators, they are used by python to link the behave tests with pyhton code used for execution.

I create a python file that contains all the commonly used step functions, that I call throughout multiple tests. This reduces code duplication and increases readability. 

Have a look at my commonly used steps [here](https://github.com/gu-eresearch/selenium-python-code/blob/main/uat/steps/common.py)
