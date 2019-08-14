
Spectroscopic fitting
==========================================

Spectroscopic database fitting is handled by the 'spectrum'-instance
and it either accepts an 'observation'-instance, or simply an array of
ordinates with an optional array of ordinate uncertainties. Whether
ordinate uncertainties are provided or not, the 'spectrum'- instance's
'Fit'-method will perform a non-negative least-chi-square or
non-negative least-square fit and return an
'fit'-instance.

.. tabs::

    .. code-tab:: idl

        ;; Using an 'observation'-instance

        fit = spectrum->Fit(observation)

	;; Using an array of ordinate values

        fit = spectrum->Fit(intensity)

	;; Using an array of ordinate uncertainty values

        fit = spectrum->Fit(intensity, uncertainty)

    .. code-tab:: python

        # Using an 'observation'-instance

        fit = spectrum.fit(observation)

	# Using an array of ordinate values

        fit = spectrum.fit(intensity)

	# Using an array of ordinate uncertainty values

        fit = spectrum.fit(intensity, uncertainty)

