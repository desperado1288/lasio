Usage
=====

Example IPython notebooks
~~~~~~~~~~~~~~~~~~~~~~~~~

Look at the `example IPython notebooks here <http://nbviewer.ipython.org/github/kinverarity1/lasio/tree/master/notebooks/>`__. More detailed examples are coming.

Opening LAS files
~~~~~~~~~~~~~~~~~

To open a LAS file from disk:

.. code:: python

    >>> import lasio
    >>> l = lasio.read("sample_big.las")

To open a LAS file from a URL:

.. code:: python

    >>> l = lasio.read("http://someplace.com/example.las")

Getting curve data
~~~~~~~~~~~~~~~~~~

.. code:: python

    >>> from pprint import pprint
    >>> pprint(l.curves)
    [Curve(mnemonic=u'DEPT', unit=u'M', value=u'', descr=u'1  DEPTH', data=array([ 1670.   ,  1669.875,  1669.75 , ...,  1669.75 ,  1670.   , 1669.875])),
     Curve(mnemonic=u'DT', unit=u'US/M', value=u'', descr=u'2  SONIC TRANSIT TIME', data=array([ 123.45,  123.45,  123.45, ...,  123.45,  123.45,  123.45])),
     Curve(mnemonic=u'RHOB', unit=u'K/M3', value=u'', descr=u'3  BULK DENSITY', data=array([ 2550.,  2550.,  2550., ...,  2550.,  2550.,  2550.])),
     Curve(mnemonic=u'NPHI', unit=u'V/V', value=u'', descr=u'4   NEUTRON POROSITY', data=array([ 0.45,  0.45,  0.45, ...,  0.45,  0.45,  0.45])),
     Curve(mnemonic=u'SFLU', unit=u'OHMM', value=u'', descr=u'5  RXO RESISTIVITY', data=array([ 123.45,  123.45,  123.45, ...,  123.45,  123.45,  123.45])),
     Curve(mnemonic=u'SFLA', unit=u'OHMM', value=u'', descr=u'6  SHALLOW RESISTIVITY', data=array([ 123.45,  123.45,  123.45, ...,  123.45,  123.45,  123.45])),
     Curve(mnemonic=u'ILM', unit=u'OHMM', value=u'', descr=u'7  MEDIUM RESISTIVITY', data=array([ 110.2,  110.2,  110.2, ...,  110.2,  110.2,  110.2])),
     Curve(mnemonic=u'ILD', unit=u'OHMM', value=u'', descr=u'8  DEEP RESISTIVITY', data=array([ 105.6,  105.6,  105.6, ...,  105.6,  105.6,  105.6]))]
    >>> for curve in l.curves:
    ...     print("%s\t[%s]\t%s\t%s" % (
    ...         curve.mnemonic, curve.unit, curve.value, curve.descr))
    ...
    DEPT    [M]             1  DEPTH
    DT      [US/M]          2  SONIC TRANSIT TIME
    RHOB    [K/M3]          3  BULK DENSITY
    NPHI    [V/V]           4   NEUTRON POROSITY
    SFLU    [OHMM]          5  RXO RESISTIVITY
    SFLA    [OHMM]          6  SHALLOW RESISTIVITY
    ILM     [OHMM]          7  MEDIUM RESISTIVITY
    ILD     [OHMM]          8  DEEP RESISTIVITY

And if all you want is the curve data itself it's accessible as items of the ``LASFile`` object:

.. code:: python

    >>> l["SFLU"]
    array([ 123.45,  123.45,  123.45, ...,  123.45,  123.45,  123.45])
    >>> l["DEPT"]
    array([ 1670.   ,  1669.875,  1669.75 , ...,  1669.75 ,  1670.   , 1669.875])

Reading header information
~~~~~~~~~~~~~~~~~~~~~~~~~~

Header information is parsed into simple ``HeaderItem`` objects, and stored in a dictionary for each section of the header:

.. code:: python

    >>> l = lasio.read("sample_big.las")
    >>> l.version
    {'VERS': HeaderItem(mnemonic=u'VERS', unit=u'', value=1.2, descr=u'CWLS LOG ASCII STANDARD -VERSION 1.2'),
     'WRAP': HeaderItem(mnemonic=u'WRAP', unit=u'', value=u'NO', descr=u'ONE LINE PER DEPTH STEP')}
    >>> l.well
    {'STRT': HeaderItem(mnemonic=u'STRT', unit=u'M', value=1670.0, descr=u''),
     'STOP': HeaderItem(mnemonic=u'STOP', unit=u'M', value=1660.0, descr=u''),
     'STEP': HeaderItem(mnemonic=u'STEP', unit=u'M', value=-0.125, descr=u''),
     'NULL': HeaderItem(mnemonic=u'NULL', unit=u'', value=-999.25, descr=u''),
     'COMP': HeaderItem(mnemonic=u'COMP', unit=u'', value=u'ANY OIL COMPANY LTD.', descr=u'COMPANY'),
     'WELL': HeaderItem(mnemonic=u'WELL', unit=u'', value=u'ANY ET AL OIL WELL #12', descr=u'WELL'),
     'FLD': HeaderItem(mnemonic=u'FLD', unit=u'', value=u'EDAM', descr=u'FIELD'),
     'LOC': HeaderItem(mnemonic=u'LOC', unit=u'', value=u'A9-16-49-20W3M', descr=u'LOCATION'),
     'PROV': HeaderItem(mnemonic=u'PROV', unit=u'', value=u'SASKATCHEWAN', descr=u'PROVINCE'),
     'SRVC': HeaderItem(mnemonic=u'SRVC', unit=u'', value=u'ANY LOGGING COMPANY LTD.', descr=u'SERVICE COMPANY'),
     'DATE': HeaderItem(mnemonic=u'DATE', unit=u'', value=u'25-DEC-1988', descr=u'LOG DATE'),
     'UWI': HeaderItem(mnemonic=u'UWI', unit=u'', value=u'100091604920W300', descr=u'UNIQUE WELL ID')}
    >>> l.params
    {'BHT': HeaderItem(mnemonic=u'BHT', unit=u'DEGC', value=35.5, descr=u'BOTTOM HOLE TEMPERATURE'),
     'BS': HeaderItem(mnemonic=u'BS', unit=u'MM', value=200.0, descr=u'BIT SIZE'),
     'FD': HeaderItem(mnemonic=u'FD', unit=u'K/M3', value=1000.0, descr=u'FLUID DENSITY'),
     'MATR': HeaderItem(mnemonic=u'MATR', unit=u'', value=0.0, descr=u'NEUTRON MATRIX(0=LIME,1=SAND,2=DOLO)'),
     'MDEN': HeaderItem(mnemonic=u'MDEN', unit=u'', value=2710.0, descr=u'LOGGING MATRIX DENSITY'),
     'RMF': HeaderItem(mnemonic=u'RMF', unit=u'OHMM', value=0.216, descr=u'MUD FILTRATE RESISTIVITY'),
     'DFD': HeaderItem(mnemonic=u'DFD', unit=u'K/M3', value=1525.0, descr=u'DRILL FLUID DENSITY')}

The ~Other section is stored as free text:

.. code:: python

    >>> l.other
    u'Note: The logging tools became stuck at 625 meters causing the data\nbetween 625 meters and 615 meters to be invalid.'

The actual values are stored as the ``value`` attribute:

.. code:: python

    >>> l.well["UWI"].value
    u'100091604920W300'
    >>> l.well["DATE"].value
    u'25-DEC-1988'
    >>> l.params["BHT"].value
    35.5

Creating a LAS file from scratch
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

First create an empty ``LASFile`` object:

.. code:: python

    >>> l = lasio.LASFile()
    >>> l.header
    {'~V': {'VERS': HeaderItem(mnemonic='VERS', unit='', value=2.0, descr='CWLS log ASCII Standard -VERSION 2.0'),
     'WRAP': HeaderItem(mnemonic='WRAP', unit='', value='NO', descr='One line per depth step'),
     'DLM': HeaderItem(mnemonic='DLM', unit='', value='SPACE', descr='Column Data Section Delimiter')},
     '~W': {'STRT': HeaderItem(mnemonic='STRT', unit='m', value=nan, descr='START DEPTH'),
     'STOP': HeaderItem(mnemonic='STOP', unit='m', value=nan, descr='STOP DEPTH'),
     'STEP': HeaderItem(mnemonic='STEP', unit='m', value=nan, descr='STEP'),
     'NULL': HeaderItem(mnemonic='NULL', unit='', value=-9999.25, descr='NULL VALUE'),
     'COMP': HeaderItem(mnemonic='COMP', unit='', value='', descr='COMPANY'),
     'WELL': HeaderItem(mnemonic='WELL', unit='', value='', descr='WELL'),
     'FLD': HeaderItem(mnemonic='FLD', unit='', value='', descr='FIELD'),
     'LOC': HeaderItem(mnemonic='LOC', unit='', value='', descr='LOCATION'),
     'PROV': HeaderItem(mnemonic='PROV', unit='', value='', descr='PROVINCE'),
     'CNTY': HeaderItem(mnemonic='CNTY', unit='', value='', descr='COUNTY'),
     'STAT': HeaderItem(mnemonic='STAT', unit='', value='', descr='STATE'),
     'CTRY': HeaderItem(mnemonic='CTRY', unit='', value='', descr='COUNTRY'),
     'SRVC': HeaderItem(mnemonic='SRVC', unit='', value='', descr='SERVICE COMPANY'),
     'DATE': HeaderItem(mnemonic='DATE', unit='', value='', descr='DATE'),
     'UWI': HeaderItem(mnemonic='UWI', unit='', value='', descr='UNIQUE WELL ID'),
     'API': HeaderItem(mnemonic='API', unit='', value='', descr='API NUMBER')},
     '~C': [],
     '~P': {},
     '~O': }

Then let's add some header items and curve data:

.. code:: python

    >>> import datetime
    >>> l.well["DATE"].value = str(datetime.datetime.today())
    >>> l.params["ENGI"] = lasio.HeaderItem("ENGI", "", "kinverarity@hotmail.com", "Creator of this file...")
    >>> l.other = "Example of creating a LAS file from scratch using lasio"
    >>> depth = [100, 100.5, 101, 101.5, 102]
    >>> data = [5, 6, 9, 7, -9999.25]
    >>> l.add_curve("DEPT", depth, unit="m")
    >>> l.add_curve("DAT", data, descr="Made-up curve data for example")

Finally write the result (in this case to the console):

.. code:: python

    >>> import sys
    >>> l.write(sys.stdout, version=2.0, fmt="%10.5g")
    ~Version ---------------------------------------------------
    VERS.       2.0 : CWLS log ASCII Standard -VERSION 2.0
    WRAP.        NO : One line per depth step
    DLM .     SPACE : Column Data Section Delimiter
    ~Well ------------------------------------------------------
    STRT.m                         100.0 : START DEPTH
    STOP.m                         102.0 : STOP DEPTH
    STEP.m                           0.5 : STEP
    NULL.                       -9999.25 : NULL VALUE
    COMP.                                : COMPANY
    WELL.                                : WELL
    FLD .                                : FIELD
    LOC .                                : LOCATION
    PROV.                                : PROVINCE
    CNTY.                                : COUNTY
    STAT.                                : STATE
    CTRY.                                : COUNTRY
    SRVC.                                : SERVICE COMPANY
    DATE.     2015-08-09 17:12:52.371000 : DATE
    UWI .                                : UNIQUE WELL ID
    API .                                : API NUMBER
    ~Curves ----------------------------------------------------
    DEPT.m      :
    DAT .       : Made-up curve data for example
    ~Params ----------------------------------------------------
    ENGI.     kinverarity@hotmail.com : Creator of this file...
    ~Other -----------------------------------------------------
    Example of creating a LAS file from scratch using lasio
    ~ASCII -----------------------------------------------------
            100          5
          100.5          6
            101          9
          101.5          7
            102    -9999.2

Character encodings
~~~~~~~~~~~~~~~~~~~

Three options:

1. Do nothing and hope for no `errors <https://docs.python.org/2.7/howto/unicode.html#encodings>`__.

2. Specify the encoding (internally ``lasio`` uses the ``open`` `function <https://docs.python.org/2/library/codecs.html#codecs.open>`__ from ``codecs`` which is part of the standard library):

.. code:: python

    >>> l = lasio.read("example.las", encoding="windows-1252")

3. Install a third-party package like `cChardet <https://github.com/PyYoshi/cChardet>`__ (faster) or `chardet <https://pypi.python.org/pypi/chardet>`__ (slower) to automatically detect the character encoding. If these packages are installed this code will use whichever is faster:

.. code:: python

    >>> l = lasio.read("example.las", autodetect_encoding=True)

Note that by default ``autodetect_encoding=False``.
