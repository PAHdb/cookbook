
Laboratory
============

Working with raw laboratory spectra

.. tabs::

    .. code-tab:: idl

        ; The 'AmesPAHdbIDLSuite_Laboratory'-object exposes available raw
        ; laboratory spectra when an experimental database XML-file is loaded.

        laboratory = pahdb->getLaboratoryByUID( $
                     pahdb->Search("c<=20 neutral n=2 neutral"))

        ; The 'AmesPAHdbIDLSuite_Laboratory-object's 'Plot'-method will display
        ; the raw laboratory spectra. The spectrum of each PAH species will be
        ; presented in a different color.

        laboratory->Plot

        ; Optionally, the 'Wavelength', 'Oplot', and 'Color'-keywords can be
        ; given to the 'Plot'-method to control the abscissa, overplotting, and
        ; color, respectively. Through IDL's keyword inheritance mechanism
        ; additional keywords accepted by IDL's 'PLOT'-procedure can be passed.

        laboratory->Plot,/Wavelength,Color=5,XRANGE=[2.5,15],/XSTYLE

        ; The 'AmesPAHdbIDLSuite_Laboratory-object's 'Write'-method will write
        ; the raw laboratory spectrum to file. The spectrum of each PAH species
        ; will be written to a separate text (.txt) file, where each filename
        ; will have the PAH species UID embedded. Optionally, a prefix can be
        ; given that will be prepended to the filename.

        laboratory->Write,'myPrefix'


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