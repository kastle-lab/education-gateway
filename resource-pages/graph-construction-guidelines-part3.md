### **Proto-OKN Guidelines Document 3: FRINK Recommendations for Ontologies**

### **Introduction**

These recommendations were compiled from feedback given by the [FRINK](https://frink.renci.org) team.

### **Metadata**

A complete ontology should have metadata about itself (such as its creator) and its (non-borrowed) terms (such as a natural language description of the term). See [here](https://github.com/frink-okn/graph-descriptions/blob/main/README.md) for predicates to use when introducing such metadata.

### **Ontology Engineering**

* If possible, avoid using string literals that contain whole sentences expressing content about the subject area of your graph. There is probably enough information in those literals that they could be translated into triples.
* Please give data properties consistent range types, e.g., a single property should not take `xsd:integer` values and also `xsd:dateTime` values.
* If you use numeric values that have an associated unit: best practices for attaching units to numbers can be found in the first answer on this [Stack Overflow question](https://stackoverflow.com/questions/20248369/units-of-measurement-in-owl-and-rdf/20253100#20253100).
* Properties that take values from a known finite set of options should have IRI values, not data literals. For example, if you have a data property that takes the value `"M"^^xsd:string` for “male” and `"F"^^xsd:string` for “female”, consider replacing these strings with Wikidata IRIs for those concepts. This makes your graph “self-documenting,” and prevents data literals with duplicate meanings, malformed values, etc. If the allowed values of a property come from a finite set of IRIs, this set can be specified with an OWL axiom or SHACL constraint.
* True data literals are appropriate when the range of values of a property is potentially infinite (or too large to label each possible value with an IRI), or when the values of the property are clearly numeric (i.e. a user might want to add them, average them, etc.)

### **External Identifiers**

As a reminder, it is best practice to re-use external IRIs whenever possible instead of creating new ones; see:
* [https://kastle-lab.github.io/education-gateway/resource-pages/graph-construction-guidelines.html](https://kastle-lab.github.io/education-gateway/resource-pages/graph-construction-guidelines.html)
* [https://kastle-lab.github.io/education-gateway/resource-pages/graph-construction-guidelines-part2.html](https://kastle-lab.github.io/education-gateway/resource-pages/graph-construction-guidelines-part2.html)

This also applies to IRIs used in place of data values, as recommended in the previous section.

### **New Identifiers**

If you must create a new IRI, create it in a domain that you control, so that it is resolvable to a web page you control, if a user enters the IRI in their browser. Ideally, this web page should define the IRI.

#### **Authors:**

1. **Mahir Morshed**
- Email: [mmorshed@scripps.edu](mmorshed@scripps.edu)
- Affiliation: FRINK

2. **Joseph Zalewski**
- Email: [jzalewski@ksu.edu](mailto:jzalewski@ksu.edu)
- Affiliation: EduGate

May 2025