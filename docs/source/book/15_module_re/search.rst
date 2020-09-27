Search function
--------------

Function ``search()``: 

* is used to find a substring that matches the template
* returns the Match object if a substring is found
* returns ``None`` if no substring was found

The search() function is suitable when you need to find only one match in a string, for example when a regular expression describes the entire string or part of a string.

Consider an example of using the search() function to parse a log file.

The log.txt file contains log messages indicating that the same MAC is too often re-learned on one or another interface. One of the reasons for these messages is loop in network.

Contents of log.txt file:

::

    %SW_MATM-4-MACFLAP_NOTIF: Host 01e2.4c18.0156 in vlan 10 is flapping between port Gi0/16 and port Gi0/24
    %SW_MATM-4-MACFLAP_NOTIF: Host 01e2.4c18.0156 in vlan 10 is flapping between port Gi0/16 and port Gi0/24
    %SW_MATM-4-MACFLAP_NOTIF: Host 01e2.4c18.0156 in vlan 10 is flapping between port Gi0/24 and port Gi0/19
    %SW_MATM-4-MACFLAP_NOTIF: Host 01e2.4c18.0156 in vlan 10 is flapping between port Gi0/24 and port Gi0/16

The MAC address can jump between several ports. In this case it is very important to know from which ports the MAC comes.

Try to figure out which ports and which VLAN was the problem. Check regular expression with one line from log file:

.. code:: python

    In [1]: import re

    In [2]: log = '%SW_MATM-4-MACFLAP_NOTIF: Host 01e2.4c18.0156 in vlan 10 is flapping between port Gi0/16 and port Gi0/24'

    In [3]: match = re.search(r'Host \S+ '
       ...:                   r'in vlan (\d+) '
       ...:                   r'is flapping between port '
       ...:                   r'(\S+) and port (\S+)', log)
       ...:

The regular expression is divided into parts for ease of reading. It has three groups:

* ``(\d+)`` - describes VLAN number
* ``(\S+) and port (\S+)`` - describes port numbers

As a result, the following parts of the line fell into the groups:

.. code:: python

    In [4]: match.groups()
    Out[4]: ('10', 'Gi0/16', 'Gi0/24')

In the resulting script, log.txt is processed line by line and port information is collected from each line. Since ports can be duplicated we add them immediately to the set in order to get a compilation of unique interfaces (parse_log_search.py file):

.. literalinclude:: /pyneng-examples-exercises/examples/15_module_re/parse_log_search.py
  :language: python
  :linenos:

The result of script execution:

::

    $ python parse_log_search.py
    Loop between ports Gi0/19, Gi0/24, Gi0/16 в VLAN 10

Processing of ‘show cdp neighbors detail’ output
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Try to get device parameters from 'sh cdp neighbors detail' output.

Example of output for one neighbor:

::

    SW1#show cdp neighbors detail
    -------------------------
    Device ID: SW2
    Entry address(es):
      IP address: 10.1.1.2
    Platform: cisco WS-C2960-8TC-L,  Capabilities: Switch IGMP
    Interface: GigabitEthernet1/0/16,  Port ID (outgoing port): GigabitEthernet0/1
    Holdtime : 164 sec

    Version :
    Cisco IOS Software, C2960 Software (C2960-LANBASEK9-M), Version 12.2(55)SE9, RELEASE SOFTWARE (fc1)
    Technical Support: http://www.cisco.com/techsupport
    Copyright (c) 1986-2014 by Cisco Systems, Inc.
    Compiled Mon 03-Mar-14 22:53 by prod_rel_team

    advertisement version: 2
    VTP Management Domain: ''
    Native VLAN: 1
    Duplex: full
    Management address(es):
      IP address: 10.1.1.2

The goal is to obtain such fields:

* neighbor name (Device ID: SW2) 
* IP address of neighbor (IP address: 10.1.1.2) 
* neighbor platform (Platform: cisco WS-C2960-8TC-L) 
* IOS version (Cisco IOS Software, C2960 Software (C2960-LANBASEK9-M), Version 12.2(55)SE9, RELEASE SOFTWARE (fc1))

And for convenience you need to get data in the form of a dictionary. Example of the resulting dictionary for SW2 switch:

.. code:: python

    {'SW2': {'ip': '10.1.1.2',
             'platform': 'cisco WS-C2960-8TC-L',
             'ios': 'C2960 Software (C2960-LANBASEK9-M), Version 12.2(55)SE9'}}

Example is checked on file sh_cdp_neighbors_sw1.txt.

The first solution (parse_sh_cdp_neighbors_detail_ver1.py file):

.. literalinclude:: /pyneng-examples-exercises/examples/15_module_re/parse_sh_cdp_neighbors_detail_ver1.py
  :language: python
  :linenos:

The desired strings are selected using startswith() string method. And in a string, a regular expression takes required part of the string. It all ends up in a dictionary.

The result is:

.. code:: python

    $ python parse_sh_cdp_neighbors_detail_ver1.py
    {'R1': {'ios': '3800 Software (C3825-ADVENTERPRISEK9-M), Version 12.4(24)T1',
            'ip': '10.1.1.1',
            'platform': 'Cisco 3825'},
     'R2': {'ios': '2900 Software (C3825-ADVENTERPRISEK9-M), Version 15.2(2)T1',
            'ip': '10.2.2.2',
            'platform': 'Cisco 2911'},
     'SW2': {'ios': 'C2960 Software (C2960-LANBASEK9-M), Version 12.2(55)SE9',
             'ip': '10.1.1.2',
             'platform': 'cisco WS-C2960-8TC-L'}}

It worked out well, but it can be done in a more compact way.

The second version of solution (parse_sh_cdp_neighbors_detail_ver2.py file):

.. literalinclude:: /pyneng-examples-exercises/examples/15_module_re/parse_sh_cdp_neighbors_detail_ver2.py
  :language: python
  :linenos:

Explanations for the second option:

* in regular expression, all line variants are described via ``|`` sign (or) 
* without checking a line the match is searched 
* if a match is found, the lastgroup() method is checked
* lastgroup() method returns name of the last named group in regular expression for which a match has been found
* if a match was found for the *device* group, the value that fells into the group is written to *device* variable 
* otherwise the mapping of ‘group name’: ‘corresponding value’ is written to dictionary

Result will be the same:

.. code:: python

    $ python parse_sh_cdp_neighbors_detail_ver2.py
    {'R1': {'ios': '3800 Software (C3825-ADVENTERPRISEK9-M), Version 12.4(24)T1',
            'ip': '10.1.1.1',
            'platform': 'Cisco 3825'},
     'R2': {'ios': '2900 Software (C3825-ADVENTERPRISEK9-M), Version 15.2(2)T1',
            'ip': '10.2.2.2',
            'platform': 'Cisco 2911'},
     'SW2': {'ios': 'C2960 Software (C2960-LANBASEK9-M), Version 12.2(55)SE9',
             'ip': '10.1.1.2',
             'platform': 'cisco WS-C2960-8TC-L'}}

