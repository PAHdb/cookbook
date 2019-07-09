
##############
Common tasks
##############

First we must create the ``pahdb`` object, regardless of whether we are using the Python suite or the IDL suite.

.. tabs::

    .. code-tab:: idl

        ; Interaction with the NASA Ames PAH IR Spectroscopic Database is
        ; organized around the 'AmesPAHdbIDLSuite'-object, which is created
        ; as shown below.

        pahdb = OBJ_NEW('AmesPAHdbIDLSuite')

        ; Alternatively, the 'AmesPAHdbIDLSuite'-object can be created through
        ; the implicit object creation method as shown below.

        pahdb = AmesPAHdbIDLSuite()

        ; In case the system/IDL variable for the default database is not set,
        ; or one wishes to load another version or type of database, the
        ; 'Filename'-keyword can be specified.

        pahdb = OBJ_NEW('AmesPAHdbIDLSuite', Filename='/path/to/xml-file')

        ; By default the AmesPAHdbIDLSuite will cache the parsed database XML-file
        ; for faster subsequent access. However, this behavior can be disabled
        ; by setting the 'Cache'-keyword to '0'.

        pahdb = OBJ_NEW('AmesPAHdbIDLSuite', Cache=0)

        ; When parsing the database XML-file, the AmesPAHdbIDLSuite will try
        ; and validate its content against a URL-linked Schema. However,
        ; validation can be disabled by setting the 'Check'-keyword to '0'.
        ; This can be useful when not having an active internet connection.

        pahdb = OBJ_NEW('AmesPAHdbIDLSuite', Check=0)

        ; It is possible to combine the different keywords.

        pahdb = OBJ_NEW('AmesPAHdbIDLSuite', Filename='path/to/xml-file', $
                                             Cache=0, Check=0)

        ; When finished with the AmesPAHdbIDLSuite the object should be destroyed.

        OBJ_DESTROY,pahdb


    .. code-tab:: py

        # Interaction with the NASA Ames PAH IR Spectroscopic Database is organized around the 'Amespahdbpythonsuite'-object, which is created as shown below.
        
        from amespahdbpythonsuite.pahdb import Amespahdbpythonsuite
        pahdb = Amespahdbpythonsuite(cache=False, check=False)


Now we examine common tasks you might wish to do.


Searching
============

The molecular PAH data is accessed through their associated unique identifier (UID). However, a search-interface is available that allows retrieval of these UIDs through simplified queries. The syntax is the same as used on the NASA Ames PAH IR Spectroscopic Database website.

.. tabs::

    .. code-tab:: idl

        uids = pahdb->Search("c<=20 neutral n=2 neutral")

    .. code-tab:: py

        uids = pahdb.search("c<=20 neutral n=2 neutral")


Species
============

The 'AmesPAHdbIDLSuite_Species'-object exposes the molecular PAH properties. 

.. tabs::

    .. code-tab:: idl

        pahs = pahdb->getSpeciesByUID(uids)

    .. code-tab:: py

        pahs = pahdb.getspeciesbyuid(uids)


Transitions
============

The 'AmesPAHdbIDLSuite_Transitions'-object exposes the fundamental vibrational transitions. 

.. tabs::

    .. code-tab:: idl

        transitions = pahdb->getTransitionsByUID(uids)

    .. code-tab:: py

        transitions = pahdb.gettransitionsbyuid(uids)
        

.. # Calculate the emission spectrum at the temperature
.. # reached after absorbing a 4 eV (CGS units) photon
.. transitions.calculatedtemperature(4.0 * 1.603e-12)

.. # Plot the emission 'stick' spectrum at that temperature
.. transitions.plot()

.. # Convolve the bands with a Lorentzian with
.. # FWHM of 30 /cm
.. spectrum = transitions.convolve(fwhm=30.0)

.. # Plot the convolved spectrum
.. spectrum.plot()


The 'AmesPAHdbIDLSuite_Transitions'-object's 'Plot'-method will display the fundamental vibrational transitions in a 'stick'-plot. The transitions of each PAH species will be presented in a different color. 

.. tabs::

    .. code-tab:: idl

        transitions->Plot

    .. code-tab:: py

        transitions.plot()


Geometry
============

The 'AmesPAHdbIDLSuite_Geometry'-object exposes the molecular geometric data. 

.. tabs::

    .. code-tab:: idl

        geometries = pahdb->getGeometryByUID(uids)

    .. code-tab:: py

        geometries = pahdb.getgeometrybyuid(uids)


The 'AmesPAHdbIDLSuite_Geometry'-object provides both the 'Plot' and 'Structure'-methods to output the chemical structure of the PAH with provided UID. The first method uses IDL's PLOT-procedures to create the output, while the latter IDL object graphics. 

.. tabs::

    .. code-tab:: idl

        void = geometry->Plot(18)
        img = geometry->Structure(18)

    .. code-tab:: py

        transitions.plot()


Laboratory
============

The 'AmesPAHdbIDLSuite_Laboratory'-object exposes available raw laboratory spectra when an experimental database XML-file is loaded.

.. tabs::

    .. code-tab:: idl

        laboratory = pahdb->getLaboratoryByUID(uids)

    .. code-tab:: py

        laboratory = pahdb.getlaboratorybyuid(uids)

.. # Get the integrated cross-sections for coronene
.. transitions = pahdb.gettransitionsbyuid([18])

.. # Plot the 'stick' spectrum
.. transitions.plot()

.. # Calculate the emission spectrum at the temperature
.. # reached after absorbing a 4 eV (CGS units) photon
.. transitions.calculatedtemperature(4.0 * 1.603e-12)

.. # Plot the emission 'stick' spectrum at that temperature
.. transitions.plot()

.. # Convolve the bands with a Lorentzian with
.. # FWHM of 30 /cm
.. spectrum = transitions.convolve(fwhm=30.0)

.. # Plot the convolved spectrum
.. spectrum.plot()