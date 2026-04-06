# tomato-ontology

# Structure

```
owl:Thing
├── Food
│   └── Fruit
│       └── Tomato
├── TomatoVariety
├── Dish
└── GrowingCondition
```

# Relationships

```
Tomato > hasVariety > TomatoVariety
Dish > usesTomato > TomatoVariety
TomatoVariety > gownUnder > GrowingCondition
```


# Usage of online owl file

## Open it from the web in Protoge

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

## Other Ontologies can import this Ontology

For example they can re-use the current ontoly to extend it or they can add the current ontology classes as subclasses as part of a larger system.

## The Ontology can be pusblished

Any large portal can be created, or an existing agricultural web portal can use this information for its database.

# Queries

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

# Reasoner results (HermiT 1.4.3.456)

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


# Extension Tasks:

- Created disjointness: Dish, TomatoVariety and GrowingCondition are all declared disjoint from each other as a dish cannot be a variety and cannot be a gowing condition.


- Cardinality restriction: On the class Dish a restriction is used.

```
Dish SubClassOf usesTomato min 1 TomatoVariety
```

This command enforces that every Dish individual must be linked to at least one Tomato via usesTomato. A reasoner can flag this if a dish has no usesTomato assertion.
