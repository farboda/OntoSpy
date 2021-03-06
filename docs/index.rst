.. OntoSpy documentation master file, created by
   sphinx-quickstart on Tue May  5 16:43:29 2015.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to OntoSpy's documentation!
===================================

OntoSpy is a lightweight Python library and command line tool for inspecting and visualizing vocabularies encoded in the `RDF <https://en.wikipedia.org/wiki/Resource_Description_Framework>`_  family of languages. 

See also: 

- CheeseShop: https://pypi.python.org/pypi/ontospy 

- Github: https://github.com/lambdamusic/ontospy

- Homepage: http://www.michelepasin.org/projects/ontospy

- Docs: http://ontospy.readthedocs.org


.. .. warning::
..     This documentation is still in draft mode.


In a nutshell
--------------

OntoSpy can be used either as an interactive language shell (a 
`repl <https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop>`_ ) or as a Python package. 

Calling the ``ontospy`` command from a terminal window launches the interactive shell. If you pass a valid graph URI (e.g. ``ontospy`` ``http://purl.org/spar/frbr``) then OntoSpy will attempt to extract and print out any ontology-related information contained in that graph. 

Many other options are available, in particular OntoSpy allows to load/save ontologies from/to a local repository so that they can be cached and quickly reloaded for inspection later on. All without leaving your terminal window!


**Is OntoSpy for me?**

Here are some reasons why you should use it:

- You love the command line and would never leave it no matter what.

- You have a bunch of RDF vocabularies you regularly need to interrogate, but do not want to load a full-blown ontology editor like Protege.

- You need to quickly generate documentation for an ontology, either as simple html pages or via some more elaborate interactive visualization. 

- You are developing a Python application that needs to extract schema information from an RDF, SKOS or OWL vocabulary. 

.. note:: OntoSpy offers no ontology editing functionalities, nor it can be used to interrogate a triplestore.


Quick example
--------------

Here's how it works from the command line (`video link <https://vimeo.com/169707591>`_):

.. raw:: html 

	<iframe src="https://player.vimeo.com/video/169707591" width="640" height="360" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>

If used as a Python package, the basic workflow is the following: load a graph by instantiating the ``Graph`` class with a file containing RDFS, OWL or SKOS definitions; you get back an object that lets you interrogate the ontology. That's all!

Let's take a look at the `Friend Of A Friend <http://semanticweb.org/wiki/FOAF>`_ vocabulary. 

.. code-block:: python

	In [1]: import ontospy
	INFO:rdflib:RDFLib Version: 4.2.0

	In [2]: g = ontospy.Graph("http://xmlns.com/foaf/0.1/")
	----------
	Loaded 631 triples from <http://xmlns.com/foaf/0.1/>
	started scanning...
	----------
	Ontologies found...: 1
	Classes found......: 14
	Properties found...: 67
	Annotation.........: 7
	Datatype...........: 26
	Object.............: 34
	SKOS Concepts......: 0
	----------

	In [3]: g.classes
	Out[3]: 
	[<Class *http://www.w3.org/2003/01/geo/wgs84_pos#SpatialThing*>,
	 <Class *http://xmlns.com/foaf/0.1/Agent*>,
	 <Class *http://xmlns.com/foaf/0.1/Document*>,
	 <Class *http://xmlns.com/foaf/0.1/Group*>,
	 <Class *http://xmlns.com/foaf/0.1/Image*>,
	 <Class *http://xmlns.com/foaf/0.1/LabelProperty*>,
	 <Class *http://xmlns.com/foaf/0.1/OnlineAccount*>,
	 <Class *http://xmlns.com/foaf/0.1/OnlineChatAccount*>,
	 <Class *http://xmlns.com/foaf/0.1/OnlineEcommerceAccount*>,
	 <Class *http://xmlns.com/foaf/0.1/OnlineGamingAccount*>,
	 <Class *http://xmlns.com/foaf/0.1/Organization*>,
	 <Class *http://xmlns.com/foaf/0.1/Person*>,
	 <Class *http://xmlns.com/foaf/0.1/PersonalProfileDocument*>,
	 <Class *http://xmlns.com/foaf/0.1/Project*>]

	In [4]: g.properties
	Out[4]: 
	[<Property *http://xmlns.com/foaf/0.1/account*>,
	 <Property *http://xmlns.com/foaf/0.1/accountName*>,
	 <Property *http://xmlns.com/foaf/0.1/accountServiceHomepage*>,
	 <Property *http://xmlns.com/foaf/0.1/age*>,
	 <Property *http://xmlns.com/foaf/0.1/aimChatID*>,
	 <Property *http://xmlns.com/foaf/0.1/based_near*>,
	 <Property *http://xmlns.com/foaf/0.1/birthday*>,
	 <Property *http://xmlns.com/foaf/0.1/currentProject*>,
	 <Property *http://xmlns.com/foaf/0.1/depiction*>,
		### etc....
		]

	In [5]: g.printClassTree()
	[1]    http://www.w3.org/2003/01/geo/wgs84_pos#SpatialThing
	[12]   ----_file_:Person
	[2]    _file_:Agent
	[4]    ----_file_:Group
	[11]   ----_file_:Organization
	[12]   ----_file_:Person
	[3]    _file_:Document
	[5]    ----_file_:Image
	[13]   ----_file_:PersonalProfileDocument
	[6]    _file_:LabelProperty
	[7]    _file_:OnlineAccount
	[8]    ----_file_:OnlineChatAccount
	[9]    ----_file_:OnlineEcommerceAccount
	[10]   ----_file_:OnlineGamingAccount
	[14]   _file_:Project


	In [6]: g.toplayer
	Out[6]: 
	[<Class *http://www.w3.org/2003/01/geo/wgs84_pos#SpatialThing*>,
	 <Class *http://xmlns.com/foaf/0.1/Agent*>,
	 <Class *http://xmlns.com/foaf/0.1/Document*>,
	 <Class *http://xmlns.com/foaf/0.1/LabelProperty*>,
	 <Class *http://xmlns.com/foaf/0.1/OnlineAccount*>,
	 <Class *http://xmlns.com/foaf/0.1/Project*>]

	In [7]: g.getClass("document")
	Out[7]: 
	[<Class *http://xmlns.com/foaf/0.1/Document*>,
	 <Class *http://xmlns.com/foaf/0.1/PersonalProfileDocument*>]

	In [8]: d = _[0]

	In [9]: print(d.serialize())
	@prefix ns1: <http://www.w3.org/2002/07/owl#> .
	@prefix ns2: <http://www.w3.org/2003/06/sw-vocab-status/ns#> .
	@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
	@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
	@prefix xml: <http://www.w3.org/XML/1998/namespace> .
	@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

	<http://xmlns.com/foaf/0.1/Document> a rdfs:Class,
	        ns1:Class ;
	    rdfs:label "Document" ;
	    rdfs:comment "A document." ;
	    rdfs:isDefinedBy <http://xmlns.com/foaf/0.1/> ;
	    ns1:disjointWith <http://xmlns.com/foaf/0.1/Organization>,
	        <http://xmlns.com/foaf/0.1/Project> ;
	    ns1:equivalentClass <http://schema.org/CreativeWork> ;
	    ns2:term_status "stable" .



	In [10]: d.parents()
	Out[10]: []

	In [11]: d.children()
	Out[11]: 
	[<Class *http://xmlns.com/foaf/0.1/Image*>,
	 <Class *http://xmlns.com/foaf/0.1/PersonalProfileDocument*>]



Contents
--------------

.. toctree::
   :maxdepth: 2

   installation
   quickstart
   quickstart_cmdline
   tests
   
   

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

