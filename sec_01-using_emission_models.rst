
Using emission models
===========================

The software tools offer three PAH emission models. With increasing
complexity they are the 'FixedTemperature', 'CalculatedTemperature',
and 'Cascade' model. The first simply multiplies a blackbody at fixed
given temperature with the integrated cross-section of each
vibrational transition. The second first calculates the maximum
attained temperature from the provided input and subsequently
multiplies a blackbody at that fixed temperature with the integrated
cross-section of each vibrational transition. The third averages the
total emission over the entire cooling cascade (time).

Emission models are handled by the 'transitions'-instance.  The
'FixedTemperature'-model simply takes a temperature, in Kelvin, and,
in their simplest form, both the 'CalculatedTemperature' and 'Cascade'
models take an energy, in erg.

.. tabs::

    .. code-tab:: idl

        transitions->FixedTemperature,600D

        transitions->CalculatedTemperature,6D*1.603D-12 ; 6 eV

        transitions->Cascade,6D*1.603D-12 ; 6 eV

    .. code-tab:: python

        transitions.fixed_temperature(600)

        transitions.calculatedtemperature(4.0 * 1.603e-12)

        transitions.cascade(6 * 1.603e-12)

Both the 'CalculatedTemperature' and 'Cascade'-methods accept the
'Approximate', 'Star', 'StellarModel', and 'ISRF'-keywords. With the
'Approximate'-keyword specified, calculations are performed using the
PAH emission model from Bakes et al. (2001a, b). When the
'Star'-keyword is set, a stellar blackbody at the provided temperature
is used to calculate the average energy absorbed by each PAH utilizing
the PAH absorption cross-sections from Draine & Li (2007). In case the
'StellarModel'-keyword is provided as well, the input is considered to
be a full-blown, for example, Kurucz stellar atmosphere model. In the
IDL case, the 'AmesPAHdbIDLSuite_CREATE_KURUCZ_STELLARMODEL_S' helper
routine is provided to assist with molding the model data into the
proper input format. Lastly, with the 'ISRF'-keyword set, the
interstellar radiation field from Mathis et al. (1983) is used to
calculate the average energy absorbed by each PAH.

.. tabs::

    .. code-tab:: idl

        transitions->CalculatedTemperature,17D3,/Star

        transitions->Cascade,/Approximate,/ISRF

        FTAB_EXT,'ckp00_17000.fits',[1,10],angstroms,flam,EXT=1

        transitions->Cascade, $
                      AmesPAHdbIDLSuite_CREATE_KURUCZ_STELLARMODEL_S(angstroms, $
                                                                     flam), $
                      /Star, $
                      /StellarModel

    .. code-tab:: python

        transitions.calculatedtemperature(17000, star=True)

        transitions.cascade(precision='approximate', field='ISRF')

The 'Cascade'-method also accepts the 'Convolve'-keyword. When set and
combined with either the 'Star', optionally with the 'StellarModel'-keyword,
or 'ISRF'-keyword, will instead of calculating the average absorbed
photon energy for each PAH, convolve the PAH emission with the entire
radiation field.

.. tabs::

    .. code-tab:: idl

        transitions->Cascade,17D3,/Star,/Convolve

    .. code-tab:: python

        transitions.cascade(17000,star=True,convolve=True)

*NB* This is computationally expensive.

The 'transitions'-instance's 'Shift'-method can be used to redshift
the fundamental transitions to simulate some anharmonic effects.

.. tabs::

    .. code-tab:: idl

        transitions->Shift,-15D

    .. code-tab:: python

        transitions.shift(-15)

*NB Red-shifting the fundamental vibrational transitions should be
done after applying one of the three emission models described above.*
