# django-bootstrap3-datepicker

This package is a fork of django-bootstrap3-datetimepicker v2.3, available here:

[https://github.com/nkunihiko/django-bootstrap3-datetimepicker](https://github.com/nkunihiko/django-bootstrap3-datetimepicker)

Where the above uses the Bootstrap v3 datetimepicker, this package uses Bootstrap v3 datepicker widget version 1.3.0, provided by the following project:

[https://github.com/eternicode/bootstrap-datepicker](https://github.com/eternicode/bootstrap-datepicker)

The correct formatting options for dates can be found here:

[https://docs.python.org/2/library/datetime.html#strftime-and-strptime-behavior](https://docs.python.org/2/library/datetime.html#strftime-and-strptime-behavior)

It has only been tested with Bootstrap3.

## Install

-  Run ``pip install django-bootstrap3-datepicker``
-  Add ``'bootstrap3_datepicker'`` to your ``INSTALLED_APPS``

## Example

forms.py
        
    from django import forms

    from bootstrap3_datepicker.fields import DatePickerField
    from bootstrap3_datepicker.widgets import DatePickerInput


    class ToDoForm(forms.Form):
        # normal date field with no picker
        date_1 = forms.DateField()

        # date field with default picker, uses locale default
        # date format for display and decode
        date_2 = forms.DateField(widget=DatePickerInput())

        # date field with picker, uses specified format for display,
        # decode is via locale default input_formats so format specified
        # must match one
        date_3 = forms.DateField(widget=DatePickerInput(format="%Y-%m-%d"))

        # date field with picker, uses specified format for display and
        # input_formats for decode
        date_4 = forms.DateField(input_formats=["%B %Y"],
                                 widget=DatePickerInput(format="%B %Y",
                                                        options={"minViewMode": "months"}))

        # date picker field, uses locale default date format for display
        # and decode, same as date_2
        date_5 = DatePickerField()

        # date field with picker, uses specified format for display and
        # decode, same as date_3
        date_6 = DatePickerField(input_formats=["%Y-%m-%d"])

        # custom format with picker options specified, uses specified format
        # for display and decode, same as date_4
        date_7 = DatePickerField(input_formats=["%B %Y"],
                                 picker_options={"minViewMode": "months"})

The `DatePickerInput` provides the integration with the JavaScript datepicker - the `options` specified will be passed directly to the JavaScript datepicker. The available `options` are explained in the following documents:

[http://bootstrap-datepicker.readthedocs.org/en/release/options.html](http://bootstrap-datepicker.readthedocs.org/en/release/options.html)

The `DatePickerField` has been provided as a utility for a common usage. When the `DatePickerInput` is specified as a widget on a `DateField` and a non-standard date format is required, the `format` for the JavaScript datapicker and `input_formats` for the `DateField` itself should both be specified and match. On the `DatePickerField` the `input_formats` provides both the display and decode formats. The `picker_options` are passed directly to the JavaScript datepicker.

template.html

    <!DOCTYPE html>
    <html>
        <head>
            <link rel="stylesheet" href="//netdna.bootstrapcdn.com/bootstrap/3.0.0/css/bootstrap.css">
            <link rel="stylesheet" href="//netdna.bootstrapcdn.com/bootstrap/3.0.0/css/bootstrap-theme.css">
            <script src="//ajax.googleapis.com/ajax/libs/jquery/1.9.0/jquery.js"></script>
            <script src="//netdna.bootstrapcdn.com/bootstrap/3.0.0/js/bootstrap.js"></script>
            {{ form.media }}
            <style>
                body {
                    margin: 40px auto;
                    width: 90%;
                }
                form {
                    margin: 40px auto;
                    min-width: 200px;
                    max-width: 500px;
                }
            </style>
        </head>
        <body>
            <form method="post" role="form">
                {% for field in form.visible_fields %}
                <div id="div_{{ field.html_name }}" 
                     class="form-group{% if field.errors %} has-error{% endif %}">
                    {{ field.label_tag }}
                    {{ field }}
                    <div class="text-muted pull-right">
                        <small>{{ field.help_text }}</small>
                    </div>
                    <div class="help-block">
                        {{ field.errors }}
                    </div>
                </div>
                {% endfor %}
                {% for hidden in form.hidden_fields %}
                    {{ hidden }}
                {% endfor %}
                {% csrf_token %}
                <div class="form-group">
                    <input type="submit" value="Validate" class="btn btn-primary" />
                </div>
            </form>
        </body>
    </html>

Bootstrap3 and jQuery have to be included along with `{{ form.media }}`.

## Release Notes

### v0.1

- Initial release.

## Requirements

-  Python >= 2.7
-  Django >= 1.5
-  Bootstrap >= 3.0
