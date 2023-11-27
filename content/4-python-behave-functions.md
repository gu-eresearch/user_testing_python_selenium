---
title: common 
nav: common
topics: common
---


## Python functions

If there is a value in a field that needs to be deleted so that a new value can be added

First simple way
```python
_element.clear()
```
second way
```python
@then('I click on the field with the name "{name}" and delete "{number}" values')
def step_impl(context, name, number):
    xsearch = "//*[@name = " + '"'  + name + '"' + "]"
    _field = WebDriverWait(context.browser, timeout).until(
        EC.visibility_of_element_located((By.XPATH, xsearch))
        )

    for i in range(1,int(number)):
        _field.send_keys(Keys.BACKSPACE)
```

context.browser.find_element(By.XPATH, "//*[starts-with(@id, 'compound-card-smiles-code-')]")





