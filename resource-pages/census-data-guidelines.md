### **Proto-OKN Guidelines for querying Census Data**

The Census Working Group would like all Theme 1 project teams to consider using SPIDER’s technology to decompose queries to both the Proto-OKN and outside databases and merge query results. This technology allows for the use of existing, outside API resources, like the DataCommons SPARQL API, where administrative datasets like the Census have already been integrated.

#### **Features that suggest utilizing existing, external databases**

More than a third of the Proto-OKN teams had identified a need to merge Census data into their query results to provide a greater data context. The Census Working Group originally met to identify what these specific data needs were inside the dataset and to evaluate existing implementations of the data. During these initial meetings, some key features emerged:

- No team had any unique needs of data fields in the dataset that were not regularly captured or provided in an outside implementation
- No team had any unique needs of which year of the Census or ACS was used, other than for the year to align with the year of their dataset or provide the longitudinal information
- Census data had already been integrated into the DataCommons and the DataCommons has a SPARQL endpoint

Given the lack of a unique need of the Proto-OKN teams, that there are multiple datasets that researchers think of when they generally refer to the Census (Decennial Census, ACS estimates), and its existing implementation in an existing public database, it was decided to pursue the integration of the DataCommons API.

#### **Integration of External Databases with SPIDER**

At this point, the integration of external datasets should be restricted to SPARQL endpoints. This way, queries to multiple datasets (Proto-OKN and external datasets) can be automatically federated and results merged.

The corresponding SPARQL queries would need to use the
SERVICE \<endpoint\> {....} keyword.

**Example:**
Which ninth circuit court handles the most voting related cases?

```sparql
PREFIX scales: <http://schemas.scales-okn.org/rdf/scales#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX j: <http://release.niem.gov/niem/domains/jxdm/7.2/#>
PREFIX nc: <http://release.niem.gov/niem/niem-core/5.0/>

SELECT DISTINCT ?courtName ?name (COUNT(?case) AS ?caseCount)
WHERE {
  ?case nc:CaseGeneralCategoryText "civil"^^xsd:string .
  ?case nc:CaseSubCategoryText "441 Voting" .
  ?case j:CaseCourt ?court .
  ?court scales:isInCircuit "Ninth" ;
         j:CourtName ?courtName ;
         nc:CountryCode ?countryCode .

  SERVICE <Data Commons SPARQL endpoint> {
    SELECT ?name
    WHERE {
      ?place a Place .
      ?place dcid:geoId ?countryCode .
      ?place schema:name ?name .
    }
    LIMIT 1
  }
}
GROUP BY ?courtName ?name
ORDER BY DESC(?caseCount)
```

#### **Authors**

1. **Adam Pah**
   - Email: [apah@gsu.edu](mailto:apah@gsu.edu)
   - Affiliation: IJP

2. **Volkmar Frinken**
   - Email: [volkmar@onai.com](mailto:volkmar@onai.com)
   - Affiliation: SPIDER

3. **Guha Jayachandran**
   - Email: [guha@onai.com](mailto:guha@onai.com)
   - Affiliation: SPIDER
