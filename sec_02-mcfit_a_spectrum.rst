

Monte Carlo fit a spectrum
=============================

A complete example of fitting an astronomical spectrum using a Monte Carlo
approach is shown below. It is an adaption of 'mc_fit_a_spectrum' found in the
examples-directory of the software tools, and is followed by a detailed,
line-by-line, explanation of the code.

.. tabs::

    .. code-tab:: idl
        :linenos:

        observation = OBJ_NEW('AmesPAHdbIDLSuite_Observation', myFile, $
                              Units=AmesPAHdbIDLSuite_CREATE_OBSERVATION_UNITS_S())

        observation->AbscissaUnitsTo,1

        pahdb = OBJ_NEW('AmesPAHdbIDLSuite')

        transitions = pahdb->getTransitionsByUID( $
                      pahdb->Search("magnesium=0 oxygen=0 iron=0 silicium=0 chx=0 ch2=0 c>20 h>0"))

        transitions->Cascade,8D*1.602D-12

        transitions->Shift,-15D

        spectrum = transitions->Convolve(Grid=observation->getGrid(), $
                                         FWHM=15D, $
                                         /Gaussian)

        OBJ_DESTROY,[transitions]

        mcfit = spectrum->MCFit(observation, 1024)

        OBJ_DESTROY,[spectrum]

        mcfit->Plot,/Wavelength

        mcfit->Plot,/Wavelength,/Size

        mcfit->Plot,/Wavelength,/Charge

        mcfit->Plot,/Wavelength,/Composition

        mcfit->Plot,/DistributionSize,NBins=10L,Min=20,Max=200

        OBJ_DESTROY,[mcfit, pahdb, observation]


    .. code-tab:: python
        :linenos:

        import importlib_resources

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

        transitions.cascade(8.0 * 1.603e-12, multiprocessing=False)

        transitions.shift(-15.0)

        spectrum = transitions.convolve(
            grid=obs.getgrid(), fwhm=15.0, gaussian=True, multiprocessing=False
        )

        mcfit = spectrum.mcfit(obs, samples=1024, multiprocessing=False)

        mcfit.plot(wavelength=True, residual=True)
        mcfit.plot(wavelength=True, size=True)
        mcfit.plot(wavelength=True, charge=True)
        mcfit.plot(wavelength=True, composition=True)


A line-by-line explanation of the code follows.

 .. tabs::

    .. group-tab:: IDL

        lines 1-2: An observation is read from 'myFile' and the
        'AmesPAHdbIDLSuite_CREATE_OBSERVATION_UNITS_S'-helper function
        is called to associate units.

        line 4: Observation abscissa units are converted to
        wavenumber.

        line 6: The default NASA Ames PAH IR Spectroscopic Database
        XML-file is loaded.

        lines 8-9: The fundamental vibrational transitions from a subset
        of PAHs are retrieved.

        line 10: A full Cascade emission model at 8 eV is applied.

        line 13: The fundamental vibrational transitions are
        redshifted 15 cm\ :sup:`-1`.

        lines 19-20: The fundamental vibrational transitions are convolved
        with Gaussian profiles having a full-width-at-half-maximum
        of 15 cm\ :sup:`-1` onto the observational grid.

        line 19: Cleanup of 'transitions'.

        line 21: The observation is fitted with the PAH emission
        spectra using a Monte Carlo approach.

        line 23: Cleanup of 'spectrum'.

        line 25-33: Display several aspects of the fit.

        line 35: Cleanup.


    .. group-tab:: Python

        lines 1-4: Importing the necessary modules.

	lines 6-8: A spectrum included with the suite is loaded.

        line 10: Observation abscissa units are converted to wavenumber.

        line 13: The cutdown version of the database XML-file included with the
        suite is loaded.

        line 15-16: The fundamental vibrational transitions are retrieved.

        line 22: A full Cascade emission model at 8 eV is applied.

        line 24: The fundamental vibrational transitions are redshifted 15 cm\
        :sup:`-1`.

        lines 22-24: The fundamental vibrational transitions are convolved with
        Gaussian profiles having a full-width-at-half-maximum of 15 cm\
        :sup:`-1` onto the observational grid.

        line 30 The observation is fitted with the PAH emission spectra using a
        Monte Carlo approach.

        line 32-35: Display several aspects of the fit.


Below some examples of the generated output.

.. tabs::

    .. group-tab:: IDL

        .. figure:: figures/Screenshots/IDL/mc_fit/1.png
           :align: center

	   Top: Result of a PAHdb-fit to the 5-15 micron spectrum of
           NGC 7023. Bottom: Residual of the fit.

    .. group-tab:: Python

        .. figure:: figures/Screenshots/Python/mc_fit/1.png
           :align: center

           Result of a PAHdb-fit to the 5-15 micron spectrum of NGC 7023.


.. tabs::

    .. group-tab:: IDL

        .. figure:: figures/Screenshots/IDL/mc_fit/2.png
           :align: center

           Result of a PAHdb-fit to the 5-15 micron spectrum of NGC 7023
           showing the contribution from large, medium, and small PAHs.

    .. group-tab:: Python

        .. figure:: figures/Screenshots/Python/mc_fit/2.png
           :align: center

           Result of a PAHdb-fit to the 5-15 micron spectrum of NGC 7023
           showing the contribution from large and small PAHs.


.. tabs::

    .. group-tab:: IDL

        .. figure:: figures/Screenshots/IDL/mc_fit/3.png
           :align: center

           Result of a PAHdb-fit to the 5-15 micron spectrum of NGC
           7023 showing the contribution from PAH anions, neutrals and
           cation.

    .. group-tab:: Python

        .. figure:: figures/Screenshots/Python/mc_fit/3.png
           :align: center

           Result of a PAHdb-fit to the 5-15 micron spectrum of NGC
           7023 showing the contribution from PAH anions, neutrals and
           cation.


.. tabs::

    .. group-tab:: IDL

        .. figure:: figures/Screenshots/IDL/mc_fit/4.png
           :align: center

           Result of a PAHdb-fit to the 5-15 micron spectrum of NGC
           7023 showing the contribution from 'pure' and nitrogen
           containing PAHs (PANHs).

    .. group-tab:: Python

        .. figure:: figures/Screenshots/Python/mc_fit/4.png
           :align: center

           Result of a PAHdb-fit to the 5-15 micron spectrum of NGC
           7023 showing the contribution from 'pure' and nitrogen
           containing PAHs (PANHs).


.. tabs::

    .. group-tab:: IDL

        .. figure:: figures/Screenshots/IDL/mc_fit/5.png
           :align: center

           Result of a PAHdb-fit to the 5-15 micron spectrum of NGC
           7023 showing the derived PAH size distribution.
