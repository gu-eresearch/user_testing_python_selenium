---
title: Writing Behave and Python tests
nav: Writing Behave and Python tests
---

### File Structure

The file structure is important when running python/behave tests. Two directories are required, features, and steps. Features contains the test files written in the Behave syntax and with the .feature extension. The steps folder has the .py files that contain the python code to execute the Behave tests. Finally a python environment file is stored in the root directory, this file contains the python behave hooks to execute behave from a CLI.

{% include figure.html img="file-structure.png" alt="file structure" caption="The file structure required for running selenium user tests" width="75%" %}

It is not necessary to name the steps and feature file the same, however I find that it keeps the tests organised and themed.

### Behave feature files
The Behave feature files are written in [Gherkin syntax](https://cucumber.io/docs/gherkin/) and are human readable steps, they usually follow user requirements.

Consider the [search_for_a_course.feature](https://github.com/gu-eresearch/selenium-python-code/blob/main/uat/feature/search_for_a_course.feature) feature file. This feature contains a set of tests that search for degrees on the Griffith University website and validate the returned search query. Notice the given-when-then keywords that are used as acceptance criteria. Following this format makes it easier for the entire team to stay on the same page with expectations for how the software should work.

The steps within these feature file are executed using python code, which is found within the steps file.

### python step files
The python steps file, [search_for_a_course.py](https://github.com/gu-eresearch/selenium-python-code/blob/main/uat/steps/search_for_a_course.py) contains the code that executes the steps in feature file. There are a suite of [selenium specific libraries](https://selenium-python.readthedocs.io/) for python.
Notice the given, when and then hooks, they indicate what code python runs for each step in a feature file.

### Python Environment File
A python environment file sits within the feature folder. This module defines code to run before and after certain events, and can include opening/closing a web browser and taking snapshots if a step fails.

You will need to setup [drivers](https://selenium-python.readthedocs.io/installation.html#drivers) 'the web browser', either locally or within your CICD environment. The following environment file has a fixture to run a chrome web browser in headless 'without a graphical user interface' and normal states. Headless is required to run in CICD pipelines such as Jenkins, while a normal state is good for troubleshooting while running tests locally.

[Here](https://github.com/gu-eresearch/selenium-python-code/blob/main/uat/environment.py) is an example of a python environment file.
