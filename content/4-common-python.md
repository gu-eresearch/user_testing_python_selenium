---
title: Common python behave functions that are highly recycable
nav: Common
---

Many user testing operations are identical in the operation, and only differ in how the element is identified. For example, waiting for a button to be clickable based on when the xpath, id or selector becomes visible. Therefore to reduce repitition in code its useful to setup some functions that define how the element is accessed.

```python
from behave import when, then, given
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.support.ui import Select
from selenium.common.exceptions import TimeoutException, ElementClickInterceptedException
import time
import os

timeout=30

########################################################################################################################
########################################################################################################################

def selector_visible(context, _element, _type):
    if _type == "selector":
        selected_element = WebDriverWait(context.browser, timeout).until(
            EC.visibility_of_element_located((By.CSS_SELECTOR, _element))
            )
    
    elif _type == "id":
        selected_element = WebDriverWait(context.browser, timeout).until(
            EC.visibility_of_element_located((By.ID, _element))
            )

    elif _type == "xpath":
        selected_element = WebDriverWait(context.browser, timeout).until(
            EC.visibility_of_element_located((By.XPATH, _element))
            )
    
    elif _type == "text":
        xsearch = "//*[text()=" + '"' + _element + '"' + "]"
        selected_element = WebDriverWait(context.browser, timeout).until(
            EC.visibility_of_element_located((By.XPATH, xsearch))
            )

    elif _type == "name":
        selected_element = WebDriverWait(context.browser, timeout).until(
            EC.visibility_of_element_located((By.NAME, _element))
            )
        
    return selected_element

##################################
##################################

def selector_invisible(context, _element, _type, timeout=timeout):
    if _type == "selector":
        selected_element = WebDriverWait(context.browser, timeout).until(
            EC.invisibility_of_element_located((By.CSS_SELECTOR, _element))
            )
    
    elif _type == "id":
        selected_element = WebDriverWait(context.browser, timeout).until(
            EC.invisibility_of_element_located((By.ID, _element))
            )

    elif _type == "xpath":
        selected_element = WebDriverWait(context.browser, timeout).until(
            EC.invisibility_of_element_located((By.XPATH, _element))
            )
    
    elif _type == "text":
        xsearch = "//*[text()=" + '"' + _element + '"' + "]"
        selected_element = WebDriverWait(context.browser, timeout).until(
            EC.invisibility_of_element_located((By.XPATH, xsearch))
            )

    elif _type == "name":
        selected_element = WebDriverWait(context.browser, timeout).until(
            EC.invisibility_of_element_located((By.NAME, _element))
            )
        
    return selected_element

##################################
##################################

def selector_clickable(context, _element, _type):
    if _type == "selector":
        selected_element = WebDriverWait(context.browser, timeout).until(
            EC.element_to_be_clickable((By.CSS_SELECTOR, _element))
            )
    
    elif _type == "id":
        selected_element = WebDriverWait(context.browser, timeout).until(
            EC.element_to_be_clickable((By.ID, _element))
            )

    elif _type == "xpath":
        selected_element = WebDriverWait(context.browser, timeout).until(
            EC.element_to_be_clickable((By.XPATH, _element))
            )
    
    elif _type == "text":
        xsearch = "//*[text()=" + '"' + _element + '"' + "]"
        selected_element = WebDriverWait(context.browser, timeout).until(
            EC.element_to_be_clickable((By.XPATH, xsearch))
            )

    elif _type == "name":
        selected_element = WebDriverWait(context.browser, timeout).until(
            EC.element_to_be_clickable((By.NAME, _element))
            )
        
    return selected_element
#####################################
#####################################
@when(u'I send the text "{_text}" to the element/type "{_element}"/"{_type}"')
@then(u'I send the text "{_text}" to the element/type "{_element}"/"{_type}"')
def step_impl(context, _text, _element, _type):
    context.execute_steps(u'when I wait for the element/type "{element}"/"{_type}" to load'.format(element=_element, _type=_type))
    _selected_element = selector_visible(context, _element, _type)

    context.browser.execute_script("arguments[0].scrollIntoView();", _selected_element)
    
    _selected_element.send_keys(_text)

#####################################
#####################################
@when(u'I click on the button that contains the element/type "{_element}"/"{_type}"')
@then(u'I click on the button that contains the element/type "{_element}"/"{_type}"')
def step_impl(context, _element, _type):
    context.execute_steps(u'when I wait for the element/type "{element}"/"{_type}" to be clickable'.format(element=_element, _type=_type))
    _selected_element = selector_clickable(context, _element, _type)

    context.browser.execute_script("arguments[0].scrollIntoView();", _selected_element)
    _selected_element.click()

#####################################
#####################################
@when(u'The element/type "{_element}"/"{_type}" is visible')
@then(u'The element/type "{_element}"/"{_type}" is visible')
def step_impl(context, _element, _type):
    selector_visible(context, _element, _type)

#####################################
#####################################
@then(u'The element/type "{_element}"/"{_type}" is invisible')
def step_impl(context, _element, _type):
    selector_invisible(context, _element, _type, timeout=45)

########################################################################################################################
########################################################################################################################
@when(u'I send the text "{text}" to the id "{id}" and click enter')
def step_impl(context, text, id):
    _field = WebDriverWait(context.browser, timeout).until(
        EC.visibility_of_element_located((By.ID, id))
        )

    context.browser.execute_script("arguments[0].scrollIntoView();", _field)
    
    _field.send_keys(text)
    _field.click()

########################################################################################################################
########################################################################################################################
@when(u'I select the value "{value}" from the dropdown with the id "{id}"')
def step_impl(context, value, id):
    select = Select(context.browser.find_element(By.ID, id))
    select.select_by_value(value)

########################################################################################################################
########################################################################################################################
@when(u'I select the value "{value}" from the dropdown with the name "{name}"')
def step_impl(context, value, name):
    xsearch = "//*[@name=" + '"' + name + '"' + "]"
    select = Select(context.browser.find_element(By.XPATH, xsearch))
    select.select_by_value(value)

########################################################################################################################
########################################################################################################################
@when(u'I clear the text from the name "{name}"')
def step_impl(context, name):
    xsearch = "//*[@name=" + '"' + name + '"' + "]"
    _field = WebDriverWait(context.browser, timeout).until(
        EC.visibility_of_element_located((By.XPATH, xsearch))
        )

    context.browser.execute_script("arguments[0].scrollIntoView();", _field)
    
    _field.clear()


########################################################################################################################
########################################################################################################################

def check_element_located(context, _type, element):
    # _type are: selector, id, text (uses xpath search), xpath
    if _type == "selector":
        try:
            WebDriverWait(context.browser, 0.1).until(
                EC.visibility_of_element_located((By.CSS_SELECTOR, element))
                )
        except TimeoutException:
            return False
        return True
    
    if _type == "id":
        try:
            WebDriverWait(context.browser, 0.1).until(
                EC.visibility_of_element_located((By.ID, element))
                )
        except TimeoutException:
            return False
        return True
    
    if _type == "xpath":
        try:
            WebDriverWait(context.browser, 0.1).until(
                EC.visibility_of_element_located((By.XPATH, element))
                )
        except TimeoutException:
            return False
        return True
    
    if _type == "text":
        xsearch = "//*[text()=" + '"' + element + '"' + "]"
        try:
            WebDriverWait(context.browser, 0.1).until(
                EC.visibility_of_element_located((By.XPATH, xsearch))
                )
        except TimeoutException:
            return False
        return True

    if _type == "name":
        try:
            WebDriverWait(context.browser, 0.1).until(
                EC.visibility_of_element_located((By.NAME, element))
                )
        except TimeoutException:
            return False
        return True
    
#####################################
#####################################       
@when(u'I wait for the element/type "{element}"/"{_type}" to load')
@then(u'I wait for the element/type "{element}"/"{_type}" to load')
# _type are: selector, id, text (uses xpath search), xpath
# element is the name of the selector, id ect
def step_impl(context, _type, element, timeout=timeout):
    seconds = 0.0
    _wait = True
    while _wait and seconds < timeout:
        if check_element_located(context, _type, element):
            _wait = False
        seconds += 0.1
        time.sleep(0.1)
    # if it timesout, assert if the statment is true
    time.sleep(0.2)
    assert(_type)

########################################################################################################################
########################################################################################################################

def check_clickable_by_element(context, _type, element):
    # _type are: selector, id, text (uses xpath search), xpath
    if _type == "selector":
        try:
            WebDriverWait(context.browser, 0.1).until(
                EC.element_to_be_clickable((By.CSS_SELECTOR, element))
                )
        except TimeoutException:
            return False
        return True
    
    if _type == "id":
        try:
            WebDriverWait(context.browser, 0.1).until(
                EC.element_to_be_clickable((By.ID, element))
                )
        except TimeoutException:
            return False
        return True
    
    if _type == "xpath":
        try:
            WebDriverWait(context.browser, 0.1).until(
                EC.element_to_be_clickable((By.XPATH, element))
                )
        except TimeoutException:
            return False
        return True
    
    if _type == "text":
        xsearch = "//*[text()=" + '"' + element + '"' + "]"
        try:
            WebDriverWait(context.browser, 0.1).until(
                EC.element_to_be_clickable((By.XPATH, xsearch))
                )
        except TimeoutException:
            return False
        return True
    
#####################################
#####################################
@when(u'I wait for the element/type "{element}"/"{_type}" to be clickable')
# _type are: selector, id, text (uses xpath search), xpath
# element is the name of the selector, id ect
def step_impl(context, _type, element, timeout=timeout):
    seconds = 0.0
    _wait = True
    while _wait and seconds < timeout:
        if check_clickable_by_element(context, _type, element):
            _wait = False
        seconds += 0.1
        time.sleep(0.1)
    # if it timesout, assert if the statment is true
    time.sleep(0.2)
    assert(_type)

########################################################################################################################
########################################################################################################################

def rows_equal_x(context, id, number_of_rows):
    try:
        _table = WebDriverWait(context.browser, timeout).until(
            EC.visibility_of_element_located((By.ID, id))
            )
        assert(len(_table.find_elements(By.TAG_NAME, "tr")) == int(number_of_rows))
    except AssertionError:
        return False
    return True

#####################################
#####################################
@when(u'I wait for the table "{table_id}" to contain exactly "{number_of_rows}" rows')
def step_impl(context, table_id, number_of_rows, timeout=timeout):
    seconds = 0.0
    _wait = True
    while _wait and seconds < timeout:
        if rows_equal_x(context, table_id, number_of_rows):
            _wait = False
        seconds += 0.1
        time.sleep(0.1)
    # if it timesout, assert if the statment is true
    time.sleep(0.2)
    _table = WebDriverWait(context.browser, timeout).until(
        EC.visibility_of_element_located((By.ID, table_id))
        )
    assert(len(_table.find_elements(By.TAG_NAME, "tr")) == int(number_of_rows))
    
########################################################################################################################
########################################################################################################################
@then(u'I wait "{t}" seconds')
@when(u'I wait "{t}" seconds')
def step_impl(context, t):
    time.sleep(int(t))
```

