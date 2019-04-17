
##############
Tab usage
##############

Can use tabs to embed entire RST files, or code blocks, or code chunks. Using ``sphinxcontrib.contentui`` tabs here for example.

N.B. If you use `content tabs` and ``.. include::`` an RST file in one of those tabs, you can't use embedded `content tabs` in that RST file. It explodes.


Full code blocks
==================

.. content-tabs::

    .. tab-container:: tab1
        :title: pyPAHdb

        .. code-block:: python

            import pkg_resources

            from pypahdb.decomposer import Decomposer
            from pypahdb.observation import Observation

            # A provided sample data file (in FITS format).
            file_path = 'data/sample_data_NGC7023.fits'
            data_file = pkg_resources.resource_filename('pypahdb', file_path)

            # Construct an Observation object.
            obs = Observation(data_file)

            # Pass the Observation's spectrum to Decomposer, which performs the fit.
            pahdb_fit = Decomposer(obs.spectrum)

            # Write the fit to file.
            pahdb_fit.save_pdf('NGC7023_pypahdb.pdf')
            pahdb_fit.save_fits('NGC7023_pypahdb.fits', header=obs.header)

    .. tab-container:: tab2
        :title: PAHdbPythonSuite

        .. code-block:: python

            # Primary class
            from amespahdbpythonsuite.pahdb import Amespahdbpythonsuite
            
            # Instantiate a PAHdb object, which parses the XML file.
            pahdb = Amespahdbpythonsuite(cache=False, check=False)

            # Get the integrated cross-sections for coronene
            transitions = pahdb.gettransitionsbyuid([18])

            # Plot the 'stick' spectrum
            transitions.plot()

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


Step-by-step inline
====================

We begin this series by performing a simple analysis of a single
astronomical spectrum. We will use one of the sample spectra here. Feel
free to follow along, and attempt the same method with a simple spectrum
of your own.

Data used in this example:
``pyPAHdb/data/sample_data_NGC7023-NW-PAHs.txt``.

These data are from: Boersma, C., Bregman, J. D., & Allamandola, L. J.
2013, ApJ, 769, 117 (http://adsabs.harvard.edu/abs/2013ApJ...769..117B)

First import the needed packages.

.. content-tabs::

    .. tab-container:: tab1
        :title: pyPAHdb

        .. code-block:: python

            import pkg_resources

            from pypahdb.decomposer import Decomposer
            from pypahdb.observation import Observation

    .. tab-container:: tab2
        :title: PAHdbPythonSuite

        .. code-block:: python

            # Primary class
            from amespahdbpythonsuite.pahdb import Amespahdbpythonsuite


Then do something else.

.. content-tabs::

    .. tab-container:: tab1
        :title: pyPAHdb

        .. code-block:: python

            dir(Decomposer)

    .. tab-container:: tab2
        :title: PAHdbPythonSuite

        .. code-block:: python

            dir(Amespahdbpythonsuite)


Then maybe another thing.

.. content-tabs::

    .. tab-container:: tab1
        :title: pyPAHdb

        .. code-block:: python

            import pkg_resources

            from pypahdb.decomposer import Decomposer
            from pypahdb.observation import Observation

    .. tab-container:: tab2
        :title: PAHdbPythonSuite

        .. code-block:: python

            # Primary class
            from amespahdbpythonsuite.pahdb import Amespahdbpythonsuite
