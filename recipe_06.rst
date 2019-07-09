
##################################
A complete example
##################################

A complete example of fitting an observation is shown below. It is an adaption of the 'fit_a_spectrum.pro'-procedure found in the examples-directory of the AmesPAHdbIDLSuite, and is followed by a detailed, line-by-line, explanation of the code.

.. tabs::

    .. code-tab:: idl

        ; An observation is read from 'myFile' and the
        ; 'AmesPAHdbIDLSuite_CREATE_OBSERVATION_UNITS_S'-helper function is
        ; called to associate units.
        observation = OBJ_NEW('AmesPAHdbIDLSuite_Observation', 'myFile', $
                               Units=AmesPAHdbIDLSuite_CREATE_OBSERVATION_UNITS_S())

        ; Observation abscissa units are converted to wavenumber. 
        observation->AbscissaUnitsTo,1

        ; The observation is rebinned onto a uniform grid spaced 5 wavenumbers. 
        observation->Rebin,5D,/Uniform

        ; The default NASA Ames PAH IR Spectroscopic Database XML-file is loaded 
        pahdb = OBJ_NEW('AmesPAHdbIDLSuite')

        ; The fundamental vibrational transitions from a subset of PAHs are retrieved. 
        transitions = pahdb->getTransitionsByUID( $
                      pahdb->Search("mg=0 o=0 fe=0 si=0 chx=0 ch2=0 c>20"))

        ; A FixedTemperature emission model at 600 Kelvin is applied. 
        transitions->FixedTemperature,600D

        ; The fundamental vibrational transitions are redshifted 15 wavenumbers. 
        transitions->Shift,-15D

        ; The fundamental vibrational transitions are convolved with Lorentzian
        ; profiles having a full-width-at-half-maximum of 20 cm-1 onto the
        ; observational grid. 
        spectrum = transitions->Convolve(/Lorentzian, $
                                         Grid=observation->getGrid(), $
                                         FWHM=20D)

        ; The observation is fitted with the PAH emission spectra.
        fit = spectrum->Fit(observation)

        ; Cleanup of 'spectrum'. 
        OBJ_DESTROY,spectrum

        ; Display several aspects of the fit. 
        fit->Plot,/Wavelength
        fit->Plot,/Wavelength,/Residual
        fit->Plot,/Wavelength,/Charge

        ; The transitions are intersected with the PAH species in the fit. 
        transitions->Intersect, $
                fit->getUIDs()

        ; The fundamental vibrational transitions are again convolved with
        ; Lorentzian profiles having a full-width-at-half-maximum of 20 cm-1,
        ; but now onto a generated grid from 3-10 micron. 
        spectrum = transitions->Convolve(/Lorentzian, $
                                         FWHM=20D, $
                                         XRange=1D4/[20D, 3D])

        ; The individual PAH spectra are added using weights retrieved from the
        ; fit. 
        coadded = spectrum->CoAdd(Weights=fit->getWeights())

        ; The coadded spectrum is displayed, revealing the entire 3-20 micron,
        ; predicted, PAH spectrum. 
        coadded->Plot

        ; Cleanup. 
        OBJ_DESTROY,[coadded, spectrum, fit, transitions, pahdb, observation]


    .. code-tab:: py

        # Interaction with the NASA Ames PAH IR Spectroscopic Database is organized around the 'Amespahdbpythonsuite'-object, which is created as shown below.
        
        from amespahdbpythonsuite.pahdb import Amespahdbpythonsuite
        pahdb = Amespahdbpythonsuite(cache=False, check=False)

