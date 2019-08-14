
Working with laboratory data
====================================

The 'laboratory'-instance exposes available raw laboratory spectra
when an experimental database XML-file is loaded.

.. tabs::

    .. code-tab:: idl

        uids = pahdb->Search("c<=20 neutral n=2 neutral")

        laboratory = pahdb->getLaboratoryByUID(uids)

    .. code-tab:: python

        uids = pahdb.search("c<=20 neutral n=2 neutral")

        laboratory = pahdb.getlaboratorybyuid(uids)

The 'laboratory'-instance's 'Plot'-method will display the raw
laboratory spectra. The spectrum of each PAH species will be presented
in a different color.

.. tabs::

    .. code-tab:: idl

        laboratory->Plot

    .. code-tab:: python

        laboratory.plot()

Optionally, the 'Wavelength', 'Oplot', and 'Color'-keywords can be
given to the 'Plot'-method to control the abscissa, overplotting, and
color, respectively. In the IDL case additional keywords accepted by
IDL's 'PLOT'-procedure can be passed via the keyword inheritance
mechanism.

.. tabs::

    .. code-tab:: idl

        laboratory->Plot,/Wavelength,Color=5,XRANGE=[2.5,15],/XSTYLE

    .. code-tab:: python

        laboratory.plot(wavelength=True,color='blue')

The 'laboratory'-instance's 'Write'-method will write the raw
laboratory spectrum to file. The spectrum of each PAH species will be
written to an IPAC table (.tbl). Optionally, a filename can be
provided.

.. tabs::

    .. code-tab:: idl

        laboratory->Write,'myFile'

    .. code-tab:: python

        laboratory.write('myFile')
