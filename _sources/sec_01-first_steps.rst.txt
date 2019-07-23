
First steps
===========================

Interaction with the NASA Ames PAH IR Spectroscopic Database is
organized around the 'AmesPAHdbIDLSuite'-object, which is created
as shown below.

.. tabs::

    .. code-tab:: idl

        # For the IDL suite, we can directly create the object.
        pahdb = OBJ_NEW('AmesPAHdbIDLSuite')

    .. code-tab:: python

        from amespahdbpythonsuite.pahdb import Amespahdbpythonsuite
        pahdb = Amespahdbpythonsuite()

Alternatively, the 'AmesPAHdbIDLSuite'-object can be created through
the implicit object creation method as shown below.

.. tabs::

    .. code-tab:: idl
        
        pahdb = AmesPAHdbIDLSuite()

    .. code-tab:: python

        pahdb = Amespahdbpythonsuite()

In case the system/IDL variable for the default database is not set,
or one wishes to load another version or type of database, the
'Filename'-keyword can be specified.

.. tabs::

    .. code-tab:: idl

        pahdb = OBJ_NEW('AmesPAHdbIDLSuite', Filename='/path/to/xml-file')

    .. code-tab:: python

        pahdb = Amespahdbpythonsuite(filename='/path/to/xml-file')

By default the AmesPAHdbIDLSuite will cache the parsed database XML-file
for faster subsequent access. However, this behavior can be disabled
by setting the 'Cache'-keyword to '0'.

.. tabs::

    .. code-tab:: idl

        pahdb = OBJ_NEW('AmesPAHdbIDLSuite', Cache=0)

    .. code-tab:: python

        pahdb = Amespahdbpythonsuite(cache=True)

When parsing the database XML-file, the AmesPAHdbIDLSuite will try
and validate its content against a URL-linked Schema. However,
validation can be disabled by setting the 'Check'-keyword to '0'.
This can be useful when not having an active internet connection.

.. tabs::

    .. code-tab:: idl

        pahdb = OBJ_NEW('AmesPAHdbIDLSuite', Check=0)

    .. code-tab:: python

        pahdb = Amespahdbpythonsuite(check=False)

It is possible to combine the different keywords.

.. tabs::

    .. code-tab:: idl

        pahdb = OBJ_NEW('AmesPAHdbIDLSuite', Filename='path/to/xml-file', $
                                     Cache=0, Check=0)

    .. code-tab:: python

        pahdb = Amespahdbpythonsuite(filename='/path/to/xml-file',
                                     cache=False, check=False)

When finished with the AmesPAHdbIDLSuite the object should be destroyed.

.. tabs::

    .. code-tab:: idl

        OBJ_DESTROY,pahdb

    .. code-tab:: python

        # N/A
