

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

	 import csv
         import amespahdbpythonsuite


         observation = []
         with open('myFile', 'rb') as f:
             reader = csv.reader(f, delimiter=' ', skipinitialspace=True)
             for col in zip(*reader):
                 observation.append([float(x) for x in col])

         observation[0] = [1e4/x for x in observation[0]]

         pahdb = amespahdbpythonsuite()

         transitions = pahdb.gettransitionsbyuid( \
                       pahdb.search("magnesium=0 oxygen=0 iron=0 silicium=0 chx=0 ch2=0 c>20"))

         transitions.fixedtemperature(600.0)

         transitions.shift(-15.0)

         spectrum = transitions.convolve(grid=observation[0], \
                                         fwhm=15.0,
                                         gaussian=True)

         intensity = \
	   [observation[1][i] - observation[2][i] for i in range(0, len(observation[0]))])

         fit = spectrum.fit(intensity)

         fit.plot(wavelength=True)

         fit.plot(wavelength=True, residual=True)

         fit.plot(wavelength=True, size=True)

         fit.plot(wavelength=True, charge=True)

         fit.plot(wavelength=True, composition=True)


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

        line 25-29: Display several aspects of the fit.

        line 31: The transitions are intersected with the PAH species
        in the fit.

        line 34: The fundamental vibrational transitions are again
        convolved with Lorentzian profiles having a
        full-width-at-half-maximum of 20\ :sup:`-1`, but now onto a
        generated grid from 3-10 micron.

        line 38: The individual PAH spectra are added using weights
        retrieved from the fit.

        line 40: The coadded spectrum is displayed, revealing the
        entire 3-20 micron, predicted, PAH spectrum.

        line 42: Cleanup.

        Line 25: A figure is displayed showing the fitted spectrum and
        its components, as shown below.

    .. group-tab:: Python

        lines 1-2: Importing the necessary modules.

	lines 5-9: Read astronomical observation from 'myFile' in
	CSV-format.

        line 11: Observation abscissa units are converted to
        wavenumber.

        line 13: The default NASA Ames PAH IR Spectroscopic Database
        XML-file is loaded.

        line 15-16: The fundamental vibrational transitions from a subset
        of PAHs are retrieved.

        line 18: A FixedTemperature emission model at 600 Kelvin is
        applied.

        line 20: The fundamental vibrational transitions are
        redshifted 15 cm\ :sup:`-1`.

        lines 22-24: The fundamental vibrational transitions are
        convolved with Lorentzian profiles having a
        full-width-at-half-maximum of 15 cm\ :sup:`-1` onto the
        observational grid.

	line 26-27: Subtract the continuum component.

        line 29 The observation is fitted with the PAH emission spectra.

        line 31-39: Display several aspects of the fit.


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
