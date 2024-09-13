

Fit a spectrum
====================

A complete example of fitting an astronomical spectrum is shown
below. It is an adaption of 'fit_a_spectrum' found in the
examples-directory of the software tools, and is followed by a
detailed, line-by-line, explanation of the code.

.. tabs::

    .. code-tab:: idl
        :linenos:

        observation = OBJ_NEW('AmesPAHdbIDLSuite_Observation', 'myFile', $
                               Units=AmesPAHdbIDLSuite_CREATE_OBSERVATION_UNITS_S())

        observation->AbscissaUnitsTo,1

        observation->Rebin,5D,/Uniform

        pahdb = OBJ_NEW('AmesPAHdbIDLSuite')

        transitions = pahdb->getTransitionsByUID( $
                      pahdb->Search("mg=0 o=0 fe=0 si=0 chx=0 ch2=0 c>20"))

        transitions->FixedTemperature,600D

        transitions->Shift,-15D

        spectrum = transitions->Convolve(/Lorentzian, $
                                         Grid=observation->getGrid(), $
                                         FWHM=20D)

        fit = spectrum->Fit(observation)

        OBJ_DESTROY,spectrum

        fit->Plot,/Wavelength

        fit->Plot,/Wavelength,/Residual

        fit->Plot,/Wavelength,/Charge

        transitions->Intersect, $
                fit->getUIDs()

        spectrum = transitions->Convolve(/Lorentzian, $
                                         FWHM=20D, $
                                         XRange=1D4/[20D, 3D])

        coadded = spectrum->CoAdd(Weights=fit->getWeights())

        coadded->Plot

        OBJ_DESTROY,[coadded, spectrum, fit, transitions, pahdb, observation]


    .. code-tab:: python
        :linenos:

        import importlib_resources
        import numpy as np

        from amespahdbpythonsuite import observation
        from amespahdbpythonsuite.amespahdb import AmesPAHdb

        file_path = importlib_resources.files("amespahdbpythonsuite")
        data_file = file_path / "resources/galaxy_spec.ipac"
        obs = observation.Observation(data_file)

        obs.abscissaunitsto("1/cm")

        xml_file = file_path / "resources/pahdb-theoretical_cutdown.xml"
        pahdb = AmesPAHdb(
            filename=xml_file,
            check=False,
            cache=False,
        )

        uids = pahdb.search("c>0")
        transitions = pahdb.gettransitionsbyuid(uids)

        transitions.cascade(6 * 1.603e-12, multiprocessing=False)

        transitions.shift(-15.0)

        spectrum = transitions.convolve(
            grid=obs.getgrid(), fwhm=20.0, gaussian=True, multiprocessing=False
        )

        fit = spectrum.fit(obs)

        fit.plot(wavelength=True)
        fit.plot(wavelength=True, residual=True)
        fit.plot(wavelength=True, size=True)
        fit.plot(wavelength=True, charge=True)
        fit.plot(wavelength=True, composition=True)

        transitions.intersect(fit.getuids())

        xrange = 1e4 / np.array([20.0, 3.0])

        spectrum = transitions.convolve(
            xrange=xrange, fwhm=20.0, gaussian=True, multiprocessing=False
        )

        coadded = spectrum.coadd(weights=fit.getweights())

        coadded.plot()

A line-by-line explanation of the code follows.

 .. tabs::

    .. group-tab:: IDL

        lines 1-2: An observation is read from 'myFile' and the
        'AmesPAHdbIDLSuite_CREATE_OBSERVATION_UNITS_S'-helper function
        is called to associate units.

        line 4: Observation abscissa units are converted to
        wavenumber.

        line 6: The observation is rebinned onto a uniform grid spaced
        5 cm\ :sup:`-1`.

        line 8: The default NASA Ames PAH IR Spectroscopic Database
        XML-file is loaded.

        lines 10-11: The fundamental vibrational transitions from a subset
        of PAHs are retrieved.

        line 13: A FixedTemperature emission model at 600 Kelvin is
        applied.

        line 15: The fundamental vibrational transitions are
        redshifted 15 cm\ :sup:`-1`.

        line 17: The fundamental vibrational transitions are convolved
        with Lorentzian profiles having a full-width-at-half-maximum
        of 20 cm\ :sup:`-1` onto the observational grid.

        line 21: The observation is fitted with the PAH emission
        spectra.

        line 23: Cleanup of 'spectrum'.

        lines 25-29: Display several aspects of the fit.

        line 31: The transitions are intersected with the PAH species
        in the fit.

        line 34: The fundamental vibrational transitions are again convolved
        with Lorentzian profiles having a full-width-at-half-maximum of 20 cm\
        :sup:`-1`, but now onto a generated grid from 3-20 micron.

        line 38: The individual PAH spectra are added using weights
        retrieved from the fit.

        line 40: The coadded spectrum is displayed, revealing the
        entire 3-20 micron, predicted, PAH spectrum.

        line 42: Cleanup.

        Line 25: A figure is displayed showing the fitted spectrum and
        its components, as shown below.

    .. group-tab:: Python

        lines 1-5: Importing the necessary modules.

        lines 7-9: A spectrum included with the suite is loaded.

        line 11: Observation abscissa units are converted to
        wavenumber.

        lines 13-18: The cutdown version of the database XML-file included with the suite is loaded.

        lines 20-21: The fundamental vibrational transitions of PAHs are retrieved.

        line 23: A full temperature cascade emission model at 6 eV is applied.

        line 25: The fundamental vibrational transitions are redshifted 15 cm\
        :sup:`-1`.

        line 27: The fundamental vibrational transitions are convolved with
        Lorentzian profiles having a full-width-at-half-maximum of 20 cm\
        :sup:`-1` onto the observational grid.

        line 31: The observation is fitted with the PAH emission spectra.

        lines 33-37: Display several aspects of the fit.

        line 39: The transitions are intersected with the PAH species in the
        fit.

        line 41: Define a 3-20 micron grid in wavenumbers.

        lines 43-45: The fundamental vibrational transitions are again
        convolved with Lorentzian profiles having a full-width-at-half-maximum
        of 20 cm\:sup:`-1`, but now onto the generated 3-20 micron grid.

        line 47: The individual PAH spectra are added using weights retrieved
        from the fit.

        line 49: The coadded spectrum is displayed, revealing the entire 3-20 micron, predicted, PAH spectrum.

Below some examples of the generated output.

.. tabs::

    .. group-tab:: IDL

        .. figure:: figures/Screenshots/IDL/complete_example/1.png
           :align: center

	   Result of a PAHdb-fit to the 10-15 micron spectrum of NGC
           7023 showing the contribution from different PAHs.


    .. group-tab:: Python

        .. figure:: figures/Screenshots/Python/complete_example/1.png
           :align: center

           Result of a PAHdb-fit to the 10-15 micron spectrum of NGC
           7023 showing the contribution from different PAHs.


.. tabs::

    .. group-tab:: IDL

        .. figure:: figures/Screenshots/IDL/complete_example/2.png
           :align: center

	   Top: Result of a PAHdb-fit to the 10-15 micron spectrum of
           NGC 7023 showing the contribution from different
           PAHs. Bottom: Residual of the fit.

    .. group-tab:: Python

        .. figure:: figures/Screenshots/Python/complete_example/2.png
           :align: center

	   Top: Result of a PAHdb-fit to the 10-15 micron spectrum of
           NGC 7023 showing the contribution from different
           PAHs. Bottom: Residual of the fit.


.. tabs::

    .. group-tab:: IDL

        .. figure:: figures/Screenshots/IDL/complete_example/3.png
           :align: center

           Result of a PAHdb-fit to the 10-15 micron spectrum of NGC
           7023 showing the contribution from large and small PAHs.

    .. group-tab:: Python

        .. figure:: figures/Screenshots/Python/complete_example/3.png
           :align: center

           Result of a PAHdb-fit to the 10-15 micron spectrum of NGC
           7023 showing the contribution from large and small PAHs.


.. tabs::

    .. group-tab:: IDL

        .. figure:: figures/Screenshots/IDL/complete_example/4.png
           :align: center

           Result of a PAHdb-fit to the 10-15 micron spectrum of NGC
           7023 showing the contribution from PAH anions, neutrals and
           cation.

    .. group-tab:: Python

        .. figure:: figures/Screenshots/Python/complete_example/4.png
           :align: center

           Result of a PAHdb-fit to the 10-15 micron spectrum of NGC
           7023 showing the contribution from PAH anions, neutrals and
           cation.


.. tabs::

    .. group-tab:: IDL

        .. figure:: figures/Screenshots/IDL/complete_example/5.png
           :align: center

           Result of a PAHdb-fit to the 10-15 micron spectrum of NGC
           7023 showing the contribution from 'pure' and nitrogen
           containing PAHs (PANHs).


    .. group-tab:: Python

        .. figure:: figures/Screenshots/Python/complete_example/5.png
           :align: center

           Result of a PAHdb-fit to the 10-15 micron spectrum of NGC
           7023 showing the contribution from 'pure' and nitrogen
           containing PAHs (PANHs).


.. tabs::

    .. group-tab:: IDL

        .. figure:: figures/Screenshots/IDL/complete_example/6.png
           :align: center

           Predicted 4000-0 cm\ :sup:`-1` spectrum of NGC 7023 based
           on a PAHdb-fit to its 10-15 micron region showing the
           contribution from 'pure'.

    .. group-tab:: Python

        .. figure:: figures/Screenshots/Python/complete_example/6.png
           :align: center

           Result of a PAHdb-fit to the 10-15 micron spectrum of NGC
           7023 showing the contribution from 'pure' and nitrogen
           containing PAHs (PANHs).


