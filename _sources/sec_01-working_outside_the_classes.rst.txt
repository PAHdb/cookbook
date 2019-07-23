
Working outside the classes
==============================

The AmesPAHdbIDLSuite object classes all provide the 'Get'-method that allows extraction of the object's internal data into an anonymous IDL struct.

.. tabs::

    .. code-tab:: idl

        transitions_s = transitions->Get()

    .. code-tab:: python

        # Placeholder

This allows, for example, more control over the presentation of the data.

.. tabs::

    .. code-tab:: idl

        PLOT,transitions_s.data.frequency,1D5*transitions_s.data.intensity, $
             XTITLE=transitions_s.units.x.str, $
             YTITLE='integrated intensity [cm!U-2!N mol!U-1!N]', $
             PSYM=10

    .. code-tab:: python

        # Placeholder

Subsequently this anonymous structure can be manipulated and even be set to the object, which will try altering its internal state to reflect that of the manipulated data.

.. tabs::

    .. code-tab:: idl

        transitions->Set,transitions_s

    .. code-tab:: python

        # Placeholder

In addition, the 'Set'-method accepts keywords to alter its internal state.

.. tabs::

    .. code-tab:: idl

        transitions->Set,Data=transitions_s.data

    .. code-tab:: python

        # Placeholder

Lastly, it is also possible to create new AmesPAHdbIDLSuite-objects initialized with an appropriate anonymous structure or through keywords.

.. tabs::

    .. code-tab:: idl

        transitions = OBJ_NEW('AmesPAHdbIDLSuite_Transitions', transitions_s)

        transitions = OBJ_NEW('AmesPAHdbIDLSuite_Transitions', $
                               Type=transitions_s.type, $
                               Version=transitions_s.version, $
                               PAHdb=pahdb->Pointer(), $
                               Data=transitions_s.data, $
                               Uids=transitions_s.uids, $
                               Model=transitions_s.model, $
                               Units=transitions_s.units)

    .. code-tab:: python

        # Placeholder
