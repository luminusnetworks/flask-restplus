Fields masks
============

Flask-Restplus support partial object fetching (aka. fields mask)
by suppling a custom header in the request.

By default the header is ``X-Fields``
but it ca be changed with the ``RESTPLUS_MASK_HEADER`` parameter.

Syntax
------

The syntax is actually quite simple.
You just provide a coma separated list of field names,
optionaly wrapped in brackets.

.. code-block:: python

    # These two mask are equivalents
    mask = '{name,age}'
    # or
    mask = 'name,age'
    data = requests.get('/some/url/', headers={'X-Fields': mask})
    assert len(data) == 2
    assert 'name' in data
    assert 'age' in data

To specify a nested fields mask,
simply provide it in bracket following the field name:

.. code-block:: python

    mask = '{name, age, pet{name}}'

Nesting specification works with nested object or list of objects:

.. code-block:: python

    # Will apply the mask {name} to each pet
    # in the pets list.
    mask = '{name, age, pets{name}}'

There is a special star token meaning "all remaining fields".
It allows to only specify nested filtering:

.. code-block:: python

    # Will apply the mask {name} to each pet
    # in the pets list and take all other root fields
    # without filtering.
    mask = '{pets{name},*}'

    # Will not filter anything
    mask = '*'


Usage
-----

By default, each time you use ``api.marshal`` or ``@api.marshal_with``,
the mask will be automatically applied if the header is present.

The header will be exposed as a Swagger parameter each time you use the
``@api.marshal_with`` decorator.

As Swagger does not permet to expose a global header once
so it can make your Swagger specifications a lot more verbose.
You can disable this behavior by setting ``RESTPLUS_MASK_SWAGGER`` to ``False``.
