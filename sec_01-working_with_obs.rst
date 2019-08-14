
Working with observational data
====================================

Astronomical observations can be handled by the
'observation'-instance, which is able to read text, ISO-SWS, and
Spitzer-IRS-files. In the IDL case, a convenience routine is available
to manage units.

.. tabs::

    .. code-tab:: idl

        observation = OBJ_NEW('AmesPAHdbIDLSuite_Observation', $
                             'myObservationFile', $
                      Units=AmesPAHdbIDLSuite_CREATE_OBSERVATION_UNITS_S())

    .. code-tab:: python

        observation = observation('myObservationFile')

*NB* Text-files can have up to five columns, organized as follows:

Column 1: abscissa

Column 2: ordinate

Column 3: continuum

Column 4: uncertainty in ordinate

Column 5: uncertainty in abscissa

In addition, an 'observation'-intance can be created using
keyword-initializers.

.. tabs::

    .. code-tab:: idl

        observation = OBJ_NEW('AmesPAHdbIDLSuite_Observation', X=frequency, $
                                                               Y=intensity, $
                                                               ErrY=ystdev, $
                      Units=AmesPAHdbIDLSuite_CREATE_OBSERVATION_UNITS_S())

    .. code-tab:: python

        observation = observation(x=frequency, \
                                  y=intensity, \
                                  erry=ystdev)

The 'observation'-instance exposes the observation and provides the
'Plot', and 'Write'-methods for output. The 'Plot'- method will
display the observation and accepts the 'Oplot', and 'Color'-keywords
to control overplotting and color, respectively. Through IDL's keyword
inheritance mechanism additional keywords accepted by IDL's
'PLOT'-procedure can be passed.

.. tabs::

    .. code-tab:: idl

        observation->Plot,XRANGE=[2.5,15],/XSTYLE

    .. code-tab:: python

        observation.plot()

The 'Write'-method will write the observation to an IPAC table (.tbl)
file. Optionally, a filename can be provided.

.. tabs::

    .. code-tab:: idl

        observation->Write,'myFile'

    .. code-tab:: python

        observation.write('myFile')

The 'observation'-instance's 'Rebin', 'AbscissaUnitsTo',
and 'SetGridRange'-methods can rebin the observation onto a specified
grid or, with the 'Uniform'-Keyword set, onto a uniform created grid
with specified samplingconvert the units associated with the
abscissaand change the grid range, respectively.

.. tabs::

    .. code-tab:: idl

        observation->Rebin,myGrid

        observation->Rebin,5D,/Uniform

        observation->AbsciccaUnitsTo

        observation->SetGridRange,500,2000

    .. code-tab:: python

        observation.rebin(myGrid)

        observation.rebin(5.0, uniform=True)

        observation.AbsciccaUnitsTo

        observation.setgridrange(500,2000)

