
Accessing data
===================

The molecular PAH data is accessed through their associated unique
identifiers (UIDs). However, a search-interface is available that
allows retrieval of these UIDs through simplified queries. The syntax
is identical to that used on the NASA Ames PAH IR Spectroscopic
Database website.

.. tabs::

    .. code-tab:: idl

        uids = pahdb->Search("c<=20 neutral n=2 neutral")

    .. code-tab:: python

        uids = pahdb.search("c<=20 neutral n=2 neutral")

The molecular PAH data contained in the database XML-files consists of
four main components:

1. Properties, .e.g, molecular weight, zero point energy,
   total energy, etc.

2. Vibrational transitions

3. Geometric data

4. Laboratory spectra

The following illustrates how to access these components.

.. tabs::

    .. code-tab:: idl

        pahdb = OBJ_NEW('AmesPAHdbIDLSuite')

        uids = pahdb->Search("c<=20 neutral n=2 neutral")

        pahs = pahdb->getSpeciesByUID(uids)

        transitions = pahdb->getTransitionsByUID(uids)

        geometries = pahdb->getGeometryByUID(uids)

        laboratory = pahdb->getLaboratoryByUID(uids)

        ;; work ...

        OBJ_DESTROY,[laboratory, $
                     geometries, $
                     transitions, $
                     pahs, $
                     pahdb]

    .. code-tab:: python

        pahdb = Amespahdbpythonsuite()

        uids = pahdb.search("c<=20 neutral n=2 neutral")

        pahs = pahdb.getspeciesbyuid(uids)

        transitions = pahdb.gettransitionsbyuid(uids)

        geometry = pahdb.getgeometrybyuid(uids)

        laboratory = pahdb.getlaboratorybyuid(uids)

        # work ...

Alternatively, one can access these components through the
'species'-instance.

.. tabs::

    .. code-tab:: idl

        pahdb = OBJ_NEW('AmesPAHdbIDLSuite')

        pahs = pahdb->getSpeciesByUID( $
               pahdb->Search("c<=20 neutral n=2 neutral"))

        transitions = pahs->transitions()

        geometries = pahs->geometry()

        laboratory = pahs->laboratory()

        ;; work ...

        OBJ_DESTROY,[laboratory, $
                     geometries, $
                     transitions, $
                     pahs, $
                     pahdb]

    .. code-tab:: python

        pahdb = Amespahdbpythonsuite()

        pahs = pahdb.getspeciesbyuid( \
	             pahdb.search("c<=20 neutral n=2 neutral"))

        transitions = pahs.transitions()

        geometry = pahs.geometry()

        laboratory = pahs.laboratory()

        # work ...

