
Working outside the classes
==============================

The software classes all provide the 'Get'-method that allows
extraction of an instance's internal data.

.. tabs::

    .. code-tab:: idl

        transitions_s = transitions->Get()

    .. code-tab:: python

        transitions_dict = transitions->get()

This allows, for example, more control over the presentation of the data.

.. tabs::

    .. code-tab:: idl

        PLOT,transitions_s.data.frequency,1D5*transitions_s.data.intensity, $
             XTITLE=transitions_s.units.x.str, $
             YTITLE='integrated intensity [cm!U-2!N mol!U-1!N]', $
             PSYM=10

    .. code-tab:: python

        import matplotlib.pyplot as plt

	plt.plot(transitions_dict['data']['frequency'], transitions_dict['data']['intensity'])
	plt.xlabel(transitions_dict['units']['x']['str'])
        plt.ylabel('integrated intensity [cm!U-2!N mol!U-1!N]')
        plt.show()


Subsequently these data can be manipulated and even be set to the
instancer, which will try altering its internal state to reflect that
of the manipulated data.

.. tabs::

    .. code-tab:: idl

        transitions->Set,transitions_s

    .. code-tab:: python

         transitions.set(transitions_dict)

In addition, the 'Set'-method accepts keywords to alter its internal
state.

.. tabs::

    .. code-tab:: idl

        transitions->Set,Data=transitions_s.data

    .. code-tab:: python

        transitions.set(data=transitions_dict['data'])

Lastly, it is also possible to create new intances initialized with an
appropriate data representation or through keywords.

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

        transitions = transitions(transitions_dict)

        transitions = trantions(type=transitions_dict['type'], \
                                version=transitions_dict['version'], \
                                pahdb=pahdb, \
                                data=transitions_dict['data'], \
                                uids=transitions_dict['uids'], \
                                model=transitions_dict['model'], \
                                units=transitions_dict['units'])

