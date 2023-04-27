---
title: Using Javascript within Python
nav: Javascript
---

# javascript function that waits for element to fully scroll into view - cannot be down with python

```python
def wait_element_scroll_complete(context, element):
    def wait_until_specific_condition(func, timeout=30):
        endtime = time.time() + timeout
        while True:
            if time.time() > endtime:
                raise TimeoutException("The condition is not satisfied")
            result = func()
            if result is True:
                return result
    context.browser.execute_script("arguments[0].scrollIntoView();", element)
    is_visible_script = """function isScrolledIntoView(el) {
                                    var rect = el.getBoundingClientRect();
                                    var elemTop = rect.top;
                                    var elemBottom = rect.bottom;
                                
                                    // Only completely visible elements return true:
                                    var isVisible = (elemTop >= 0) && (elemBottom <= window.innerHeight);
                                    // Partially visible elements return true:
                                    //isVisible = elemTop < window.innerHeight && elemBottom >= 0;
                                    return isVisible;
                                }
                        return isScrolledIntoView(arguments[0]);"""
    wait_until_specific_condition(lambda: context.browser.execute_script(is_visible_script, element))
    ```
