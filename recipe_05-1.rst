
Dealing with astronomical observations
==========================================

.. tabs::

    .. code-tab:: idl

        ; Astronomical observations can be handled by the
        ; 'AmesPAHdbIDLSuite_Observation'-object, which is able to read text,
        ; ISO-SWS, and Spitzer-IRS-files. A convenience routine is available to
        ; manage units; AmesPAHdbIDLSuite_CREATE_OBSERVATION_UNITS_S.

        observation = OBJ_NEW('AmesPAHdbIDLSuite_Observation', $
                             'myObservationFile', $
                      Units=AmesPAHdbIDLSuite_CREATE_OBSERVATION_UNITS_S())

        ; In addition, an 'AmesPAHdbIDLSuite_Observation'-object can be created
        ; using keyword-initializers.

        observation = OBJ_NEW('AmesPAHdbIDLSuite_Observation', X=frequency, $
                                                               Y=intensity, $
                                                               ErrY=ystdev, $
                      Units=AmesPAHdbIDLSuite_CREATE_OBSERVATION_UNITS_S())

        ; The 'AmesPAHdbIDLSuite_Observation'-object exposes the observation
        ; and provides the 'Plot', and 'Write'-methods for output. The 'Plot'-
        ; method will display the observation and accepts the 'Oplot', and
        ; 'Color'-keywords to control overplotting and color, respectively.
        ; Through IDL's keyword inheritance mechanism additional keywords
        ; accepted by IDL's 'PLOT'-procedure can be passed.

        observation->Plot,XRANGE=[2.5,15],/XSTYLE

        ; The 'Write'-method will write the observation to a single text (.txt)
        ; file. Optionally, a prefix can be given that will be prepended to the
        ; filename.

        observation->Write,'myPrefix'

        ; NB Text-files can have up to five columns, organized as follows:
        ; Column 1: abscissa
        ; Column 2: ordinate
        ; Column 3: continuum
        ; Column 4: uncertainty in ordinate
        ; Column 5: uncertainty in abscissa

        ; The 'AmesPAHdbIDLSuite_Observation'-object's 'Rebin', 'AbscissaUnitsTo',
        ; and 'SetGridRange'-methods can rebin the observation onto a specified
        ; grid or, with the 'Uniform'-Keyword set, onto a uniform created grid
        ; with specified sampling; convert the units associated with the
        ; abscissa; and change the grid range, respectively.

        observation->Rebin,myGrid

        observation->Rebin,5D,/Uniform

        observation->AbsciccaUnitsTo

        observation->SetGridRange,500,2000


    .. code-tab:: py

        # Interaction with the NASA Ames PAH IR Spectroscopic Database is organized around the 'Amespahdbpythonsuite'-object, which is created as shown below.
        
        from amespahdbpythonsuite.pahdb import Amespahdbpythonsuite
        pahdb = Amespahdbpythonsuite(cache=False, check=False)

