---
title: General Layout
nav: General Layout
---


### File Structure

The file structure is important when running python/behave tests. Two folders are required, features, and steps. Features contains the .feature files, the python environment file and the steps folder. The steps folder contains the .py files.

features/
|--- steps/
|    |--- common.py
|    |--- import.py
|    |--- login.py
|    |--- search.py
import.feature
login.feature
search.feature
environment.py

It is not necessary to name the steps and feature file the same, I find that it keeps the tests organised and themed.


### feature files
The feature file are written in Gherkin syntax and are human readable steps, they usually follow user requirements.


Here is a look at the search.feature file.
```gherkin

Feature: Search for compounds based on different compound properties

    Scenario: Search for compound using a smiles code
        Given I am logged in
        When I search for the compound "Cc1ccc(c(=O)[nH]1)C(=O)N1CCCN(CC1)CC(F)(F)F" in the search bar
        And I do a "similarity" search
        Then 1 or more results are returned

```


### python step files
The python steps file, search.py contains the code that executes the steps in feature files. 


```python
from behave import given, when
from selenium.common.exceptions import NoSuchElementException
import time

@given('I am logged in')
def login_admin(context):
    context.browser.get(context.config.userdata.get("baseurl"))

    # click login button
    context.execute_steps(u'when I click on the button that contains the link_text "{link_text}"'.format(link_text='Sign In'))   
    context.execute_steps(u'when I send the text "{text}" to the id "{id}"'.format(text=context.config.userdata.get("username"), id="username"))
    context.execute_steps(u'when I send the text "{text}" to the id "{id}"'.format(text=context.config.userdata.get("password"), id="password"))
    time.sleep(1)

    context.execute_steps(u'when I click on the "{id}"'.format(id='kc-login'))

@when(u'I search for the compound "{compound}" in the search bar')
def step_impl(context, compound):
    context.execute_steps(u'when I navigate to the search page')
    context.execute_steps(u'when I send the text "{compound}" to the id "{id}"'.format(compound=compound, id="searchInputLabel"))


@when(u'I do a "{search_type}" search')
def step_impl(context, search_type):
    if search_type == "similarity":
        id = "inline-radio-1"
    elif search_type == "substructure":
        id = "inline-radio-2"
    elif search_type == "full text":
        id = "inline-radio-3"
    context.execute_steps(u'when I click on the "{id}"'.format(id=id))

@then(u'1 or more results are returned')
def step_impl(context):
    context.execute_steps(u'then The text "{text}" is NOT visible in the css_selector "{css_selector}"'.format(text="0 Result", css_selector="div.d-flex.justify-content-start.align-items-center.col-md-4"))

```


### Python common step functions
I create a python file that contains all the commonly used step functions, that I call throughout multiple tests. This reduces code duplication and increases readability. 


### Python Environment File
A python environment file sits within the feature folder. This module defines code to run before and after certain events, and can include opening/closing a web browser and taking snapshots if a step fails.

The following has a fixture to run a chrome web browser in headless an normal states. Headless is required to run in CICD pipelines such as Jenkins, while a normal state is good for troubleshooting while running tests locally.

```python
from behave import fixture, use_fixture
from selenium import webdriver
import logging
import os
from selenium.common.exceptions import NoSuchElementException
import re

work_dir = os.getcwd()

@fixture
def add_check_exists_by_xpath_func(context):
    def check_exists_by_xpath(xpath):
        try:
            context.browser.find_element_by_xpath(xpath)
        except NoSuchElementException:
            return False
        return True
    context.check_exists_by_xpath = check_exists_by_xpath

@fixture
def selenium_browser_chrome(context):
    context.browser = webdriver.Chrome()
    yield context.browser
    context.browser.quit()

@fixture
def selenium_browser_chrome_headless(context):
    options = webdriver.ChromeOptions()
    options.add_argument('headless')
    options.add_argument('--no-sandbox')
    options.add_argument('window-size=1200x600')
    prefs = {"download.default_directory": work_dir}
    options.add_experimental_option("prefs", prefs)
    context.browser = webdriver.Chrome(chrome_options=options)
    yield context.browser
    context.browser.quit()

def before_all(context):
    browser = os.getenv("BEHAVE_BROWSER", "chrome_headless")
    fixture_func = globals().get("selenium_browser_{}".format(browser.lower().strip()), None)
    if fixture_func is None:
        raise LookupError("No such browser fixture: selenium_browser_{}".format(browser.lower().strip()))
    logging.info("Using browser '{}'".format(browser))
    use_fixture(fixture_func, context)
    use_fixture(add_check_exists_by_xpath_func, context)


def after_all(context):
    context.browser.quit()

def slugify(text):
    slug = text.replace(" ", "_")
    return re.sub(r'(?u)[^-\w.]', '', slug)

def after_step(context, step):
    if step.status == 'failed':
        step_str = step.name
        scen_str = context.scenario.name
        filename = slugify("{}-{}".format(scen_str, step_str))
        context.browser.save_screenshot("{}.png".format(filename))
```



