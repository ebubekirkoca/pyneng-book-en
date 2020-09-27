Sorted
--------------

The ``sorted()`` function returns a new sorted list that is obtained from an iterable object that has been passed as an argument. The function also supports additional options that allow you to control sorting.

The first aspect that is important to pay attention to - **sorted** returns the list.

If you sort the list of items, a new list is returned:

.. code:: python

    In [1]: list_of_words = ['one', 'two', 'list', '', 'dict']

    In [2]: sorted(list_of_words)
    Out[2]: ['', 'dict', 'list', 'one', 'two']

When sorting the tuple also the list returns:

.. code:: python

    In [3]: tuple_of_words = ('one', 'two', 'list', '', 'dict')

    In [4]: sorted(tuple_of_words)
    Out[4]: ['', 'dict', 'list', 'one', 'two']

Sorting the set:

.. code:: python

    In [5]: set_of_words = {'one', 'two', 'list', '', 'dict'}

    In [6]: sorted(set_of_words)
    Out[6]: ['', 'dict', 'list', 'one', 'two']

Sorting the string:

.. code:: python

    In [7]: string_to_sort = 'long string'

    In [8]: sorted(string_to_sort)
    Out[8]: [' ', 'g', 'g', 'i', 'l', 'n', 'n', 'o', 'r', 's', 't']

If you pass a dictionary to sorted() the function will return sorted list of keys:

.. code:: python

    In [9]: dict_for_sort = {
       ...:         'id': 1,
       ...:         'name':'London',
       ...:         'IT_VLAN':320,
       ...:         'User_VLAN':1010,
       ...:         'Mngmt_VLAN':99,
       ...:         'to_name': None,
       ...:         'to_id': None,
       ...:         'port':'G1/0/11'
       ...: }

    In [10]: sorted(dict_for_sort)
    Out[10]:
    ['IT_VLAN',
     'Mngmt_VLAN',
     'User_VLAN',
     'id',
     'name',
     'port',
     'to_id',
     'to_name']

reverse
~~~~~~~

The **reverse** flag allows you to control the sorting order. By default, the sorting will be incremental.

The **reverse** flag changes the order:

.. code:: python

    In [11]: list_of_words = ['one', 'two', 'list', '', 'dict']

    In [12]: sorted(list_of_words)
    Out[12]: ['', 'dict', 'list', 'one', 'two']

    In [13]: sorted(list_of_words, reverse=True)
    Out[13]: ['two', 'one', 'list', 'dict', '']

key
~~~

With the **key** option you can specify how to perform sorting. The **key** parameter expects the function by which the comparison should be performed.

For example you can sort a list of strings by string length:

.. code:: python

    In [14]: list_of_words = ['one', 'two', 'list', '', 'dict']

    In [15]: sorted(list_of_words, key=len)
    Out[15]: ['', 'one', 'two', 'list', 'dict']

If you want to sort dictionary keys but ignore string register:

.. code:: python

    In [16]: dict_for_sort = {
        ...:         'id': 1,
        ...:         'name':'London',
        ...:         'IT_VLAN':320,
        ...:         'User_VLAN':1010,
        ...:         'Mngmt_VLAN':99,
        ...:         'to_name': None,
        ...:         'to_id': None,
        ...:         'port':'G1/0/11'
        ...: }

    In [17]: sorted(dict_for_sort, key=str.lower)
    Out[17]:
    ['id',
     'IT_VLAN',
     'Mngmt_VLAN',
     'name',
     'port',
     'to_id',
     'to_name',
     'User_VLAN']

The **key** option can accept any functions, not only embedded ones. It is also convenient to use the anonymous lambda() function.

Using the **key** option you can sort objects by any element. However, this requires either lambda() or special functions from the **operator** module.

For example, in order to sort the list of tuples with two items in the second element, you should use this technique:

.. code:: python

    In [18]: from operator import itemgetter

    In [19]: list_of_tuples = [('IT_VLAN', 320),
        ...:  ('Mngmt_VLAN', 99),
        ...:  ('User_VLAN', 1010),
        ...:  ('DB_VLAN', 11)]

    In [20]: sorted(list_of_tuples, key=itemgetter(1))
    Out[20]: [('DB_VLAN', 11), ('Mngmt_VLAN', 99), ('IT_VLAN', 320), ('User_VLAN', 1010)]

