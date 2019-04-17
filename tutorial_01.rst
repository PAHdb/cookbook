
Tutorial 01: a single Spitzer spectrum
======================================

We begin this series by performing a simple analysis of a single
astronomical spectrum. We will use one of the sample spectra here. Feel
free to follow along, and attempt the same method with a simple spectrum
of your own.

Data used in this example:
``pyPAHdb/data/sample_data_NGC7023-NW-PAHs.txt``.

These data are from: Boersma, C., Bregman, J. D., & Allamandola, L. J.
2013, ApJ, 769, 117 (http://adsabs.harvard.edu/abs/2013ApJ...769..117B)

Table of contents

1. `Import needed modules <#step1>`__
2. `Data validation <#step2>`__

   1. `Inspect the data <#step2a>`__
   2. `Make a quick plot <#step2b>`__

3. `Running pyPAHdb <#step3>`__

   1. `Instantiate an Observation object <#step3a>`__
   2. `Pass the spectrum to Decomposer <#step3b>`__
   3. `Write the results to disk <#step3c>`__

Step 1: Necessary modules 
--------------------------

.. code:: ipython3

    # The below command will suppress the shell output, since we are using
    # matplotlib within a notebook.
    %matplotlib inline
    
    import os
    import pkg_resources
    
    import matplotlib.pyplot as plt
    import numpy as np
    import pandas as pd
    
    from pypahdb.decomposer import Decomposer
    from pypahdb.observation import Observation

--------------

Step 2: Data validation 
------------------------

You should ensure your data has a simple format. Acceptable formats
include:

-  FITS

   -  ...

-  ASCII

   -  either two-column (wavelength, flux) or three-column (wavelength,
      flux, flux error)
   -  seperated by commas (CSV) or single spaces

A. Inspect the data 
~~~~~~~~~~~~~~~~~~~~

We will use the example spectrum ``sample_data_NGC7023-NW-PAHs.txt``,
included in the pypahdb distribution.

.. code:: ipython3

    # Loading from the data directory. For your uses, point to the location
    # of the spectrum you are examining.
    # data_file = data_dir + 'NGC7023-NW-PAHs.txt'
    data_file = pkg_resources.resource_filename('pypahdb', 'data/sample_data_NGC7023-NW-PAHs.txt')

Let's examine the first few lines of this file so we understand its
structure...

.. code:: ipython3

    for index, line in enumerate(open(data_file, 'r')):
        print(line, end='')
        if index >= 3:
            break


.. parsed-literal::

    wavelength "surface brightness"
    5.24282 50.918551188790325
    5.2738 53.081826767577795
    5.30479 55.46148169840347


We will use ``pandas`` to load the data for quick analysis:

.. code:: ipython3

    df = pd.read_csv(data_file, sep=' ')  # Loads the data into a pandas DataFrame.

.. code:: ipython3

    df.head()




.. raw:: html

    <div>
    <style scoped>
        .dataframe tbody tr th:only-of-type {
            vertical-align: middle;
        }
    
        .dataframe tbody tr th {
            vertical-align: top;
        }
    
        .dataframe thead th {
            text-align: right;
        }
    </style>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>wavelength</th>
          <th>surface brightness</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>0</th>
          <td>5.24282</td>
          <td>50.918551</td>
        </tr>
        <tr>
          <th>1</th>
          <td>5.27380</td>
          <td>53.081827</td>
        </tr>
        <tr>
          <th>2</th>
          <td>5.30479</td>
          <td>55.461482</td>
        </tr>
        <tr>
          <th>3</th>
          <td>5.33577</td>
          <td>58.099673</td>
        </tr>
        <tr>
          <th>4</th>
          <td>5.36676</td>
          <td>61.056485</td>
        </tr>
      </tbody>
    </table>
    </div>



B. Make a quick plot 
~~~~~~~~~~~~~~~~~~~~~

Let's make a quick plot to make sure the spectrum has no unusual
features/artifacts.

.. code:: ipython3

    plt.plot(df['wavelength'], df['surface brightness'])
    plt.xlabel('Wavelength (μm)')
    plt.ylabel('Surface brightness (MJy/sr)')



.. image:: tutorial_01_files/tutorial_01_19_0.png


We see that it is a reasonably smooth spectrum composed of Spitzer/IRS
observations using the SL module (SL1 and SL2, covering ~5-14 microns
approximately).

The data needs to be monotonic, i.e. not double-valued or out of order
(as determined by the wavelength array).

.. code:: ipython3

    def strictly_increasing(L):
        return all(x < y for x, y in zip(L, L[1:]))
    
    strictly_increasing(df['wavelength'])




.. parsed-literal::

    True



--------------

Step 3: Running pyPAHdb 
------------------------

A. Instantiate an ``Observation`` object 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

All that's needed is the path to the text file above.

.. code:: ipython3

    data_file




.. parsed-literal::

    '/Users/koma/Documents/GitHub/pyPAHdb/pypahdb/data/sample_data_NGC7023-NW-PAHs.txt'



.. code:: ipython3

    obs = Observation(data_file)

.. code:: ipython3

    obs.file_path




.. parsed-literal::

    '/Users/koma/Documents/GitHub/pyPAHdb/pypahdb/data/sample_data_NGC7023-NW-PAHs.txt'



Now we have an ``Observation`` object that encapsulates our data.

B. Pass the spectrum to ``Decomposer`` 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Now with our ``Observation`` instance, we simply pass its spectrum to
the pyPAHdb ``Decomposer``, which will perform the decomposition by PAH.

.. code:: ipython3

    pahdb_fit = Decomposer(obs.spectrum)

Now we have a ``Decomposer`` object that encapsulates the fit.

C. Write the results to disk 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The ``Decomposer`` class includes methods for saving the fit results to
disk:

.. code:: ipython3

    # write results to file
    pahdb_fit.save_pdf(filename='NGC7023_pypahdb.pdf')
    pahdb_fit.save_fits(filename='NGC7023_pypahdb.fits')


.. parsed-literal::

    Saved:  NGC7023_pypahdb.pdf
    Saved:  NGC7023_pypahdb.fits



.. parsed-literal::

    <Figure size 432x288 with 0 Axes>

