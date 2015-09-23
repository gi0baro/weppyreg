weppy-BS3 provides [Bootstrap3](http://getbootstrap.com) integration for weppy. The extension will add the style for forms and some frontend utilities like:

* [Font Awesome](https://fortawesome.github.io/Font-Awesome) css icons
* [Moment.js](http://momentjs.com) javascript library

## Installation

You can install weppy-BS3 using pip:

    pip install weppy-BS3

And add it to your weppy application:

```python
from weppy_bs3 import BS3

app.use_extension(BS3)
```

Then you will need to add the `include_bs3` lexer in the *head* part of your template, for example:

```html
<!DOCTYPE html>
<html lang="{{=current.request.language or 'en'}}">
    <head>
        <title>Title</title>
        <meta charset="utf-8" />

        {{include_meta}}
        {{include_helpers}}
        {{include_bs3}}
```

and you're ready to go.

## Configuration

weppy-BS3 has these configuration options:

| parameter | default value | description |
| --- | --- | --- |
| **set\_as\_default_style** | `True` | set the default style for forms to the bootstrap3 one |
| **static_folder** | `'bs3'` | the foldername inside the application's static one to store bootstrap files |
| **date_format** | `"DD/MM/YYYY"` | the format for date fields in forms |
| **time_format** | `"HH:mm:ss"` | the format for time fields in forms |
| **datetime_format** | `"DD/MM/YYYY HH:mm:ss"` | the format for datetime fields in forms |
| **time_pickseconds** | `True` | to pick seconds in time and datetime fields |
| **icon_time** | `'fa fa-clock-o'` | css class for the icon to use for time fields |
| **icon_date** | `'fa fa-calendar'` | css class for the icon to use for date fields |
| **icon_up** | `'fa fa-arrow-up'` | css class for the icon for the up arrow in the date/time picker |
| **icon_down** | `'fa fa-arrow-down'` | css class for the icon for the down arrow in the date/time picker |

You can set them in the common way for weppy extensions:

```python
app.config.BS3.set_as_default_style = False
app.use_extension(BS3)
```

> **Warning**   
> When you set configuration values for the extension, ensure to do that **before** adding the extension to your application.

## Suggested .gitignore

If you're using git version control system for your application, you'd better add the *bs3* folder to your *.gitignore* file:

```
static/bs3/*
```

and if you're using a [package pattern](http://weppy.org/docs/latest/patterns):

```
yourpackagename/static/bs3/*
```
