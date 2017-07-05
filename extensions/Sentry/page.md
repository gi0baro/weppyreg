weppy-Sentry provides the integration for the [Sentry](https://getsentry.com) error reporting platform in weppy.

## Installation

You can install weppy-Sentry using pip:

    pip install weppy-Sentry

And add it to your weppy application:

```python
from weppy_sentry import Sentry

app.use_extension(Sentry)
```

## Configuration

weppy-Sentry has 2 configuration options:

| parameter | default value | description |
| --- | --- | --- |
| `dsn` | `''` | the Sentry DSN for your application |
| `auto_load` | `True` | automatically send exceptions to Sentry |

The `dsn` parameter is **required** or the extension won't do anything, and you should declare it before loading the exension:

```python
app.config.Sentry.dsn = 'mysecretdsn'
app.use_extension(Sentry)
```

You can obviously configure the extension using a YAML file:

```yaml
dsn: mysecretdsn
```

and load it before using the extension:

```python
app.config_from_yaml('sentry.yml', 'Sentry')
app.use_extension(Sentry)
```

When the `auto_load` parameter is set to `True`, the extension will add a pipe into your application pipeline, that will log any uncaught exception in your application code and your templates.

If you want to manually set the pipe into your application pipeline, you should set the `auto_load` parameter to `False`, init the extension and then add the pipe to your application's ones. Mind that, using this configuration, Sentry won't be able to track errors in your templates:

```python
app.config.Sentry.auto_load = False
app.use_extension(Sentry)

app.pipeline = [app.ext.Sentry.pipe]
```

Finally, if you just don't want the error tracking on exceptions, just set the parameter to `False` and don't add the pipe to your application.

## Manual usage

The extension has two methods you can invoke manually, the `exception` and the `message` ones.

You can use the first one when you want to track catched exceptions:

```python
@app.route()
def some_route():
    try:
        # some code
    except:
        app.ext.Sentry.exception()
        # some code
```

while the second can be used to store messages into your Sentry project:

```python
app.ext.Sentry.message('hello from my awesome app!')
```
