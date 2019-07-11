
Spectroscopic fitting
==========================================

Spectroscopic database fitting is handled by the
'AmesPAHdbIDLSuite_Spectrum'-object and it either accepts an
'AmesPAHdbIDLSuite_Observation'-object, or a simple array of ordinates
with an optional array of ordinate uncertainties. Whether ordinate
uncertainties are provided or not, the 'AmesPAHdbIDLSuite_Spectrum'-
object's 'Fit'-method will perform a non-negative least-chi-square or
non-negative least-square fit and return an
'AmesPAHdbIDLSuite_Fitted_Spectrum'-object.

.. tabs::

    .. code-tab:: idl

        fit = spectrum->Fit(intensity, uncertainty)

    .. code-tab:: python

        # Placeholder
