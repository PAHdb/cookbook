
Working with laboratory data
====================================

The 'AmesPAHdbIDLSuite_Laboratory'-object exposes available raw
laboratory spectra when an experimental database XML-file is loaded.

.. tabs::

    .. code-tab:: idl

        uids = pahdb->Search("c<=20 neutral n=2 neutral")

        laboratory = pahdb->getLaboratoryByUID(uids)

    .. code-tab:: python

        uids = pahdb.search("c<=20 neutral n=2 neutral")
        
        laboratory = pahdb.getlaboratorybyuid(uids)

The 'AmesPAHdbIDLSuite_Laboratory-object's 'Plot'-method will display
the raw laboratory spectra. The spectrum of each PAH species will be
presented in a different color.

.. tabs::

    .. code-tab:: idl

        laboratory->Plot

    .. code-tab:: python

        laboratory.plot()

Optionally, the 'Wavelength', 'Oplot', and 'Color'-keywords can be
given to the 'Plot'-method to control the abscissa, overplotting, and
color, respectively. Through IDL's keyword inheritance mechanism
additional keywords accepted by IDL's 'PLOT'-procedure can be passed.

.. tabs::

    .. code-tab:: idl

        laboratory->Plot,/Wavelength,Color=5,XRANGE=[2.5,15],/XSTYLE

    .. code-tab:: python

        laboratory.plot()
        
The 'AmesPAHdbIDLSuite_Laboratory-object's 'Write'-method will write
the raw laboratory spectrum to file. The spectrum of each PAH species
will be written to a separate text (.txt) file, where each filename
will have the PAH species UID embedded. Optionally, a prefix can be
given that will be prepended to the filename.

.. tabs::

    .. code-tab:: idl

        laboratory->Write,'myPrefix'

    .. code-tab:: python

        # Placeholder
