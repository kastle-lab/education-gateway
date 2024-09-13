#### **Proto-OKN Best Practice Guidelines for Knowledge Graph Construction: Relations, Classes**

For questions, contact [edugate@proto-okn.net](mailto:edugate@proto-okn.net).

#### **Standard Relation Identifiers**

There are online resources, such as [Linked Open Vocabularies](https://www.lov.okfn.org/), [DBpedia’s Archivo](http://dbpedia.org/ontology/), and [NCBO BioPortal](https://bioportal.bioontology.org/), from which standard IRIs from established ontologies for common properties (such as spatial containment or overlap, equivalence of entities, etc.) can be taken. In addition, Theme 2 maintains a list of [IRIs that it has found useful]((https://docs.google.com/document/d/1mDErGTy7Kfg6Ma1vbikCYwnNR53zTgh914bRcue0_Mw/edit#heading=h.ozq48iibstyy)) as part of its analysis of existing submitted graphs. Whichever set of IRIs you draw from, their use will help improve the connectivity of your graph with other graphs. 

If your graph has triples representing one of these common relations, we recommend that you use a standard IRI as the predicate of these triples. If you use a different IRI with exactly the same meaning as a standard one, please put a triple in your graph asserting that your IRI is equivalent to the standard one, using the predicate `http://www.w3.org/2002/07/owl#equivalentProperty` (owl:equivalentProperty). This will make the various POKN graphs more interoperable.

**Example:**

Suppose the recommended IRI for spatial containment is `http://www.opengis.net/ont/geosparql#sfContains`. Let `ex:` represent your own namespace. To assert that Kansas is fully inside the US, your graph should use a triple like:

```
ex:UnitedStates <http://www.opengis.net/ont/geosparql#sfContains> ex:Kansas.
```

But if, instead, you have 

```
ex:UnitedStates ex:hasSpatialPart ex:Kansas .
```
your graph should contain the triple

```
ex:hasSpatialPart owl:equivalentProperty <http://www.opengis.net/ont/geosparql#sfContains> .
```

#### **Representing Types**

If your graph asserts that some entities have certain types, we recommend making the types IRI’s (not string or other data literals), and assigning them to entities using the predicate `rdf:type`. Using data literals for types slows down SPARQL querying. If you have type information that you want to represent with a property other than `rdf:type`, please also use `rdf:type`. This maintains consistency across graphs and enables some RDFS inferences.

**Example:**

This is suboptimal for query speed:
```
ex:Bob ex:hasType “Bird” .
```

This is better:
```
ex:Bob ex:hasType ex:Bird .
```

This is best, and should be used even if you also use your own IRI ‘hasType’:
```
ex:Bob rdf:type ex:Bird .
```

#### **Authors**

Joseph Zalewski, jzalewski@ksu.edu (EduGate)
Mahir Morshed, mmorshed@scripps.edu (FRINK)
Pascal Hitzler, hitzler@ksu.edu (EduGate)

September 2024


