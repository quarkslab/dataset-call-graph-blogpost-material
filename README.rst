Publication of a call graph dataset
===================================

The aim of this dataset is to learn structural information on unknown files, based on the call graphs, in order to classify them in two categories : malware or goodware.
A call graph is an abstract object produced by program analysis tools to represent the relationships between subroutines in a computer program. Each node represents a function and each edge represents a call from a function to another.


Extraction Method
-----------------

We have 1361 PE (portable executable), 546 are considered as sane files and 815 as malicious files. Call graphs are extracted from these binaries with the help of radare2. 
Call graphs contain a lot of information. In order to treat them from a statistical point of view, we reformat the attribute of each node, which represents a function, to summarize relevant information.

We are counting in each node the number of calls to the most frequent opcodes : ['mov', 'call', 'lea', 'jmp', 'push', 'add', 'xor', 'cmp',
'int3', 'nop', 'pushl', 'dec', 'sub', 'insl', 'inc','jz', 'jnz', 'je', 'jne', 'ja', 'jna', 'js', 'jns', 'jl', 'jnl', 'jg', 'jng']. [Ref]_

An opcode (or mnemonic) is the assembly language instruction which describes the operation to be performed.

For example if the node's attributes are : ['mov': 3, 'cmp' : 4] there are 3 mov operations, 4 cmp operations and none other from the previous list.


NB: These call graphs are extracted in a static way. Indeed radare2 doesn't execute these programs, they are just disassembled and transformed into call graphs. At the time of the execution some graphs may change.


Dataset Description
-------------------

The call graphs are organized into two subsets, one for the goodwares (sane files) and a second for the malwares.
They are in the form of a list, each element contains two dictionaries :

- One dictionary Node <-> Attributes
- One dictionary Node <-> Neighbors

This format allows us to access directly each node's neighbors which are commonly used in graph matching and classification.
 
Here are some descriptive statistics for this dataset :

Goodware :

- Number of graphs :  546
- Mean number of nodes :  648.1
- Mean degree : 3.3
- Median degree : 2.7
- Maximum degree  : 10.1
- Number of isolated nodes : 130812
- Mean of isolated nodes : 239.6
- Number of self loops : 0

Malware :

- Number of graphs :  815
- Mean number of nodes :  871.5
- Mean degree : 3.6
- Median degree : 3.7
- Maximum degree : 34.4
- Number of isolated nodes : 231990
- Mean of isolated nodes : 284.7
- Number of self loops : 0


Exploit this dataset
--------------------


.. code:: shell

        git clone https://github.com/quarkslab/dataset-call-graph-blogpost-material.git
        
        cd dataset-call-graph-blogpost-material/

        tar xvf call_graph_dataset.tar.gz

        cd dataset/


Call graphs can be loaded using pickle :

.. code:: python

        import pickle

        list_of_malware_graph=pickle.load(open("path_to_malware_graph","rb"))
        list_of_malware_graph=pickle.load(open("path_to_goodware_graph","rb"))

Links 
-----

.. [Ref] Comparative Analysis of Feature Extraction Methods of Malware Detection, in *International Journal of Computer Applications (0975 8887) Volume 120*, June 2015, https://pdfs.semanticscholar.org/8ec8/858a25ce2fb76388d40ad07878b0ec79f580.pdf


