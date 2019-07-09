
Line profiles
===========================

.. tabs::

    .. code-tab:: idl

        ; Line profiles are handled by the 'AmesPAHdbIDLSuite_Transitions'-object
        ; and it provides three profiles; Lorentzian, Gaussian and Drude.
        ; Convolution with the keyword chosen line profile is achieved through
        ; the 'AmesPAHdbIDLSuite_Transitions'-object's 'Convolve'-method, which
        ; will return the convolved spectrum in the form of an
        ; 'AmesPAHdbIDLSuite_Spectrum'-object.

        spectrum = transitions->Convolve(/Drude)

        ; Optionally, the 'Convolve'-method accepts the 'FWHM', 'Grid', 'NPoints',
        ; and 'XRange'-keywords, which control the full-width-at-half-maximum
        ; of the selected line profile (in cm-1), convolution onto a specified
        ; grid, the number of resolution elements in the generated spectrum,
        ; and the frequency range (in cm-1) of the spectrum.

        spectrum = transitions->Convolve(/Drude, FWHM=20D, Grid=myGrid)

        ; The 'AmesPAHdbIDLSuite_Spectrum'-object exposes the convolved spectra
        ; and provides the 'Plot', and 'Write'-methods. The 'Plot'-method will
        ; display the spectrum of each PAH species in a different color. The
        ; 'Write'-method will write all spectra to a single text (.txt) file.
        ; Optionally, a prefix can be given that will be prepended to the
        ; filename.

        spectrum->Plot

        spectrum->Write,'myPrefix'

        ; Optionally, the 'Wavelength', 'Stick', 'Oplot', 'Legend', and
        ; 'Color'-keywords can be given to the 'Plot'-method to control abscissa,
        ; stick representation, overplotting, legend and color, respectively.
        ; Through IDL's keyword inheritance mechanism additional keywords
        ; accepted by IDL's 'PLOT'-procedure can be passed.

        spectrum->Plot,/Wavelength,XRANGE=[2.5,15],/XSTYLE


    .. code-tab:: py

        # Interaction with the NASA Ames PAH IR Spectroscopic Database is organized around the 'Amespahdbpythonsuite'-object, which is created as shown below.
        
        from amespahdbpythonsuite.pahdb import Amespahdbpythonsuite
        pahdb = Amespahdbpythonsuite(cache=False, check=False)

