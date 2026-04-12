# tomato-ontology

# 1. Workbook

## Structure - Classes and Subclasses

```
owl:Thing
├── Food
│   └── Fruit
│       └── Tomato
├── TomatoVariety
├── Dish
└── GrowingCondition
```

## Relationships
+ gownUnder has been pronounced purposely like this because I made a mistake in the code earlier.

```
Tomato > hasVariety > TomatoVariety             eg.: Tomato_001 > hasVariety > Roma_Tomato
Dish > usesTomato > TomatoVariety               eg.: Bruschetta > usesTomato > Roma_tomato
TomatoVariety > gownUnder > GrowingCondition    eg.: Roma_Tomato > gownUnder > Outdoor_Condition
```

## Data properties
- climate
- colour
- cookingMethod
- cuisine
- dishName                                     eg.: Bruschetta
- flavor
- harvestDate
- isOrganic
- pH
- season
- size
- soilType
- tomatoID
- usesTomato
- varietyName
- weight

# 2. Emulation Task
- 6 classes, 2 of them are subclasses
- 3 object properties
- 21 individuals
- 3 object properties

# 3. Extension Tasks
- Assigned individuals to Tomato subclass
- Defined disjointness:
   - Dish cannot be a tomato variety
   - Dish cannot be a growing condition
- Cardinality restriction used on Dish class:
   - usesTomato min 1 Tomato (which means 1 tomato must be used as minumum for every dish individual)

# 4. Resoning
- Reasoning was used and helped me identify issues in code such as:
  - wrong data type used for individuals

## Reasoner results (HermiT 1.4.3.456)

```
------------------------------- Running Reasoner ------------------------------- 
Pre-computing inferences: 
    - class hierarchy 
    - object property hierarchy 
    - data property hierarchy 
    - class assertions 
    - object property assertions 
    - same individuals 
Ontologies processed in 131 ms by HermiT 
``` 

# 5. SPARQL Queries
+ gownUnder has been pronounced purposely like this because I made a mistake in the code earlier, used In q3.

- Prefixes

```
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX tomato: <http://https://hera191.github.io/tomato-ontology/tomato#>
```

- Query 1 -- List all tomato varieties and their colour

```
SELECT ?variety ?name ?colour
WHERE {
  ?variety rdf:type tomato:TomatoVariety .
  ?variety tomato:varietyName ?name .
  ?variety tomato:colour ?colour .
}
ORDER BY ?name
```

- Query 2 -- Find all dishes and the tomato varieties they use

```
SELECT ?dish ?dishName ?variety ?varietyName
WHERE {
  ?dish rdf:type tomato:Dish .
  ?dish tomato:dishName ?dishName .
  ?dish tomato:usesTomato ?variety .
  ?variety tomato:varietyName ?varietyName .
}
```

- Query 3 -- Find all individuals that are instances of food

```
SELECT ?individual ?type
WHERE {
  ?type rdfs:subClassOf* tomato:Food .
  ?individual rdf:type ?type .
}
```

- Query 4 -- Find varieties grown in what climate and what season

```
SELECT ?variety ?varietyName ?climate ?season
WHERE {
  ?variety rdf:type tomato:TomatoVariety .
  ?variety tomato:varietyName ?varietyName .
  ?variety tomato:gownUnder ?condition .
  ?condition tomato:climate ?climate .
  ?condition tomato:season ?season .
}
ORDER BY ?varietyName
```

- Query 5 -- Find all Hungarian dishes and their cooking method

```
SELECT ?dish ?dishName ?cookingMethod
WHERE {
  ?dish rdf:type tomato:Dish .
  ?dish tomato:dishName ?dishName .
  ?dish tomato:cuisine "Hungarian" .
  ?dish tomato:cookingMethod ?cookingMethod .
}
```


# 6. Linked Data

## How it could be reused

### Open it from the web in Protoge

This ontology can be re-used such as:
- In protoge
File > Open from URL.. > Click the RDF/XML file format if needed

```
------------------------------- Loading Ontology ------------------------------- 
Loading ontology from https://github.com/hera191/tomato-ontology 
URL connection input stream is compressed using gzip 
URL connection input stream is compressed using gzip 
URL connection input stream is compressed using gzip 
URL connection input stream is compressed using gzip 
URL connection input stream is compressed using gzip 
URL connection input stream is compressed using gzip 
URL connection input stream is compressed using gzip 
Finished loading https://github.com/hera191/tomato-ontology 
```

### Other Ontologies can import this Ontology

For example they can re-use the current ontoly to extend it or they can add the current ontology classes as subclasses as part of a larger system.

### The Ontology can be pusblished

Any large portal can be created, or an existing agricultural web portal can use this information for its database.
