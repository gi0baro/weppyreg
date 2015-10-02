weppy-Assets provides assets pipeline management for weppy.

## Installation

You can install weppy-Assets using pip:

    pip install weppy-Assets

And add it to your weppy application:

```python
from weppy_assets import Assets

app.use_extension(Assets)
```

weppy-Assets is designed in order to work *out of the box*. This means that it will install quite a lot of dependencies in order to allow you use:

- coffescript files
- sass and scss files

without compiling them outside of weppy.

There's only one caveat you should take care of regarding coffeescript files, since there's no a pure python implementation to parse them. You will need one of the following external dependencies in order to get them compiled:

- *v8 javascript engine* which is packed inside *node.js*. Just install node on your machine and you will be good.
- *coffee* binary file installed with the coffeescript ruby gem

## Configuration

weppy-Assets has these configuration options:

| parameter | default value | description |
| --- | --- | --- |
| **out_folder** | `'out'` | the name of the folder inside the application's static one to store output files |

You can set them in the common way for weppy extensions:

```python
app.config.Assets.out_folder = 'foobar'
app.use_extension(Assets)
```

> **Warning**   
> When you set configuration values for the extension, ensure to do that **before** adding the extension to your application.


## Assets management

The extension expect you to have an *assets* folder inside the root of your application. Given that, all the files you specify in defining assets will be considered inside that folder.

You can add assets to your application quite easily:

```python
js = app.ext.Assets.js(
    'libs/minified.min.js',
    'base.js',
    'coffeefile.coffee',
    output="all.js")

css = app.ext.Assets.css(
    'css/base.css',
    'css/theme.sass',
    'css/partial.scss',
    output="all.css").minify()

app.ext.Assets.register('base_js', js)
app.ext.Assets.register('base_css', css)
```

As you can see, you should threat separately javascript assets and css assets, but the syntax is always the same. You can create a *bundle* of assets calling the `Assets.js` or `Assets.css` methods, with the list of files to include and the output name of the resulting file.   
Both the object resulting from the methods have a `minify` method, which will minify all the contents of the resulting output file.

As in the given example, you can pack together files with different extensions, like both *.js* and *.coffee* for the javascript and *.css*, *.sass*, *.scss* for the styles. The extension will take care of them, applying the appropriate filters on the files which need them.

Once you have defined your bundles, you can register them to your application using the `Assets.register` method, where the first parameter is the identifier of your bundle, and the second paramtere is the bundle you've just declared.

When you want to add your bundles to the templates, just use the `assets` lexer followed by the identifier:

```html
<!DOCTYPE html>
<html lang="{{=current.request.language or 'en'}}">
    <head>
        <title>Title</title>
        <meta charset="utf-8" />

        {{include_meta}}
        {{include_helpers}}
        {{assets 'base_js'}}
        {{assets 'base_css'}}
```

and the extension will take care of the rest.

## Suggested .gitignore

If you're using git version control system for your application, you'd better add the *gen* folder (or the one you've set) to your *.gitignore* file:

```
static/gen/*
```

and if you're using a [package pattern](http://weppy.org/docs/latest/patterns):

```
yourpackagename/static/gen/*
```
