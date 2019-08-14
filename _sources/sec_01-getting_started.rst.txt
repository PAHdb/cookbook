
Getting started
===========================

The IDL and Python software tools can be obtained from their GitHub
repositories located at `github.com/PAHdb/AmesPAHdbIDLSuite
<https://github.com/PAHdb/AmesPAHdbIDLSuite>`_ and
`github.com/PAHdb/AmesPAHdbPythonSuite
<https://github.com/PAHdb/AmesPAHdbPythonSuite>`_,
respectively. Instructions on installing the software tools can be
found in the accompanying documentation.

Every interaction with the NASA Ames PAH IR Spectroscopic Database
starts out with the creation of a 'pahdb'-instance, as is illustrated
below.

.. tabs::

    .. code-tab:: idl

        ;; Using the OBJ_NEW-function

	pahdb = OBJ_NEW('AmesPAHdbIDLSuite')

	;; Alternatively, using implicit object creation

	pahdb = AmesPAHdbIDLSuite()
    .. code-tab:: python

        from amespahdbpythonsuite.pahdb import Amespahdbpythonsuite
        pahdb = amespahdbpythonsuite()

In case the system variable for the default database is not set, or
one wishes to load another version or type of database, the
'Filename'-keyword can be specified.

.. tabs::

    .. code-tab:: idl

        pahdb = OBJ_NEW('AmesPAHdbIDLSuite', Filename='/path/to/xml-file')

    .. code-tab:: python

        pahdb = Amespahdbpythonsuite(filename='/path/to/xml-file')

By default the parsed database XML-file will be cached for faster
subsequent access. However, this behavior can be disabled by setting
the 'Cache'-keyword to false.

.. tabs::

    .. code-tab:: idl

        pahdb = OBJ_NEW('AmesPAHdbIDLSuite', Cache=0)

    .. code-tab:: python

        pahdb = Amespahdbpythonsuite(cache=False)

When parsing a database XML-file, the software will validate its
content against a URL-linked Schema. However, validation can be
disabled by setting the 'Check'-keyword to false.  This can be useful
when not having an active internet connection.

.. tabs::

    .. code-tab:: idl

        pahdb = OBJ_NEW('AmesPAHdbIDLSuite', Check=0)

    .. code-tab:: python

        pahdb = Amespahdbpythonsuite(check=False)

Of course, it is possible to combine the different keywords.

.. tabs::

    .. code-tab:: idl

        pahdb = OBJ_NEW('AmesPAHdbIDLSuite', Filename='path/to/xml-file', $
                                     Cache=0, Check=0)

    .. code-tab:: python

        pahdb = Amespahdbpythonsuite(filename='/path/to/xml-file',
                                     cache=False, check=False)

Lastly, when finished with the 'pahdb'-instance it should be destroyed
when no garbage collection is available.

.. tabs::

    .. code-tab:: idl

        OBJ_DESTROY,pahdb

    .. code-tab:: python

       del pahdb
