
Working with spectra
===========================


The 'spectrum'-instance exposes convolved spectra and provides the
'Plot', and 'Write'-methods. The 'Plot'-method will display the
spectrum of each PAH species in a different color. The 'Write'-method
will write all spectra to an IPAC table (.tbl).  Optionally, a
filename can be provided.

.. tabs::

    .. code-tab:: idl

        spectrum->Plot

        spectrum->Write,'myFile'

    .. code-tab:: python

        spectrum.plot()

	spectrum.write('myFile')

Optionally, the 'Wavelength', 'Stick', 'Oplot', 'Legend', and
'Color'-keywords can be given to the 'Plot'-method to control
abscissa, stick representation, overplotting, legend and color,
respectively. In the IDL case additional keywords accepted by IDL’s
‘PLOT’-procedure can be passed via the keyword inheritance mechanism.

.. tabs::

    .. code-tab:: idl

        spectrum->Plot,/Wavelength,XRANGE=[2.5,15],/XSTYLE

    .. code-tab:: python

        spectrum.plot(wavelength=True)
