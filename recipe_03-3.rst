
Transitions
============================================================

Working with fundamental vibrational transitions

.. tabs::

    .. code-tab:: idl

        transitions = pahdb->getTransitionsByUID(uids)

        ; The 'AmesPAHdbIDLSuite_Transitions'-object exposes the fundamental
        ; vibrational transitions.

        transitions = pahdb->getTransitionsByUID( $
                      pahdb->Search("c<=20 neutral n=2 neutral"))

        ; The 'AmesPAHdbIDLSuite_Transitions-object's 'Print'-method will
        ; print out the associated fundamental vibrational transitions for each
        ; PAH species.

        transitions->Print

        ; Optionally, the 'Str'-keyword can be given to the 'Print'-method,
        ; which will return the associated fundamental vibrational transitions
        ; for each PAH species as a single, concatenated string.

        transitions->Print,Str=Str

        ; The 'AmesPAHdbIDLSuite_Transitions'-object's 'Plot'-method will
        ; display the fundamental vibrational transitions in a 'stick'-plot.
        ; The transitions of each PAH species will be presented in a different
        ; color.

        transitions->Plot

        ; Optionally, the 'Wavelength', 'Stick', 'Oplot', 'Legend', and
        ; 'Color'-keywords can be given to the 'Plot'-method to control the
        ; abscissa, stick representation, overplotting, legend and color,
        ; respectively. Through IDL's keyword inheritance mechanism additional
        ; keywords accepted by IDL's 'PLOT'-procedure can be passed.

        transitions->Plot,/Wavelength,Color=5,XRANGE=[2.5,15],/XSTYLE

        ; The 'AmesPAHdbIDLSuite_Transitions'-object's 'Write'-method will
        ; write the fundamental vibrational transitions to file. The transitions
        ; of each PAH species will be written to a separate text (.txt) file,
        ; where each filename will have the PAH species UID embedded. Optionally,
        ; a prefix can be given that will be prepended to the filename.

        transitions->Write,'myPrefix'


    .. code-tab:: py

        transitions = pahdb.gettransitionsbyuid(uids)
        
        # Calculate the emission spectrum at the temperature
        # reached after absorbing a 4 eV (CGS units) photon
        transitions.calculatedtemperature(4.0 * 1.603e-12)

        # Plot the emission 'stick' spectrum at that temperature
        transitions.plot()

        # Convolve the bands with a Lorentzian with
        # FWHM of 30 /cm
        spectrum = transitions.convolve(fwhm=30.0)

        # Plot the convolved spectrum
        spectrum.plot()

