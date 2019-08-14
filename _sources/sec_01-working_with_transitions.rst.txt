
Working with transition data
============================================================

The 'transitions'-instance exposes the fundamental vibrational
transitions.

.. tabs::

    .. code-tab:: idl

        uids = pahdb->Search("c<=20 neutral n=2 neutral")

        transitions = pahdb->getTransitionsByUID(uids)

    .. code-tab:: python

        uids = pahdb.search("c<=20 neutral n=2 neutral")

        transitions = pahdb.gettransitionsbyuid(uids)

The 'transitions'-instance's 'Print'-method will print out the
associated fundamental vibrational transitions for each PAH species.

.. tabs::

    .. code-tab:: idl

        transitions->Print

    .. code-tab:: python

        print(transitions)

Optionally, the 'Str'-keyword can be given to the 'Print'-method,
which will return the associated fundamental vibrational transitions
for each PAH species as a single, concatenated string.

.. tabs::

    .. code-tab:: idl

        transitions->Print,Str=Str

    .. code-tab:: python

        str = transitions.print()

The 'transitions'-instance's 'Plot'-method will display the
fundamental vibrational transitions in a 'stick'-plot.  The
transitions of each PAH species will be presented in a different
color.

.. tabs::

    .. code-tab:: idl

        transitions->Plot

    .. code-tab:: python

        transitions.plot()

Optionally, the 'Wavelength', 'Stick', 'Oplot', 'Legend', and
'Color'-keywords can be given to the 'Plot'-method to control the
abscissa, stick representation, overplotting, legend and color,
respectively. In the IDL case additional keywords accepted by IDL’s
‘PLOT’-procedure can be passed via the keyword inheritance mechanism.

.. tabs::

    .. code-tab:: idl

        transitions->Plot,/Wavelength,Color=5,XRANGE=[2.5,15],/XSTYLE

    .. code-tab:: python

        transitions.plot(wWavelengt=True,color='blue')

The 'transitions'-instance's 'Write'-method will
write the fundamental vibrational transitions to file. The transitions
of each PAH species will be written to a IPAC table (.tbl). Optionally,
a filename can be provided.

.. tabs::

    .. code-tab:: idl

        transitions->Write,'myFile'

    .. code-tab:: python

        transitions.write('myFile')

