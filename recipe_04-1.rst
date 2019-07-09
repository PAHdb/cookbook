
Emission models
===========================

.. tabs::

    .. code-tab:: idl

        ; The AmesPAHdbIDLSuite offers three PAH emission models. With increasing
        ; complexity they are the 'FixedTemperature', 'CalculatedTemperature',
        ; and 'Cascade' model. The first simply multiplies a blackbody at fixed
        ; given temperature with the integrated cross-section of each vibrational
        ; transition. The second first calculates the maximum attained temperature
        ; from the provided input and subsequently multiplies a blackbody at that
        ; fixed temperature with the integrated cross-section of each vibrational
        ; transition. The third averages the total emission over the entire
        ; cooling cascade (time).

        ; Emission models are handled by the 'AmesPAHdbIDLSuite_Transitions'-object.
        ; The 'FixedTemperature'-model simply takes a temperature, in Kelvin,
        ; and, in their simplest form, both the 'CalculatedTemperature' and
        ; 'Cascade' models take an energy, in erg.

        transitions->FixedTemperature,600D

        transitions->CalculatedTemperature,6D*1.603D-12; 6 eV

        transitions->Cascade,6D*1.603D-12; 6 eV

        ; Both the 'CalculatedTemperature' and 'Cascade'-methods accept the
        ; 'Approximate', 'Star', 'StellarModel', and 'ISRF'-keywords. With the
        ; 'Approximate'-keyword specified, calculations are performed using the
        ; PAH emission model from Bakes et al. (2001a, b). When the 'Star'-keyword
        ; is set, a stellar blackbody at the provided temperature is used to
        ; calculate the average energy absorbed by each PAH utilizing the PAH
        ; absorption cross-sections from Draine & Li (2007). In case the
        ; 'StellarModel'-keyword is provided as well, the input is considered
        ; to be a full-blown, for example, Kurucz stellar atmosphere model. The
        ; 'AmesPAHdbIDLSuite_CREATE_KURUCZ_STELLARMODEL_S' helper routine is
        ; provided to assist with molding the model data into the proper input
        ; format. Lastly, with the 'ISRF'-keyword set, the interstellar radiation
        ; field from Mathis et al. (1983) is used to calculate the average energy
        ; absorbed by each PAH.

        transitions->CalculatedTemperature,17D3,/Star

        transitions->Cascade,/Approximate,/ISRF

        FTAB_EXT,'ckp00_17000.fits',[1,10],angstroms,flam,EXT=1

        transitions->Cascade, $
                      AmesPAHdbIDLSuite_CREATE_KURUCZ_STELLARMODEL_S(angstroms, $
                                                                     flam), $
                      /Star, $
                      /StellarModel

        ; The 'Cascade'-method also accepts the 'Convolve'-keyword. When set and
        ; combined with either the 'Star', optionally with the 'StellarModel'-keyword,
        ; or 'ISRF'-keyword, will instead of calculating the average absorbed
        ; photon energy for each PAH, convolve the PAH emission with the entire
        ; radiation field.

        transitions->Cascade,17D3,/Star,/Convolve

        ; NB This is computationally expensive.

        ; The 'AmesPAHdbIDLSuite_Transitions'-object's 'Shift'-method can be used
        ; to redshift the fundamental transitions to simulate anharmonic effects.

        transitions->Shift,-15D

        ; NB Red-shifting the fundamental vibrational transitions should be done
        ; after applying one of the three emission models described above. 


    .. code-tab:: py

        # Interaction with the NASA Ames PAH IR Spectroscopic Database is organized around the 'Amespahdbpythonsuite'-object, which is created as shown below.
        
        from amespahdbpythonsuite.pahdb import Amespahdbpythonsuite
        pahdb = Amespahdbpythonsuite(cache=False, check=False)

