
Working with fitted data
===========================

The 'AmesPAHdbIDLSuite_Fitted_Spectrum'-object exposes the fit and
provides the 'Plot', and 'Write'-methods for output. The 'Plot'-method
accepts the 'Residual', 'Size', 'Charge', and 'Composition'-keywords,
which selectively display the residual of the fit, or either the size,
charge and compositional breakdown. Without these keywords the fit
itself is displayed.

.. tabs::

    .. code-tab:: idl

        fit->Plot,/Charge

    .. code-tab:: python

        # Placeholder

Optionally, the 'Wavelength', 'Stick', 'Oplot', 'Legend', and 'Color'-
keywords can be given to the 'Plot'-method to control the abscissa,
stick representation, overplotting, legend and color, respectively.
Through IDL's keyword inheritance mechanism additional keywords
accepted by IDL's 'PLOT'-procedure can be passed.

.. tabs::

    .. code-tab:: idl

        fit->Plot,/Size,/Wavelength,XTITLE=[2.5,15],/XSTYLE

    .. code-tab:: python

        # Placeholder

The 'AmesPAHdbIDLSuite_Fitted_Spectrum'-object's 'Write'-method will
write the fit to a single text (.txt) file. Optionally, a prefix can
be given that will be prepended to the filename.

.. tabs::

    .. code-tab:: idl

        fit->Write,'myPrefix'

    .. code-tab:: python

        # Placeholder

The 'AmesPAHdbIDLSuite_Fitted_Spectrum'-object's 'GetClasses', and
'GetBreakdown'-methods return the fit broken down by charge, size,
and composition, where the first provides the spectrum for each
component and the latter its relative contribution.

.. tabs::

    .. code-tab:: idl

        classes = fit->getClasses()

        breakdown = fit->getBreakdown()

    .. code-tab:: python

        # Placeholder

Optionally the 'Small' keyword can be set, which controls the small
cutoff size in number of carbon atoms.

.. tabs::

    .. code-tab:: idl

        classes = fit->getClasses(Small=20L)

    .. code-tab:: python

        # Placeholder

The 'GetBreakdown'-method also accepts the 'Flux'-keyword, which
controls whether the relative breakdown should be reported based on
fitted weight or integrated flux.

.. tabs::

    .. code-tab:: idl

        breakdown = fit->getBreakdown(/Flux)

    .. code-tab:: python

        # Placeholder