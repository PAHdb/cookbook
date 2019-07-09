
Database fitting
===========================

.. tabs::

    .. code-tab:: idl

        ; Spectroscopic database fitting is handled by the
        ; 'AmesPAHdbIDLSuite_Spectrum'-object and it either accepts an
        ; 'AmesPAHdbIDLSuite_Observation'-object, or a simple array of ordinates
        ; with an optional array of ordinate uncertainties. Whether ordinate
        ; uncertainties are provided or not, the 'AmesPAHdbIDLSuite_Spectrum'-
        ; object's 'Fit'-method will perform a non-negative least-chi-square or
        ; non-negative least-square fit and return an
        ; 'AmesPAHdbIDLSuite_Fitted_Spectrum'-object.

        fit = spectrum->Fit(intensity, uncertainty)

        ; The 'AmesPAHdbIDLSuite_Fitted_Spectrum'-object exposes the fit and
        ; provides the 'Plot', and 'Write'-methods for output. The 'Plot'-method
        ; accepts the 'Residual', 'Size', 'Charge', and 'Composition'-keywords,
        ; which selectively display the residual of the fit, or either the size,
        ; charge and compositional breakdown. Without these keywords the fit
        ; itself is displayed.

        fit->Plot,/Charge

        ; Optionally, the 'Wavelength', 'Stick', 'Oplot', 'Legend', and 'Color'-
        ; keywords can be given to the 'Plot'-method to control the abscissa,
        ; stick representation, overplotting, legend and color, respectively.
        ; Through IDL's keyword inheritance mechanism additional keywords
        ; accepted by IDL's 'PLOT'-procedure can be passed.

        fit->Plot,/Size,/Wavelength,XTITLE=[2.5,15],/XSTYLE

        ; The 'AmesPAHdbIDLSuite_Fitted_Spectrum'-object's 'Write'-method will
        ; write the fit to a single text (.txt) file. Optionally, a prefix can
        ; be given that will be prepended to the filename.

        fit->Write,'myPrefix'

        ; The 'AmesPAHdbIDLSuite_Fitted_Spectrum'-object's 'GetClasses', and
        ; 'GetBreakdown'-methods return the fit broken down by charge, size,
        ; and composition, where the first provides the spectrum for each
        ; component and the latter its relative contribution.

        classes = fit->getClasses()

        breakdown = fit->getBreakdown()

        ; Optionally the 'Small' keyword can be set, which controls the small
        ; cutoff size in number of carbon atoms.

        classes = fit->getClasses(Small=20L)

        ; The 'GetBreakdown'-method also accepts the 'Flux'-keyword, which
        ; controls whether the relative breakdown should be reported based on
        ; fitted weight or integrated flux.

        breakdown = fit->getBreakdown(/Flux)


    .. code-tab:: py

        # Interaction with the NASA Ames PAH IR Spectroscopic Database is organized around the 'Amespahdbpythonsuite'-object, which is created as shown below.
        
        from amespahdbpythonsuite.pahdb import Amespahdbpythonsuite
        pahdb = Amespahdbpythonsuite(cache=False, check=False)

