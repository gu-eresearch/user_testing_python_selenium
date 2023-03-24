---
title: common
nav: common
topics: common
---


## Basic Configuration

scroll
```python
context.browser.execute_script("window.scrollTo(0, 0)")
```


split based on new lines


```python
_selector = WebDriverWait(context.browser, timeout).until(
    EC.visibility_of_element_located((By.CSS_SELECTOR, css_selector))
    )

assert(_selector.text.split('\n')[0] == value)
```

upload file from absolute path
```python
# behave command
# When I attach the file "../../ca-test.xlsx" to the form element with id "importcompoundsaustraliaform-importfile"

@when(u'I attach the file "{file}" to the form element with id "{id}"')
def step_impl(context, file, id):
	file_upload = WebDriverWait(context.browser, 10).until(
	    EC.presence_of_element_located((By.ID, id))
	)
	file_upload.send_keys(os.path.abspath(Path(__file__).parent.absolute() / file))
```

```python
@then(u'I swipe left on the button with the class "{css_class}"')
def step_impl(context, css_class):
    xsearch = "//*[@class=" + '"'  + css_class + '"' + "]"
    download_button = context.browser.find_element(By.XPATH, xsearch)
    actions = ActionChains(context.browser)
    actions.drag_and_drop_by_offset(download_button, -80, 0)
    actions.perform()
```

```python
#get value from dropdown
select = Select(context.browser.find_element(By.ID, 'importlabextractsform-importid'))
select.select_by_visible_text('Banana')
select.select_by_value('1679456710')
select.select_by_index(2)
select.select_by_visible_text('full text')
```

```python
browser.switch_to.alert.accept()
```


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



```python
def check_exists(test_item, check_item):
    try:
        assert(test_item== check_item)
    except AssertionError:
        return False
    return True
```

