# Railway Survey and Design Ontology (RSDO) — release 1.0

| | |
| --- | --- |
| **Ontology IRI** | <https://w3id.org/rsdo> |
| **Version IRI** | <https://w3id.org/rsdo/1.0> |
| **Term namespace** | `https://w3id.org/rsdo#` |
| **Preferred prefix** | `rsdo` |
| **Imports** | <https://w3id.org/bpo/1-2> (Building Product Ontology); BOT (<https://w3id.org/bot#>) and schema.org (<https://schema.org/>) terms are referenced |
| **Content** | 316 RSDO classes (326 classes in the signature), 9 RSDO object properties (11 with `qudt:unit`), 5 RSDO data properties, 751 axioms |
| **Validated on** | railway tunnel-portal (铁路隧道洞门) design — see *Scope and status* |
| **Licence** | CC BY 4.0 |

RSDO (**R**ailway **S**urvey and **D**esign **O**ntology) is a modular ontology
for the semantic description of railway survey and design information. It is
built on reusable AEC vocabularies — BOT for space topology, BPO for products,
components and attributes, QUDT for units — and adds the railway-specific
classification that those vocabularies do not provide: survey/design
disciplines, project spaces, the component and attribute structure of railway
engineering objects, and the semantic views that relate them.

## Scope and status of this release

* This release is the **terminology (schema) of RSDO, scoped to the railway
  tunnel-portal design case** documented in the accompanying paper. The classes,
  object properties and data properties that the tunnel-portal case study
  actually uses are included; branches of the full development ontology that the
  case study does not use (for example bridge foundations, girder decks,
  soil-particle descriptors and roadbed materials) are not part of release 1.0.
* The ontology **has been validated by railway tunnel-portal design**: the
  terminology in this file is the one used to describe the tunnel portals of a
  real railway tunnel, to retrieve the control parameters of each portal type,
  and to generate the corresponding IFC parametric components.
* RSDO is under **continuous development** — new components, attributes and
  disciplines are added as the ontology is applied to further railway projects.
  Later releases keep the same IRIs, so data written against release 1.0 stays
  valid.
* Version 1.0 is the first public release: the terminology required by the
  tunnel-portal case study of the paper, published under the persistent IRI
  <https://w3id.org/rsdo>.

## Files

| File | Syntax | Size | Note |
| --- | --- | --- | --- |
| `rsdo.owl` | OWL/XML | 128 KB | native Protégé serialization, all entity IRIs absolute |
| `rsdo.ttl` | Turtle | 89 KB | for RDF tooling / triple stores |
| `rsdo.rdf` | RDF/XML | 128 KB | for RDFLib-based instance generation |
| `rsdo.ofn` | OWL functional syntax | 81 KB | for OWL API / programmatic use |

All four serializations are isomorphic — loading any of them yields the
ontology IRI `https://w3id.org/rsdo`, version IRI `https://w3id.org/rsdo/1.0`,
326 classes, 11 object properties, 6 data properties and 751 axioms (verified by
round-tripping each file through the OWL API 5.1.20).

## Namespaces

| Prefix | Namespace IRI |
| --- | --- |
| `owl` | `http://www.w3.org/2002/07/owl#` |
| `rdf` | `http://www.w3.org/1999/02/22-rdf-syntax-ns#` |
| `rdfs` | `http://www.w3.org/2000/01/rdf-schema#` |
| `xsd` | `http://www.w3.org/2001/XMLSchema#` |
| `xml` | `http://www.w3.org/XML/1998/namespace` |
| `bot` | `https://w3id.org/bot#` |
| `bpo` | `https://w3id.org/bpo#` |
| `qudt` | `http://qudt.org/schema/qudt/` |
| `schema` | `https://schema.org/` |
| `rsdo` | `https://w3id.org/rsdo#` |

Note that `qudt` is the QUDT **schema** namespace
(`http://qudt.org/schema/qudt/`), which is where the QUDT classes and properties
such as `qudt:unit`, `qudt:Unit` and `qudt:QuantityKind` live.

## Structure of the ontology

RSDO is organised in five modules.

**Spatial module** — reuses BOT and takes `bot:Zone` as its core class. Railway
spaces are described at three levels: `RailwaySpace` (the whole line),
`ProfessionSpace` (the workspace of one survey or design discipline) and
`ProjectSpace` (the space of a concrete sub-project). The profession spaces
cover the three survey disciplines (engineering geology, hydrology, topography)
and the design disciplines (line, track, tunnel, bridge and culvert, subgrade,
station and yard, signalling, power supply, …). Spaces are related with the BOT
topology properties (`bot:hasSpace`, `bot:containsZone`) and linked to their
components with `bot:containsElement`.

**Complementary semantic module** — the same physical space can be seen from
several points of view, so the module mirrors the three spatial levels with
parallel semantic classes and the properties `hasRailwaySemantic`,
`hasProfessionSemantic` and `hasProjectSemantic` (and their inverses
`RailwaySemanticBelongsTo`, `professionSemanticBelongsTo`,
`projectSemanticBelongsTo`). A disciplinary view decomposes a semantic whole
into its sub-semantics, for example Engineering Geology into Geology, Hydrology
and Topography. This is what allows a component to be retrieved either through
its spatial position or through its disciplinary meaning.

**Component module** — reuses BPO and BOT, with `bpo:Element` and `bot:Element`
declared equivalent. Components are divided into **abstract** components
(`rsdo:AbstractElement` and its subclasses: terrain, tunnel inner contour,
open-cut lining section, railway track, …) and **solid** components
(`rsdo:SolidComponent` and its subclasses: geological body, tunnel body, tunnel
portal, …). The solid-component branch follows the *Classification and Coding
Standard for Railway Engineering Information Models*; the tunnel-portal case
extends it with `TunnelPortalElement` (end-wall, pillar-type, stepped,
wing-wall portals), `TunnelBodyElement`, `RockElement`,
`CutAndCoverStructureElement` and their parts (main end wall, side wall, pier
column, auxiliary retaining wall, wing wall, crown arch, sole plate, …).
`bot:containsElement` attaches components to the spaces that contain them.

**Attribute module** — attributes are attached to components and spaces with
`bpo:hasAttribute`. Attributes whose value is a single literal carry it in
`schema:value`; attributes that describe a range or an interval use
`bpo:hasValue1` and `bpo:hasValue2`, with `bpo:permittedStepSize` for the step
between admissible values. The module distinguishes professional attributes
(control point, railway alignment, geological, tunnel, terrain, …) from shared
attributes (functional, geometric, physical) so that they can be reused across
disciplines; enumerated design values (rock grade, surrounding-rock class,
traction type, …) are modelled as classes of the extended attribute branch.

**Unit module** — reuses the QUDT ontology; its schema namespace
`http://qudt.org/schema/qudt/` is declared as the `qudt` prefix of this
ontology. A quantity attribute (`rsdo:Quantity` and its subclasses, for example
`rsdo:BearingQuantity`, `rsdo:InnerContourCenterQuantity`,
`rsdo:RailwayLineQuantity` or `rsdo:StepQuantity`) is declared to be a
`qudt:Quantity`, and the relationship between an attribute and its unit is
defined with `qudt:unit`: the ontology adds `rsdo:hasUnit` as a sub-property of
`qudt:unit`, with domain `rsdo:Quantity` and range `qudt:Unit`. A value such as
"60 MPa" is therefore expressed as the numeric attribute value plus a QUDT unit
individual (`unit:MPA`, `unit:M`, `unit:MilliM`, `unit:DEG`, …). As with BOT and
schema.org, QUDT is referenced by IRI rather than copied into the file, so the
unit individuals (and the quantity kinds) are the ones published by QUDT itself.

## Licence

Creative Commons Attribution 4.0 International (CC BY 4.0),
<https://creativecommons.org/licenses/by/4.0/>.
