File opening
---------------

To start working with a file you have to open it.

``open()``
^^^^^^^^^^

The ``open()`` function is most often used to open files:

.. code:: python

    file = open('file_name.txt', 'r')

In the open() function:

-  ``'file_name.txt'`` - file name
-  You can specify not only the name but also the path (absolute or relative)
-  ``'r'`` - file opening mode

The ``open()`` function creates a **file** object to which different methods can then be applied to work with it.

File opening modes:

*  ``r`` - open file in read-only mode (default)
*  ``r+`` - open file for reading and writing
*  ``w`` - open file for writing only

  *  if the file exists, its content is removed
  *  if the file does not exist, a new one is created

*  ``w+`` - open file for reading and writing
  *  if the file exists, its content is removed
  *  if the file does not exist, a new one is created

*  ``a`` - open  file to add a data. Data is added to end of file
*  ``a+`` - open file for reading and writing. Data is added to end of file

.. note::
    r - read; a - append; w - write
