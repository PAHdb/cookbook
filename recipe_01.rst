
############
Tab options
############

Using ``sphinx-tabs`` [#f1]_ : 

.. tabs::

   .. code-tab:: c

         C Main Function

   .. code-tab:: c++

         C++ Main Function

   .. code-tab:: py

         import numpy
         print(np.sqrt(42))

Using ``sphinxcontrib.contentui`` :

.. content-tabs::

    .. tab-container:: tab1
        :title: Tab title one

        .. code-block:: python

            import matplotlib.pyplot as plt
            from pypahdb.observation import Observation

            filename = 'sample_data_NGC7023.tbl'
            obs = Observation(filename)
            s = obs.spectrum
            plt.plot(s.spectral_axis, s.flux,0,0,:])
            plt.show()

    .. tab-container:: tab2
        :title: Tab title two

        .. code-block:: python

            import numpy as np

            # Some other code.
            print(np.sqrt(42))


.. rubric:: Footnotes

.. [#f1] N.B. seems designed for different languages, might be able to hack it for our uses if desired though.



