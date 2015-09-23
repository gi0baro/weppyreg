weppy-Haml provides a [Haml](http://haml.info) like syntax for templates. This is not a template engine but a compiler which converts haml files to html weppy templates.

## Haml-like syntax

So, how an haml template will look like? Let's say you have this html template in your application:

```html
{{extend 'layout.html'}}

<div class='row'>
    <div id='posts' class='posts'>
    {{for post in posts:}}
        <div class='col-md-4'>
            {{if post.image:}}
            <img src='{{=post.image}}' class='post-image' />
            {{pass}}
            <h3 class='post-title'>
                {{=post.title}}
            </h3>
            <h5>
                {{=post.description}}
            </h5>
        </div>
    {{pass}}
    </div>
</div>
``` 

Then the same template in haml syntax will be like this:

```haml
- extend 'layout.html'

.row
    #posts.posts
    - for post in posts
        .col-md-4
            - if post.image
                %img.post-image{src: '{{=post.image}}'}
            %h3.post-title
                = post.title
            %h5
                = post.description
``` 

Here is a short list of weppy-Haml pros vs the default html templating system:

* Less code to write
* Your code will be more human readable
* You don't have to write the `pass` instructions, just to correctly indent your code

So, you still gonna use html? :)

## Installation

You can install weppy-Haml using pip:

    pip install weppy-Haml

And add it to your weppy application:

```python
from weppy_haml import Haml

app.use_extension(Haml)
```

## Configuration

weppy-Haml has 2 configuration options:

| parameter | default value | description |
| --- | --- | --- |
| `set_as_default` | `False` | set the default extension for templates to haml |
| `auto_reload` | `False` | check for changes in template files and reload them |

If you only use haml templates, you should set the `set_as_default` parameter to True:

```python
app.config.Haml.set_as_default = True
app.use_extension(Haml)
```

so when you expose a function from your application without explicitly pass the template

```python
@app.expose()
def myfunction():
    # some awesome code
```

weppy will look at the haml file (in this case *myfunction.haml*) insted of the html one.

> **Warning**   
> When you set configuration values for the extension, ensure to do that **before** adding the extension to your application.

### Auto reloading haml templates

The *auto reload* feature of Haml extension works in the same way of weppy caching system for templates – introduced since v0.3 – so it's automatically enabled when your application is in *debug* mode, ignoring the configuration value.

Depending on your needs – and remembering that the auto-reload feature might affect your application performance in production – you can set the `auto_reload` parameter to `True`, but it will work only if you set the `templates_auto_reload` parameter of your application configuration to `True` as well:

```python
app.config.templates_auto_reload = True
app.config.Haml.auto_reload = True
app.use_extension(Haml)
```

## Suggested .gitignore

If you're using git version control system for your application, you'd better add to your *.gitignore* file:

```
*.haml.html
```

so you won't track the generated html files from the haml ones.
