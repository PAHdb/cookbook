
Working with spectra
===========================

Line profiles are handled by the 'AmesPAHdbIDLSuite_Transitions'-object
and it provides three profilesLorentzian, Gaussian and Drude.
Convolution with the keyword chosen line profile is achieved through
the 'AmesPAHdbIDLSuite_Transitions'-object's 'Convolve'-method, which
will return the convolved spectrum in the form of an
'AmesPAHdbIDLSuite_Spectrum'-object.

.. tabs::

    .. code-tab:: idl

        spectrum = transitions->Convolve(/Drude)

    .. code-tab:: python

        spectrum = transitions.convolve(fwhm=30.0)

Optionally, the 'Convolve'-method accepts the 'FWHM', 'Grid', 'NPoints',
and 'XRange'-keywords, which control the full-width-at-half-maximum
of the selected line profile (in cm-1), convolution onto a specified
grid, the number of resolution elements in the generated spectrum,
and the frequency range (in cm-1) of the spectrum.

.. tabs::

    .. code-tab:: idl

        spectrum = transitions->Convolve(/Drude, FWHM=20D, Grid=myGrid)

    .. code-tab:: python

        spectrum = transitions.convolve(fwhm=30.0)

The 'AmesPAHdbIDLSuite_Spectrum'-object exposes the convolved spectra
and provides the 'Plot', and 'Write'-methods. The 'Plot'-method will
display the spectrum of each PAH species in a different color. The
'Write'-method will write all spectra to a single text (.txt) file.
Optionally, a prefix can be given that will be prepended to the
filename.

.. tabs::

    .. code-tab:: idl

        spectrum->Plot

        spectrum->Write,'myPrefix'

    .. code-tab:: python

        spectrum.plot()

Optionally, the 'Wavelength', 'Stick', 'Oplot', 'Legend', and
'Color'-keywords can be given to the 'Plot'-method to control abscissa,
stick representation, overplotting, legend and color, respectively.
Through IDL's keyword inheritance mechanism additional keywords
accepted by IDL's 'PLOT'-procedure can be passed.

.. tabs::

    .. code-tab:: idl

        spectrum->Plot,/Wavelength,XRANGE=[2.5,15],/XSTYLE

    .. code-tab:: python

        spectrum.plot()