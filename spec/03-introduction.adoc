== Introduction

This model defines ways of arranging models elements from <<GEO, GeoSPARQL>>, other common https://en.wikipedia.org/wiki/Semantic_Web[Semantic Web] models such as <<SDO, schema.org>>, and some of its own elements to represent uncertainty in the position of geospatial features. By representing uncertainty in the data, this model removes possible ambiguity in the presentation of features with positional uncertainty and allows for their use with topological functions.

The figure below and code listing that follow it give graphical and data views of simple example data created according to this model that represent a feature with spatial uncertainty.

[id="fig-ex1",width="50%",align="center"]
.A simple example of a geospatial feature with positional uncertainty - the location of an Indigenous people's homeland area. The polygons A, B, C & D give different estimates for the extent of the area with A being the most certain and D the least.
image::img/ex1.svg[]

.Listing 1: Example data for the figure above
[source, turtle]
----
include::code/ex1.ttl[]
----

The following subsection in this Introduction describes the motivation for this work and the final subsection describes the structure of this specification.

=== Motivation

The immediate motivation for this model was the need, in multiple projects in 2022 - 2024, to represent the position of features with uncertainty.

For example, to represent the traditional homeland areas of some Australian First Nations people which are known to not be sensibly represented with precise positions such as polygons with crisp boundaries due to the naturally fluid nature of First Nations peoples' land occupation patterns.

Another example is mineral occurrences: mining companies sample the earth's crust to determine where high concentrations of certain minerals are but they do so by sample - boreholes - and only build an incomplete picture of the occurrence area, i.e. one with uncertainty.

A third example is endangered species locations: in Australia, it can be illegal to represent the precise location of endangered native species however representations of the species' location must be made and shared for environmental impact and other land use assessments. Obscured locations are currently provided for such species however the manufactured uncertainty is usually represented as a crisp polygon and additional notes on tolerances. Uncertainty built into the data would be better.

The requirement to handle spatial uncertainty is thought to be widespread, for conversations about the potential for doing this hav attracted much interest.

GeoSPARQL defines spatial https://docs.ogc.org/is/22-047r1/22-047r1.html#_class_geofeature[`Features`] and https://docs.ogc.org/is/22-047r1/22-047r1.html#_geometry_class[`Geometry`] classes that can be used, with elements from other well-known Semantic Web models, for data with uncertainty in all the above scenarios however, without dedicated methods for this, the representation and handling of it will differ in implementations.

Where spatial uncertainty is handled in mainstream non-Semantic Web Graphical Information Systems (GIS), such as ESRI's ArcGISfootnote:[See the following ESRI product video at 8:03: https://www.esri.com/arcgis-blog/products/arcgis-pro/mapping/how-to-visualize-uncertainty-for-lines/], it is handled superficially - in the display of the data, not encoded within the data. This leads to the issue of reproducibility where if data with uncertainty handled superficial is displayed elsewhere, the display may not be the same if the display software is different.

This model encodes uncertainty in the data itself which, with the rules presented in the <<Principles>> section below, ensure that any software systems can implement the same display. Not only that: if uncertainty is encoded as per this model and the rules adhered to, topological calculations that result in probabilistic outcomes can be implemented against the data too.

=== Structure

This model provides:

. <<Principles>>
.. A series of principles describing the scope of what this model is intending to offer
. <<Elements>>
.. New Semantic Web Classes and Predicated defined here and guided reuse of Classes and Predicates from other models to represent uncertainty
. <<Rules>>
.. A series of rules that must be followed in the use of the elements of this model to ensure consistency in implementation
. <<Topological Functions>>
.. Definitions for topological functions that can be performed on spatial data with uncertainty, as represented by the Elements of this model in accordance with this model's Rules

and even a https://docs.ogc.org/is/22-047r1/22-047r1.html#_class_geogeometrycollection[`Geometry Collection`] class which can be used to associate multiple geometries with a feature. This is the core of this model since multiple geometries for a feature can be used to convey location uncertainty in several ways - see the <<Principles>> below.

GeoSPARQL 1.1 does not, however, provide all the elements needed for deterministic representations of feature location uncertainty, nor does GeoSPARQL 1.1 describe how to perform topological functions on features with uncertain location representation. This model defines conventions for GeoSPARQL 1.1 element use, and introduces several new model elements, for this purpose.
