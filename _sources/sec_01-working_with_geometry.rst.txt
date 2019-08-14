
Working with geometry data
=============================

Molecular geometry data is exposed through an 'geometry'-instance.

.. tabs::

    .. code-tab:: idl

        uids = pahdb->Search("c<=20 neutral n=2 neutral")

        geometry = pahdb->getGeometryByUID(uids)

    .. code-tab:: python

        uids = pahdb.search("c<=20 neutral n=2 neutral")

        geometry = pahdb.getgeometrybyuid(uids)

The 'geometry'-instance provides both the 'Plot' and
'Structure'-methods to output the chemical structure of the PAH with
provided UID. The first method uses a 2D representation, while the
latter a 3D one.

.. tabs::

    .. code-tab:: idl

        ;; UID 18 = coronene (C24H12)

        void = geometry->Plot(18)

        img = geometry->Structure(18)

    .. code-tab:: python

        # UID 18 = coronene (C24H12)

        geometry.plot(18)

        img = geometry.structure(18)

The 'Plot'-method will accept the 'Scale', 'Thick', and 'RGB'- keywords
to control the scale at which the atoms are to be drawn, the thickness
of the bonds, and if colors should be outputted as decomposed RGB,
respectively. In addition, the 'NoErase' and 'Position'-keywords
control display erasing prior plotting and the plot position in
normalized coordinates, respectively. When the 'Resolution'-keyword
is set, the plot will be rendered to a Z-buffer device at given
resolution and outputted as an image and returned to the caller.
When the 'Save'-keyword is also set, a PNG-image will be generated
as well, which will have the UID of the PAH molecule under
consideration embedded in the filename. Lastly, the 'Angle'-keyword
controls rotation of the structure around the z-axis.

.. tabs::

    .. code-tab:: idl

        ;; UID 18 = coronene (C24H12)

        img = geometry->Plot(18, Scale=2.0, Thick=5.0, $
                         Resolution=[1200, 1200], /Save)

    .. code-tab:: python

        # UID 18 = coronene (C24H12)

        img = geometry.plot(18, scale=2.0, thick=5.0, \
                         resolution=(1200, 1200), save=True)

The 'Structure'-method will accept the 'Background' and
'Frame'-keywords to control background color and display of a
wireframe. The 'Resolution'-keyword controls the dimension of the
3D-rendered output image. The 'Save'-keyword can be specified to save
the generated image as a PNG-image as well, which will have the UID of
the PAH molecule under consideration embedded in the filename. If in
addition the 'Transparent'-keyword is specified as a color-triplet
array, that color will be set to transparent in the generated
PNG-file. The 'Axis' and 'Angle'-keywords can control axis and angle
of rotation, respectively. With the 'View'-keyword set the generated
structure will be rendered on screen. Lastly, when the 'Obj'-keyword
is set, the generated 3D model is returned to the caller.

.. tabs::

    .. code-tab:: idl

        ;; UID 18 = coronene (C24H12)

        img = geometry->Structure(18, Background=[0, 255, 0], $
                                      Resolution=[600, 600], $
                                      /Save, Transparent=[0, 255, 0], $
                                      /View)

    .. code-tab:: python

        img = geometry.structure(18, background=(0, 255, 0), \
                                     resolution=(600, 600), \
                                      save=True, transparent=(0, 255, 0), \
                                      view=True)

In addition, the 'geometry'-instance provides the 'Mass', 'Rings', and
'Area'-methods that return the calculated mass based on atomic masses,
the number of 3-8 membered rings, and total surface area of the PAH
molecules under consideration.

.. tabs::

    .. code-tab:: idl

        masses = geometry->Mass()

        rings = geometry->Rings()

        areas = geometry->Area()

    .. code-tab:: python

        masses = geometry.mass()

        rings = geometry.rings()

        areas = geometry.area()

Lastly, the 'Inertia'-method provides the moment of inertia matrices,
which are diagonalized with the 'Diagonalize'-method, for the PAH
molecules under consideration. The latter method can be particular
useful to ensure proper alignment of the structure with the view
before calling the 'Plot' or 'Structure'-methods.

.. tabs::

    .. code-tab:: idl

        matrix = geometry->Inertia()

        geometry->Diagonalize

    .. code-tab:: python

        matrix = geometry.inertia()

        geometry.diagonalize()

