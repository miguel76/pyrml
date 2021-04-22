# pyRML
pyRML is a Pythong based engine for processing RML files. The [RDF Mapping Language](https://rml.io/specs/rml/) (RML) is a mapping language defined to express customized mapping rules from heterogeneous data structures and serializations to the RDF data model. RML is defined as a superset of the W3C-standardized mapping language [R2RML](https://rml.io/specs/rml/#bib-r2rml), aiming to extend its applicability and broaden its scope, adding support for data in other structured formats.
### Installation
pyRML requires Python 3.
Once the source code has been downladed it is possible to install the Python package by means of pip. For example:

```
pip install .
```

Alternatively, it is possible to install the pyRML package directly from GitHub in the following way:

```
pip install git+https://github.com/anuzzolese/pyrml
```

### Usage

It is possible to use pyRML either by means of its API or the command line tool that is provided along with the source package.

###### API
The ```RMLConverter``` is the key class of pyRML. It accepts the path to an RML file as input and return an RDF graph as output. The output graph is an instance of the class ```Graph``` provided by [RDFLib](https://github.com/RDFLib/rdflib).
```python
from pyrml import RMLConverter
import os

# Create an instance of the class RMLConverter.
rml_converter = RMLConverter()

'''
Invoke the method convert on the instance of class RMLConverter by:
 - using the file examples/artist/artist-map.ttl (see the examples in this repo);
 - obtaining an RDF graph as output.
'''
rml_file_path = os.path.join('examples', 'artists', 'artist-map.ttl')
rdf_graph = rml_converter.convert(rml_file_path)

# Print the triples contained into the RDF graph.
for s,p,o in rdf_graph:
    print(s, p, o)

```

###### Command line tool
The command line tool is implemented by the script ```converter.py```.
Such a script can be used in the followint way:

```bash
python converter.py [-o RDF out file] [-f RDF out file] [-m] input
```

where:
 - the positional argument ```input``` is the input RML mapping file for enabling the RDF conversion;
 - the optional arument ```-o filename``` is the file to store the resulting RDF graph. If no choice is provided then standard output is assumed as default.
 - the optional argunent ```-f rdf-syntax``` can be used to specify the syntax to serialise the RDF graph. Possible values are n3, nquads, nt, pretty-xml, trig, trix, turtle, and xml. If no choice is provided then NTRIPLES is assumed as default.
 - the optional flag ```-m``` enebles the conversion based on multiproccessing for speeding up the transformation process.
 
The following is an example about how to use the command line tool for processing the RML file available in ```examples/artists/artist-map.ttl```, thus converting the CSV files ```examples/artists/Artist.csv``` and ```examples/artists/Place.csv``` into an RDF graph serialised as TURTLE and stored into the file named ```artists_places.ttl```.

```bash
python converter.py -o artists_places.ttl -f turtle examples/artists/artist-map.ttl
```

