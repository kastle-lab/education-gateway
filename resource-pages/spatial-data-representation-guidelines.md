### **Proto-OKN Guidelines for Representation of Spatial Data**

The Geospatial Working Group would like all Theme 1 project teams to consider using standard vocabularies to represent geospatial information.

#### **Representing Geometries**

The recommended practice for geometries (point, line, polyline, or polygon) is to use the predicates <span style='color:blue'>hasGeometry</span> and <span style='color:blue'>asWKT</span> and the class <span style='color:blue'>Feature</span> from the GeoSPARQL ontology. GeoSPARQL is a standard that enables on-the-fly geometric calculations to be used when evaluating queries. We do not aim to make the whole Proto-OKN GeoSPARQL compliant. However, GeoSPARQL provides an ontology with useful geospatial terms in it, and this ontology is widely used. The base IRI for the GeoSPARQL ontology is 
    http://www.opengis.net/ont/geosparql
which we will abbreviate here as <span style='color:blue'>geo</span>:

Every geospatial feature should be typed with the class <span style='color:blue'>geo:Feature</span> (see example below).

The object of an asWKT triple should be a literal with type <span style='color:blue'>“geo:wktLiteral”</span>. WKT is a format for geometries. A [WKT](https://en.wikipedia.org/wiki/Well-known_text_representation_of_geometry) literal by itself does not pick out a point or region on the Earth; it must be combined with a coordinate reference system (CRS). We recommend EPSG:4326 ([WGS-84 2D](https://en.wikipedia.org/wiki/World_Geodetic_System)) as the default CRS for Proto-OKN. This means any literal that is an object of <span style='color:blue'>geo:asWKT</span> will be interpreted as a set of EPSG:4326 coordinates, if not otherwise specified.

If you need to use coordinates in another CRS, one can include an IRI for the CRS inside the WKT string, before the coordinates, as for example in [this paper](https://www.semantic-web-journal.net/sites/default/files/swj176_0.pdf) on GeoSPARQL (Listing 4). This format is not standard WKT, but is widely used.

Example: if you have a power plant located at position -82, 39 in EPSG:4326 coordinates, your graph may contain triples like:

 <span style='color:blue'>ex:Plant12 rdf:type geo:Feature .
ex:Plant12 geo:hasGeometry ex:geometry_Plant12 .
ex:geometry_Plant12 geo:asWKT “POINT (-82 39)”^^geo:wktLiteral .</span>

#### **Names of Administrative Regions**

Where possible, please use the IRIs for US administrative regions of level 1 and 2 (states and counties) found in the [KnowWhereGraph](https://knowwheregraph.org) (KWG).

Examples include the following representation of the state of Maine (US states have two-digit identifiers) and Hamilton County in Ohio (counties have five digit identifiers). The identifiers for US administrative regions use their respective FIPS codes. 
kwgr:administrativeRegion.USA.23
kwgr:administrativeRegion.USA.39061

For any other administrative regions that are already present in Data Commons, please use Data Commons IRIs of the following format (the numeric codes or Data Commons GeoIDs):

dcgeoid:2301902795  (denoting City of Bangor, a census county subdivision)  or
dcgeoid:23019011000 (denoting a census tract)

All use the following namespace:
PREFIX dcgeoid: https://datacommons.org/browser/geoId/



#### **S2 Cells**

S2 cells are square-shaped regions on the Earth, defined by the [S2 library](http://s2geometry.io/devguide/s2cell_hierarchy.html), which can be used in place of traditional coordinates to designate locations. They come in many granularities, each level containing 4 times more cells (the sides of which are approximately half the length) than the last. Each level-n cell is divided into 4 level-n+1 cells. S2 cells have unique identifier numbers, which are also specified by the S2 library.

Each S2 cell on Earth, of level up to 9, has an IRI used in KWG. Each S2 cell within the United States, of level up to 13, is in KWG as well. If you refer to S2 cells in your graph, regardless of level, please use the following IRI format to be consistent with KWG:

The cell of level [L] with identifier [ID] should have the IRI

<span style='color:blue'>kwgr:s2.level[L].[ID]</span>

For example:
kwgr:S2.level13.5526595845832572928

They use the following namespace:
PREFIX kwgr: <http://stko-kwg.geog.ucsb.edu/lod/resource/>

#### **Topological Relations**

KWG also provides predicates for the common topological relations between regions, as originally defined by the SF ontology. If you assert any of these relations between two regions, please use the predicates:

<span style='color:blue'>kwg-ont:sfWithin
kwg-ont:sfContains
kwg-ont:sfEquals
kwg-ont:sfDisjoint
kwg-ont:sfOverlaps
kwg-ont:sfTouches
kwg-ont:sfCrosses</span>

They are provided by the namespace:
PREFIX kwg-ont: <http://stko-kwg.geog.ucsb.edu/lod/ontology/>

These have the same meaning as the corresponding geo: predicates, whose DE-9IM codes can be found here. These topological relations should have subjects and predicates that are regions on the Earth (geo:Feature), not Geometries.

An OWL ontology that slightly extends and simplifies the use of KWG spatial concept and relations is provided here:
https://github.com/SAWGraph/geospatial-kg/blob/e6977e35e9d57840f822ce42abe7d34734806c2a/ontologies/sawgraph-spatial-ontology.ttl

It additionally provides a relation <span style='color:blue'>spatial:connectedTo</span> (any kind of connection relation regardless whether the involved entities are points, lines or polygons), which subsumes any of the above topological relations EXCEPT for <span style='color:blue'>kwg-ont:sfDisjoint</span> and thus simplifies querying over multiple topological relations.

#### **Validation**

Not all WKT strings specify valid polygons, even those that come from shapefiles. Before uploading data to the Proto-OKN, it would be a good idea to validate all WKT literals in the data. This can be done using the [shapely](https://shapely.readthedocs.io/en/stable/manual.html) Python library. If invalid polygons exist in a knowledge graph, with some graph stores, indexing will fail.


#### **Contributors to these guideline (in alphabetical order)**

Torsten Hahmann

Pascal Hitzler

David Kedrowski

Shirly Stephen

Joseph Zalewski


