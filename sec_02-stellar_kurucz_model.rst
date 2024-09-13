

Stellar Kurucz Model
====================

A complete example of using a stellar Kurucz model to generate an emission
spectrum is shown below. It is an adaption of 'stellar_kurucz_model' found in
the examples-directory of the software tools, and is followed by a detailed,
line-by-line, explanation of the code.

.. tabs::

    .. code-tab:: idl
        :linenos:

        pahdb = OBJ_NEW('AmesPAHdbIDLSuite')

        uid = 18

        transitions = pahdb->getTransitionsByUID(uid)

        FTAB_EXT,'ckp00_17000.fits',[1,10],angstroms,flem,EXT=1

        e = !EXCEPT

        !EXCEPT = 0

        transitions->Cascade, $
           AMESPAHDBIDLSUITE_CREATE_KURUCZ_STELLARMODEL_S(angstroms, flem), $
           /Star, $
           /StellarModel, $
           /Convolved

        transitions->Shift,-15D

        spectrum = transitions->Convolve(FWHM=15D)

        spectrum->Plot

        OBJ_DESTROY,[spectrum, transitions, pahdb]

        !EXCEPT = e


    .. code-tab:: python
        :linenos:

        import importlib_resources
        import numpy as np

        from amespahdbpythonsuite.amespahdb import AmesPAHdb

        from astropy.io import fits


        file_path = importlib_resources.files("amespahdbpythonsuite")
        xml_file = file_path / "resources/pahdb-theoretical_cutdown.xml"
        pahdb = AmesPAHdb(
            filename=xml_file,
            check=False,
            cache=False,
        )

        uid = 18

        transitions = pahdb.gettransitionsbyuid(uid)

        fits_file = file_path / "resources/ckp00_17000.fits"
        with fits.open(fits_file) as hdulist:
            angstrom = hdulist[1].data["WAVELENGTH"]
            kurucz = {
                "frequency": 1e8 / np.flip(angstrom),
                "intensity": 1e-8
                * np.flip(hdulist[1].data["g40"] * angstrom**2)
                / (4 * np.pi),
            }

        transitions.cascade(
            kurucz,
            star=True,
            stellar_model=True,
            convolved=True,
            multiprocessing=False,
        )

        transitions.shift(-15.0)

        spectrum = transitions.convolve(fwhm=15.0, lorentzian=True, multiprocessing=False)

        spectrum.plot(show=True, legend=True)

A line-by-line explanation of the code follows.

 .. tabs::

    .. group-tab:: IDL

        line 1: The default NASA Ames PAH IR Spectroscopic Database
        XML-file is loaded.

        line 3: Selecting UID 18 for coronene.

        line 5: Retrieving the fundamental vibrational transitions for coronene.

        line 7: Use astrolib's FTAB\_EXT to load in the Kurucz model from FITS
        file.

        lines 9-11: Surpress under/overflow reporting.

        lines 13-17: A Cascade emission model is applied that takes the Kurucz
        model as input using the
        `AMESPAHDBIDLSUITE_CREATE_KURUCZ_STELLARMODEL_S` convenience function
        to convert to expected units and convolves it with the entire spectrum.

        line 19: The fundamental vibrational transitions are
        redshifted 15 cm\ :sup:`-1`.

        line 21: The fundamental vibrational transitions are convolved
        with Lorentzian profiles having a full-width-at-half-maximum
        of 15 cm\ :sup:`-1`.

        line 23: Display the resulting spectrum.

        line 25: Cleanup.

        line 27: Restore suppressing under/overflow reporting.


    .. group-tab:: Python

        lines 1-6: Importing the necessary modules.

        lines 9-15: The cutdown version of the database XML-file included with
        the suite is loaded.

        line 17: Selecting UID 18 for coronene.

        line 19: Retrieving the fundamental vibrational transitions for coronene.

        line 21-29: Use astropy to load the Kurucz model from FITS file and
        convert to expected units.

        lines 31-37: A Cascade emission model is applied that takes the Kurucz
        model as input and convolves it with the entire spectrum.

        line 39: The fundamental vibrational transitions are
        redshifted 15 cm\ :sup:`-1`.

        line 41: The fundamental vibrational transitions are convolved
        with Lorentzian profiles having a full-width-at-half-maximum
        of 15 cm\ :sup:`-1`.

        line 43: Display the resulting spectrum.


Below the generated output.

.. tabs::

    .. group-tab:: IDL

        .. figure:: figures/Screenshots/IDL/kurucz_example/1.png
           :align: center

           Result of using a stellar Kurucz model to compute the emission
           spectrum of coronene.

    .. group-tab:: Python

        .. figure:: figures/Screenshots/Python/kurucz_example/1.png
           :align: center

           Result of using a stellar Kurucz model to compute the emission
           spectrum of coronene.
