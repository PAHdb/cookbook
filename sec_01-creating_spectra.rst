
Creating spectra
===========================

Line profiles are also handled by the 'transitions'-intance and it
provides three profilesLorentzian, Gaussian and Drude. Convolution
with the keyword chosen line profile is achieved through the
'transitions'-instance's 'Convolve'-method, which will return the
convolved spectrum in the form of a 'spectrum'-instance.

.. tabs::

    .. code-tab:: idl

        spectrum = transitions->Convolve(/Drude)

    .. code-tab:: python

        spectrum = transitions.convolve(drude=True)

Optionally, the 'Convolve'-method accepts the 'FWHM', 'Grid',
'NPoints', and 'XRange'-keywords, which control the
full-width-at-half-maximum of the selected line profile (in cm\
:sup:`-1`), convolution onto a specified grid, the number of
resolution elements in the generated spectrum, and the frequency range
(in cm\ :sup:`-1`) of the spectrum.

.. tabs::

    .. code-tab:: idl

        spectrum = transitions->Convolve(/Drude, FWHM=20D, Grid=myGrid)

    .. code-tab:: python

        spectrum = transitions.convolve(drude=True, fwhm=20.0, grid=myGrid)
