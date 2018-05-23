``extends``
===========

The ``extends`` tag can be used to extend a template from another one.

.. note::

    Like PHP, Twig does not support multiple inheritance. So you can only have
    one extends tag called per rendering. However, Twig supports horizontal
    :doc:`reuse<use>`.

Let's define a base template, ``base.html``, which defines a simple HTML
skeleton document:

.. code-block:: html+jinja

    <!DOCTYPE html>
    <html>
        <head>
            {% block head %}
                <link rel="stylesheet" href="style.css" />
                <title>{% block title %}{% endblock %} - My Webpage</title>
            {% endblock %}
        </head>
        <body>
            <div id="content">{% block content %}{% endblock %}</div>
            <div id="footer">
                {% block footer %}
                    &copy; Copyright 2011 by <a href="http://domain.invalid/">you</a>.
                {% endblock %}
            </div>
        </body>
    </html>

In this example, the :doc:`block<block>` tags define four blocks that child
templates can fill in.

All the ``block`` tag does is to tell the template engine that a child
template may override those portions of the template.

Child Template
--------------

A child template might look like this:

.. code-block:: jinja

    {% extends "base.html" %}

    {% block title %}Index{% endblock %}
    {% block head %}
        {{ parent() }}
        <style type="text/css">
            .important { color: #336699; }
        </style>
    {% endblock %}
    {% block content %}
        <h1>Index</h1>
        <p class="important">
            Welcome on my awesome homepage.
        </p>
    {% endblock %}

The ``extends`` tag is the key here. It tells the template engine that this
template "extends" another template. When the template system evaluates this
template, first it locates the parent. The extends tag should be the first tag
in the template.

Note that since the child template doesn't define the ``footer`` block, the
value from the parent template is used instead.

You can't define multiple ``block`` tags with the same name in the same
template. This limitation exists because a block tag works in "both"
directions. That is, a block tag doesn't just provide a hole to fill - it also
defines the content that fills the hole in the *parent*. If there were two
similarly-named ``block`` tags in a template, that template's parent wouldn't
know which one of the blocks' content to use.

If you want to print a block multiple times you can however use the
``block`` function:

.. code-block:: jinja

    <title>{% block title %}{% endblock %}</title>
    <h1>{{ block('title') }}</h1>
    {% block body %}{% endblock %}

Parent Blocks
-------------

It's possible to render the contents of the parent block by using the
:doc:`parent<../functions/parent>` function. This gives back the results of
the parent bloc