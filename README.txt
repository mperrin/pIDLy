pIDLy: IDL within Python
Control ITT's IDL (Interactive Data Language) from within Python.

Version 0.2.4+

Copyright (c) 2008-2012, Anthony Smith
A.J.Smith 'at' sussex.ac.uk


CONTENTS
========

1. Requirements
2. Installation
3. Usage
4. Further information
5. Known bugs/issues
6. To do
7. Release history


1. REQUIREMENTS
===============

The following are required, but should be installed automatically if you
follow the installation instructions below:
* Pexpect: http://pexpect.sourceforge.net/
* NumPy: http://numpy.scipy.org/

Also required:
* IDL: http://www.ittvis.com/idl/

This version tested on 
* Mac OS X with IDL 7.1.1 and Python 2.7.2
* Linux with IDL 8.0 and Python 2.6.2


2. INSTALLATION
===============

1. Type "easy_install pidly"

If that fails:

1. Download http://peak.telecommunity.com/dist/ez_setup.py and run it
(type "python ez_setup.py")
2. Type "easy_install pidly"


3. USAGE
========

Initiate:
>>> import pidly
>>> idl = pidly.IDL()

Or:
    idl = pidly.IDL('/path/to/idl')

Execute commands:
>>> idl('x = total([1, 1], /int)')

Retrieve values:
>>> print idl.x
2

Or (slightly faster):
>>> print idl.ev('x')
2

Evaluate expressions:
>>> print idl.ev('x ^ 2')
4

Assign value from Python expression:
>>> idl.x = 2 + 2
>>> print idl.x
4

Or:
>>> idl('x', 2 + 2)
>>> print idl.x
4

Perform IDL function on Python expression(s):
>>> idl.reform(range(4), 2, 2)
array([[0, 1],
       [2, 3]])

Or (slightly faster):
>>> idl.func('reform', range(4), 2, 2)
array([[0, 1],
       [2, 3]])

With keywords (/L64 -> L64=True or L64=1)
>>> idl.histogram(range(4), binsize=3, L64=True)
array([3, 1], dtype=int64)

IDL procedure with Python argument(s):
>>> idl.pro('plot', range(10), range(10), xstyle=True, ystyle=True)

Interactive mode:
>>  idl.interact()
IDL> print, x
     4
IDL> ^D
>>>

Close:
>>> idl.close()

pIDLy supports the transfer of:
* ints, longs, ...
* floats, doubles, ...
* strings
* arrays of the above types, with arbitrary size and shape
* dictionaries <-> structures & lists of dicts <-> arrays of structures
  but with certain limitations on transfer from Python to IDL

[NB if getting Syntax Errors when passing large arrays to IDL, try using
>>  idl = pidly.IDL(long_delay=0.05)
default is 0.02.]


4. FURTHER INFORMATION
======================

Available on the pIDLy web page:
  http://astronomy.sussex.ac.uk/~anthonys/pidly/
or from the Python Package Index:
  http://pypi.python.org/pypi/pIDLy/
or from the author:
  A.J.Smith 'at' sussex.ac.uk


5. KNOWN BUGS/ISSUES
====================

* Python variables cannot be used as "output" parameters for IDL procedures
  and functions; use idl('my_procedure, output_parameter') to run the procedure
  then idl.output_parameter to retrieve the output.
* If Python is force-killed when IDL is running, IDL will persist and run wild
* Restrictive limits on size of Python dictionaries to send to IDL structures
* Slow transferring large Python arrays to IDL, e.g., 20,000 doubles in 12-15s
* IPython on Aquamacs: prints input in interactive mode
* Aquamacs: interactive mode has very small input buffer (253 bytes?)
* idl.f(..., idl.g(...)) doesn't work (pidly_tmp conflict)


6. TO DO
========

* Test on Windows
* Complex numbers
* Raise exceptions (e.g., for unsupported types)
* Passing special characters in strings (\t, \n etc)
* Make compatible with GDL


7. RELEASE HISTORY
==================

Version 0.2.4, 22 Feb 2008
* Fixed bug with keyword arguments in functions
* Added pro() method for IDL procedures with Python arguments

Version 0.2.3, 18 Feb 2008
* Improved garbage collection (using weakref and atexit)
* IDL Errors: launches interactive after '% Stop' or '% Execution Halted'
* If IDL pauses (waiting for input?), KeyboardInterrupt -> interactive mode
* Fixed bugs with NumPy array input
* Fixed problems with double precision float transfer
* Fixed problem with spaces in strings in structures/dictionaries
* Added test() function for full tests
* Added NaN and Inf support

Version 0.2.2, 9 Feb 2008
* Fixed bug, where IDL would run wild when IPython closed

Version 0.2.1, 8 Feb 2008
* Added keyword parameters in calls to IDL functions
* Added support for Python bool type

Version 0.2, 7 Feb 2008
* Structures can be transferred from IDL to Python as dictionaries
* Dictionaries can be transferred from Python to IDL as structures. But:
  - lists of dictionaries must be explicitly and consistently typed
  - the dictionary, or each dictionary in the list, must be short enough
    to fit into a single command for IDL
  - long lists of dictionaries are likely to be slow from Python to IDL, 
    as assignment takes place one dictionary at a time
* Now gives "live" output while waiting for the IDL prompt
* Fixed bug related to long IDL 'help' output
* String arrays with arbitrary spaces now work

Version 0.1.3, 6 Feb 2008
* Added support for unsigned integers
* Fixed bug with byte/int8
* Added easy access to IDL variables and functions (__getattr__ and __setattr__)

Version 0.1.2, 4 Feb 2008
* Performance improvement:
  * 5-100 times faster, tranferring from Python to IDL
  * ~1.5x faster, transferring from IDL to Python
* Renamed Session class to IDL

Version 0.1.1, 1 Feb 2008
* Removed timeout limit
* Fixed typo in license
* README and LICENSE files

Version 0.1, 31 Jan 2008
* Wrapper on Pexpect, with conversions between IDL data and NumPy arrays
* Handles arbitrarily sized and shaped arrays of strings, ints and floats
