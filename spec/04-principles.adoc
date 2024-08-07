== Principles

This section give a series of principles describing the scope of what this model is intending to offer.

Some of these principles are conceptual and may not be computationally tested against data created in accordance with this model. Other principles are and lead on to rules in the <<Rules>> section.


=== Uncertainty in position only

This model only directly addresses uncertainty in the position of spatial features. It does not address uncertainty in the values of other feature properties. For example, if a map of average daily temperature for a region was modelled using GeoSPARQL 1.1 and this model's elements, uncertainty in the measurement of the temperature values themselves would be out of scope.

NOTE:: Many other ontologies provide for the representation of measurement uncertain and simple models such as <<SDO, schema.org>> include properties such as https://schema.org/marginOfError[marginOfError] for this purpose.


=== Uncertainty not in resolution or accuracy

This model is not intended to represent spatial resolution or accuracy of positions (geometry elements): these are already representable with elements from <<GEO, GeoSPARQL>>, see:

* https://docs.ogc.org/is/22-047r1/22-047r1.html#_property_geohasspatialresolution[geo:hasSpatialResolution]
* https://docs.ogc.org/is/22-047r1/22-047r1.html#_property_geohasspatialaccuracy[geo:hasSpatialAccuracy]

This model aims to allow a user to represent specific values for uncertainty of position for a feature, rather than uncertainty which is unknown to them, perhaps due to the tolerance of instruments used to make position measurements.


=== Deterministic interpretation

The aim of this model is to allow a user to convey a defined amount of uncertainty in data and this amount is unambiguous: all users of the data will be able to derive the same amount.


=== XXX

This model defines a number of rules that define the manner in which the spatial representation of features that contain uncertainty is to be interpreted.

Uncertainty in position is conveyed by linking the feature not to one geometry but to multiple - a geometry collection. Each geometry in the collection conveys the certainty of the feature's location at its boundary with the value either being given in the data or inferrable due to the geometry's relative position in the collection.

A series of rules, given in the following subsections, define how certainty at each geometry in a collection is to be obtained. These rules are based on the instructions on the level of measurement <<KIRCH>> relationship between the geometries in a geometry collection contained. The level of measurement will be one of _Nominal_, _Ordinal_, _Integer_ or _Ratio_, as defined in the <<Levels of Measurement, Levels of Measurement>> vocabulary in the <<Supporting Vocabularies>> section of this model. It will either be indicated directly by the predicate <<levelOfMeasurement, level of measurement>> on a <<GeometryCollection, Geometry Collection>> instance, or it will be divined from other properties of the geometries. The default value is _Nominal_.

==== Obtaining the Level of Measurement

The following reasoning is to be used to determine the level of measurement of a Geometry Collection.

.Use of <<levelOfMeasurement, level of measurement>> predicate
..If a <<GeometryCollection, Geometry Collection>> instance indicates its member <<Geometry, Geometries>> are related by the <<levelOfMeasurement, level of measurement>> to be a particular level of measurement, that value is to be used

==== Rule: Nominal geometries



==== Rule: Ordinal geometries

==== Rule: Interval geometries

==== Rule: Ratio geometries

=== Feature display


=== Probabilistic Topological Functions

The <<GEO, GeoSPARQL>> standard provides both an ontology which defines spatial elements and predicates to indicate topological relationships between them and also function definitions that can be used to calculate those predicates.

For example, the predicate https://docs.ogc.org/is/22-047r1/22-047r1.html#sf_relations[`geo:sfWithin`] indicated that its subject is spatially _within_ its object where _within_ is the Simple Features Within relation defined by <<OGC-06-103r4>>. The related GeoSPARQL function is http://www.opengis.net/def/function/geosparql/sfWithin[`geof:sfWithin] which defines the parameters and results from the function used to calculate the `geo:sfWithin` relation between two spatial objects.

This model indicates how to calculate the https://docs.ogc.org/is/22-047r1/22-047r1.html#sf_relations[Simple Features relations] between features whose position is uncertain and represented using elements from this model. The general method is described below and specific topological relation calculations follow.

==== General method for probabilistic topological function calculation

For a spatial feature `A` with certain position - a single geometry - and spatial feature `B` with uncertain geometry - as conveyed by this model - calculate a simple feature relation `R` as follows:

. Determine the level of measurement `B_l` for the geometries of `B`
... If `B_l` is <<Levels of Measurement, nominal>>,
.... For each geometry `BG_i` of `B`, calculate `A R BG_i` for each geometry
.... If any `A R BG_i` is `true` then `A R B` is `true`
.... Else it is `false`
... If `B_l` is <<Levels of Measurement, ordinal>>,
.... For each geometry `BG_i` of `B`, calculate `A R BG_i` for each geometry in order




.... Starting with the first geometry `BG_1` and proceeding to the last, `BG_N`, calculate `A R BG_i` for each geometry
..... If
... If `B_l` is <<Levels of Measurement, interval>>,
... If `B_l` is <<Levels of Measurement, ratio>>,

[[table-order]]
.Order of geometry calculation per function
|===
| SF Relation | Order

| https://docs.ogc.org/is/22-047r1/22-047r1.html#_simple_features_relation_family[equals] | -
| https://docs.ogc.org/is/22-047r1/22-047r1.html#_simple_features_relation_family[disjoint] | lowest to highest
| https://docs.ogc.org/is/22-047r1/22-047r1.html#_simple_features_relation_family[intersects] | highest to lowest
| https://docs.ogc.org/is/22-047r1/22-047r1.html#_simple_features_relation_family[touches] | lowest to highest
| https://docs.ogc.org/is/22-047r1/22-047r1.html#_simple_features_relation_family[within] | lowest to highest
| https://docs.ogc.org/is/22-047r1/22-047r1.html#_simple_features_relation_family[contains] | highest to lowest
| https://docs.ogc.org/is/22-047r1/22-047r1.html#_simple_features_relation_family[overlaps] | any
| https://docs.ogc.org/is/22-047r1/22-047r1.html#_simple_features_relation_family[crosses] | any
|===



=== Model resources

This document is this model's "Specification" which is its authoritative, human-readable, definition document. This model also contains other resources with other roles:

[width="75%", cols="2,1,4"]
|===
| Resource | Role | Notes

| https://w3id.org/suff/ont.ttl[Ontology] | _Schema_ | The technical, machine-readable, version of this model
| <<Supporting Vocabularies>> | _Vocabulary_ | The codelist vocabularies developed for this model and links to others defined elsewhere but expected to be used by this model
| <<AnnexA>> & https://w3id.org/suff/validator.ttl[Validator] | _Validation_ | The machine-readable validator file used to validate data claiming conformance to this model
| <<AnnexB>> | _Example_ | Examples of data conforming, and some not conforming, to this model
|===
