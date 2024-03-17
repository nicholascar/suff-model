== Elements

. <<Classes>>
. <<Predicates>>
. <<Vocabularies>>

This section defines the data elements of this model.

This model is composed of Web Ontology Language (OWL) <<OWL>> Classes, Predicates and classification vocabularies. While some predicates are restricted in their use to various classes, the Classes and Predicates are actually defined individually and both are "first class model citizens", with global identity, that can be used in isolation and together. This is in contrast to Unified Modelling Language (UML) _Class Diagrams_ which treat most Predicates as element of particular Classes.

This model defines only a few Classes, Predicates and Vocabularies but reuses many from other models. Where it does, basic details of a reference to that Class or Predicates' definition via `rdfs:isDefinedBy`, and a scope note about using it in this model are given.

[[fig-overview]]
.Overview of this model showing major elements
image::img/overview.svg[]


[[Classes]]
=== Classes

==== Classes defined by this model

** ??

[[Address]]
===== Address

[cols="2,6"]
|===
| Property | Value

| IRI | `addr:Address`
| Preferred Label | Address
| Definition | The Address class represents structured information that allows unambiguous identified of an object for the purposes of identification and location
| Is Defined By | This model
| Subclass Of | <<CompoundName>>
| Provenance | Derived from <<ISO19160-1>>'s `Address` class
| Expected Predicates | <<isNameFor>>, <<hasPart>>, <<hasGeocode>>, <<hasLifecycleStage>>,
| Example 
a| [source,turtle]
----
PREFIX addr: <https://linked.data.gov.au/def/addr/>
PREFIX apt: <http://linked.data.gov.au/def/address-part-types/>
PREFIX cn: <https://linked.data.gov.au/def/cn/>
PREFIX ex: <http://example.com/>
PREFIX ex-geom: <http://example.com/geometry/>
PREFIX ex-parcel: <http://example.com/parcel/>
PREFIX geo: <http://www.opengis.net/ont/geosparql#>
PREFIX ls: <https://linked.data.gov.au/def/lifecycle/>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX sdo: <https://schema.org/>

<http://example.com/oxford>
    a addr:Address ;
    sdo:hasPart [
        sdo:additionalType apt:numberFirst ;
        rdf:value 20
    ] ,
    [
        sdo:additionalType apt:road ;
        rdf:value <https://linked.data.gov.au/dataset/qldaddr/streetLocality/QLD140492> ; # "Oxford Place"
    ] ,
    [
        sdo:additionalType apt:locality ;
        rdf:value <https://linked.data.gov.au/dataset/qldaddr/locality/loc38f189794e03> ;  # "Shorncliffe"
    ] ,
    [
        sdo:additionalType apt:administrativeArea ;
        rdf:value <https://linked.data.gov.au/dataset/asgsed3/STE/3> ; # "Queensland"
    ] ,
    [
        sdo:additionalType apt:postcode ;
        rdf:value 4017
    ] ;
    addr:hasGeocode [
        geo:hasGeometry [
            sdo:additionalType <https://linked.data.gov.au/def/geocode-types/property-centroid> ;
            geo:hasGeometry ex-geom:x ;
        ] ;
    ] ;
    cn:isNameFor ex-parcel:161162441 ;
    ls:hasLifecycleStage [
        sdo:additionalType ex:Unofficial ;
    ] ;
.
----
|===

==== Existing classes reused by this model

* <<Feature>>
* <<Geometry>>
* <<GeometryCollection, Geometry Collection>>
* <<Concept>>
* <<Resource>>

[[Feature]]
===== Feature

[cols="2,6"]
|===
| Property | Value

| IRI | `geo:Feature`
| Preferred Label | Feature
| Definition | A discrete spatial phenomenon in a universe of discourse
| Is Defined By | <<GEO>>
| Scope Note | Used as the basis for the Road Object class
|===

[[Geometry]]
===== Geometry

[cols="2,6"]
|===
| Property | Value

| IRI | `geo:Geometry`
| Preferred Label | Geometry
| Definition | A coherent set of direct positions in space. The positions are held within a Spatial Reference System (SRS).
| Is Defined By | <<GEO>>
| Scope Note | Used to give spatial representation information for a Road Object
|===

[[GeometryCollection]]
===== GeometryCollection

[cols="2,6"]
|===
| Property | Value

| IRI | `geo:GeometryCollection`
| Preferred Label | Geometry Collection
| Definition | A collection of individual Geometries
| Is Defined By | <<GEO>>
|===

[[Concept]]
===== Concept

[cols="2,6"]
|===
| Property | Value

| IRI | `skos:Concept`
| Preferred Label | Concept
| Definition | An idea or notion; a unit of thought
| Is Defined By | <<SKOS>>
| Scope Note | Used to indicate a value that should come from a vocabulary (modelled as a `skos:ConceptScheme)
|===

[[Resource]]
===== Resource

[cols="2,6"]
|===
| Property | Value

| IRI | `rdfs:Resource`
| Preferred Label | Resource
| Definition | The class resource, everything
| Is Defined By | <<RDFS>>
| Scope Note | Used to indicate any kind of RDF value - a literal, IRI or Blank Node
|===


[[Predicates]]
=== Predicates

==== Predicates defined by this model

* <<hasGeometries, has geometries>>
* <<levelOfMeasurement, level of measurement>>

[[hasGeometries]]
===== hasGeometries

[cols="2,6"]
|===
| Property | Value

| IRI | `:hasGeometries`
| Preferred Label | has geometries
| Definition | Indicates a collection of spatial representations for a given Feature
| Is Defined By | This model
| Domain | <<Feature>>
| Range | <<GeometryCollection>>
|===

[[levelOfMeasurement]]
===== levelOfMeasurement

[cols="2,6"]
|===
| Property | Value

| IRI | `:levelOfMeasurement`
| Preferred Label | level of measurement
| Definition | Indicates the level of measurement - nature of information within the values assigned to variables - within members of a given collection
| Is Defined By | This model
| Domain | <<GeometryCollection>>
| Range | <<Concept>>
|===

==== Existing predicates reused by this model

* <<hasGeometry, has geometry>>
* <<member>>
* <<_1>>

[[hasGeometry]]
===== hasGeometry

[cols="2,6"]
|===
| Property | Value

| IRI | `geo:hasGeometry`
| Preferred Label | has geometry
| Definition | A spatial representation for a given Feature
| Is Defined By | <<GEO>>
| Domain | <<Feature>>
| Range | <<Geometry>>
| Scope Note | Used to indicate the Geometry of a Feature, such as a Road Object
|===

[[member]]
===== member

[cols="2,6"]
|===
| Property | Value

| IRI | `rdfs:member`
| Preferred Label | member
| Definition | A member of the subject resource
| Is Defined By | <<GEO>>
| Domain | <<Resource>>
| Range | <<Resource>>
| Scope Note | In the SUFF Model, use this predicate to indicate the member <<Geometry>> instances within a <<GeometryCollection>>
|===

[[_1]]
===== _1

[cols="2,6"]
|===
| Property | Value

| IRI | `rdfs:_1`
| Preferred Label | firs member
| Definition | An instance of the https://www.w3.org/TR/rdf12-schema/#ch_containermembershipproperty[rdfs:ContainerMembershipProperty] indicating the first member in a collection
| Is Defined By | <<RDFS>>
| Subproperty Of | <<member>>
| Scope Note | In the SUFF Model, use this predicate to indicate the first member <<Geometry>> instance within an ordered <<GeometryCollection>>
|===


[[Vocabularies]]
=== Vocabularies

This model uses a vocabulary of Levels of Measurement to provide values for the range of the predicate <<levelOfMeasurement, level of measurement>>. that vocabulary is published online at *https://linked.data.gov.au/def/levels-of-measurement*.

A simple listing of that vocabulary is also given below.

==== Levels of Measurement

* *https://linked.data.gov.au/def/levels-of-measurement*

[cols="1,1,3"]
|===
| IRI | Preferred Label | Definition

| https://linked.data.gov.au/def/levels-of-measurement/nominal[`lom:nominal`] | nominal | The nominal level differentiates between items or subjects based only on their names or (meta-)categories and other qualitative classifications they belong to; thus dichotomous data involves the construction of classifications as well as the classification of items
| https://linked.data.gov.au/def/levels-of-measurement/ordinal[`lom:ordinal`] | ordinal | The ordinal level allows for rank order (1st, 2nd, 3rd, etc.) by which data can be sorted but still does not allow for a relative degree of difference between them
| https://linked.data.gov.au/def/levels-of-measurement/interval[`lom:interval`] | interval | The interval type allows for defining the degree of difference between measurements, but not the ratio between measurements
| https://linked.data.gov.au/def/levels-of-measurement/ratio[`lom:ratio`] | ratio | The ratio type takes its name from the fact that measurement is the estimation of the ratio between a magnitude of a continuous quantity and a unit of measurement of the same kind
|===
