# Patakder Product Requirements

| Field | Value |
| --- | --- |
| Product | Patakder |
| Document version | 1.0 |
| Status | Draft for product review |
| Date | 24 November 2026, Asia/Manila |
| Platform | Web application |
| Selected framework | Laravel |
| Repository | [will3452/patakder](https://github.com/will3452/patakder) |
| Development status | Documentation only; implementation has not started |

## Contents

1. [Product purpose](#1-product-purpose)
2. [User outcomes and success measures](#2-user-outcomes-and-success-measures)
3. [Users and permissions](#3-users-and-permissions)
4. [Scope and delivery phases](#4-scope-and-delivery-phases)
5. [Main user journeys](#5-main-user-journeys)
6. [Access and projects](#6-functional-requirements-for-access-and-projects)
7. [Drawing and AI](#7-functional-requirements-for-drawing-and-ai)
8. [Views and simulation](#8-functional-requirements-for-views-and-simulation)
9. [Saving sharing and exports](#9-functional-requirements-for-saving-sharing-and-exports)
10. [Multi storey design and contractor functions](#10-multi-storey-design-and-broader-contractor-functions)
11. [User experience](#11-user-experience-requirements)
12. [Laravel architecture](#12-laravel-architecture-proposal)
13. [Data and geometry](#13-data-model-and-geometry-rules)
14. [Reliability security and performance](#14-reliability-security-and-performance)
15. [Acceptance and release evidence](#15-acceptance-scenarios-and-release-evidence)
16. [Delivery plan](#16-delivery-plan-and-dependencies)
17. [Decisions and open questions](#17-product-decisions-and-open-questions)
18. [Risks and mitigations](#18-main-risks-and-mitigations)
19. [Requirements inventory](#19-requirements-inventory-and-document-maintenance)

## 1 Product purpose

Patakder helps residential contractors turn house requirements into editable floor plans and understand those plans through connected 2D, 3D, exterior, and virtual walkthrough views. It combines simple drawing controls, real measurements, and AI-assisted layout proposals.

The primary user is the product owner's father: a contractor with hands-on house-building experience who has no formal engineering background and needs an approachable alternative to traditional CAD workflows. The application should respect his practical knowledge and explain unfamiliar controls and terms plainly.

The core experience is: describe a house or draw its layout, adjust the plan, inspect the space, and share a clear proposal with the client and the professionals responsible for technical design. All views and measurements come from one editable plan model.

### Confirmed product direction

- The product name is Patakder and should carry an Ilokano identity.
- The application must generate floor-plan proposals and allow practical manual editing.
- The drawing experience should feel easier than traditional CAD.
- Users need different views and a way to experience or simulate the plan.
- The primary drawing device is a laptop or desktop.
- The first release must support both single-storey and multi-storey houses.
- The product is a web application using Laravel.
- Product documentation and future application code belong in the selected GitHub repository.
- The current task is to document the product; application development requires a later instruction.

### Planning assumptions

These assumptions support a concrete first-release specification and can be revised during product review.

| Assumption | Initial position | Effect if changed |
| --- | --- | --- |
| Secondary devices | Laptop/desktop is confirmed; tablet editing is an enhancement and phone supports viewing/review first | Full touch editing requires a separate interaction and performance validation effort |
| Typical location | Philippine residential projects; location is entered per project | Local templates and professional requirements must match the actual jurisdiction |
| Measurement defaults | Metric dimensions; millimetres for stored lengths | Imperial display may be added without changing stored geometry |
| Interface languages | English and Filipino for the first release; Ilokano translation in the product roadmap | Full Ilokano copy requires a speaker review before release |
| Initial audience | A private pilot with the father and a small number of invited users | Public signup, commercial billing, and larger operational limits need separate launch decisions |
| AI provider | Configurable server-side provider, selected after a prototype evaluation | Provider cost, model capability, and data terms affect the integration choice |
| Sharing | Owner-controlled access to a saved plan revision | Real-time simultaneous editing is a later capability |

### Design and professional review boundary

Patakder produces concept layouts and visualizations. Saved plans, shared views, and exported drawings must identify themselves as **Concept plan — for professional review**. Client acceptance means acceptance of a design direction, not an engineering certification.

Architectural concepts and construction documents are different design stages. Construction drawings add information beyond a schematic floor plan; the [American Institute of Architects describes that distinction](https://www.aia.org/resource-center/defining-the-architects-basic-services). Patakder must provide a handoff for a qualified architect and engineer, as appropriate to the project, before a concept is used for construction. The application must not claim structural safety, permit readiness, or regulatory compliance from layout checks alone.

## 2 User outcomes and success measures

The primary outcome is that the father can prepare and explain a house-layout proposal using his own project measurements and preferences.

| Outcome | Proposed pilot measure |
| --- | --- |
| Start without CAD training | The primary user completes the onboarding example and creates a saved first plan after one guided session |
| Make an independent revision | After the guided session, the user can change a room dimension, move a door, furnish a room, and recover a mistake without help |
| Understand the proposed space | The user can switch between 2D, dollhouse, exterior, and walkthrough views and identify the same room in each |
| Produce a useful proposal | The user exports a legible dimensioned PDF and shares a saved revision with a client or professional reviewer |
| Preserve geometric correctness | The agreed geometry fixtures pass all invariants and measurement checks in section 13 |
| Preserve work | Save, restore, reconnect, and conflict scenarios complete without silently losing an acknowledged saved revision |
| Make AI useful | At least 8 of 10 agreed feasible pilot briefs produce a valid editable candidate within the retry budget; otherwise AI generation remains in pilot status |

These are product acceptance targets, not reported performance results. Timing and usability baselines will be established with the primary user during the pilot.

## 3 Users and permissions

| Role | Allowed work | Restrictions |
| --- | --- | --- |
| Project owner | Create projects, edit plans, run AI, manage revisions, export, invite collaborators, and revoke access | Cannot view another owner's private projects without an invitation |
| Invited editor | Edit the projects and features explicitly granted by the owner | Cannot transfer ownership, delete the project, or manage sharing unless granted |
| Reviewer/client | Open a granted revision, switch views, and leave feedback where enabled | Cannot change geometry, access private project files, or run chargeable AI jobs |
| Application administrator | Manage pilot access, supported catalogs, quotas, and service health | Project-content access requires an explicit support workflow and an audit event |

The first release requires an owner and a read-only reviewer. Invited editor accounts and review comments are included in the next design expansion. Every server request must enforce the relevant project permission, including asset downloads, exports, and background jobs.

## 4 Scope and delivery phases

The PRD covers the full product direction. Phases express delivery order; later functions remain specified rather than disappearing from scope.

| Phase | Function coverage | Completion meaning |
| --- | --- | --- |
| P0 Planning | Requirements, assumptions, architecture proposal, and acceptance criteria | This document is available for review; no application implementation is implied |
| P1 Core home planner | Accounts, project setup, site boundaries, manual 2D editor, single- and multi-storey plans, stairs and floor connections, AI generation and revisions, one plan model, dollhouse, exterior, cross-floor walkthrough, furniture and door checks, material previews, persistence, PDF/PNG/SVG/native export, read-only sharing | The complete concept-design workflow works for both single- and multi-storey residential concepts |
| P2 Extended design and collaboration | Elevations/sections, DXF exchange, sketch/PDF tracing and assisted conversion, editor invitations, review comments, richer materials, sunlight preview, improved touch editing, and language coverage | Users gain richer presentation, interoperability, and review workflows |
| P3 Contractor operations | Editable quantity takeoffs, quotations, budgets, expenses, purchasing/deliveries, workforce/payments, task progress, photo reports, client updates, and optional voice capture | Design information can support the contractor's project administration |
| Future evaluation | Real-time co-editing, richer roof modeling, photorealistic renders, advanced interoperability, specialist integrations, and commercial plans | Each requires a scoped feature decision before implementation |

P1 includes manual drawing, AI generation, multiple floors, and the linked 3D and cross-floor walkthrough experience. A single-storey-only, drawing-only, or static-image-only prototype is an intermediate development milestone, not the completed P1 product.

### Explicit exclusions

- Automated structural design or sizing of foundations, columns, beams, reinforcement, or load-bearing members.
- Certified seismic, wind, load, energy, thermal, ventilation, accessibility, or fire-safety analysis.
- Automatic architectural approval, professional signing/sealing, or permit submission.
- Unqualified claims that a layout is safe or compliant with a local code.
- Full compatibility with every CAD/BIM format or a complete replacement for professional CAD/BIM software.

If later requested, engineering simulations need a separate specification, validated specialist calculations, jurisdiction-specific rules, and a qualified professional's review workflow.

## 5 Main user journeys

### Generate a new house concept

1. The contractor creates a project and chooses metric units.
2. He enters the lot dimensions or boundary, road/front side, desired building area, and any supplied buildable boundary.
3. He specifies room counts, preferred room sizes, relationships, floor count, floor elevations, stair connections, and important constraints.
4. The application identifies missing or contradictory inputs before submitting the brief.
5. AI-assisted generation returns editable candidates that have passed the layout validators, with their assumptions and unmet preferences visible.
6. He selects a candidate, edits it, and switches to dollhouse, exterior, and walkthrough views.
7. He saves a named revision, exports a concept PDF, and shares that revision for review.

Example brief: “One-storey house, three bedrooms, two bathrooms, kitchen near the dining area, and the entrance facing the road.” Dimensions must come from the project inputs; the system must not invent a measured lot from this sentence.

### Draw manually

The contractor starts with a blank canvas or a template, draws the building perimeter, places internal walls, adds openings, labels the rooms, and adds furniture. He can type dimensions as an alternative to dragging. The application keeps shared walls and room boundaries consistent and immediately reflects committed changes in the other views.

### Revise through instructions

The contractor selects a room and requests “Palakihin ang kusina.” The system asks which dimension or tradeoff he wants if the instruction is ambiguous. It then displays proposed changes, affected rooms, and any conflicts. Accepting the proposal creates one reversible edit; rejecting it leaves the plan unchanged.

### Check the experience of the house

The contractor places furniture, opens doors in the preview, and follows a route through the house. He checks visible conflicts and circulation using dimensions and configurable clearance guides. These are spatial checks, not an ergonomic or regulatory certification.

### Review with another person

The contractor shares an immutable saved revision. A client can explore allowed views. In P2, the reviewer can compare explicitly shared revisions and leave feedback anchored to the revision and object/location it describes. Revising the plan does not silently move old feedback to a different design.

### Continue into project administration

In P3, the contractor creates an estimate from explicitly supported plan quantities, adds supplier and labor rates, issues a reviewed quotation, and tracks the resulting project's spending and progress. Updated designs produce an estimate comparison rather than silently rewriting an issued quotation.

## 6 Functional requirements for access and projects

Unless a row names a later phase, the requirements in sections 6 through 9 belong to P1.

### Accounts and preferences

| ID | Requirement | Acceptance criteria |
| --- | --- | --- |
| ACC-01 | Provide login, logout, invitation-based account creation, and password recovery | An invited pilot user can activate an account, sign in, reset a password, and end the session; invalid and expired links have recoverable errors |
| ACC-02 | Save a display name, interface language, measurement display, and default project preferences | Preferences persist across sessions; changing display units does not rescale stored geometry |
| ACC-03 | Enforce owner and reviewer permissions | A second owner cannot enumerate, open, edit, export, or download another owner's project using altered identifiers |
| ACC-04 | Require a fresh permission check for sensitive project actions | Removing access blocks subsequent API requests, export creation, and asset downloads; the UI provides an access-changed state |
| ACC-05 | Support account data export and deletion requests | The user can obtain their own project data; deletion explains ownership and retention consequences and follows the retention policy in section 14 |

### Projects and versions

| ID | Requirement | Acceptance criteria |
| --- | --- | --- |
| PRJ-01 | Create, rename, search, sort, duplicate, archive, restore, and delete projects | Dashboard actions operate on real persisted projects; duplicates receive independent identifiers and copying does not grant access to original share links |
| PRJ-02 | Store client-facing project title, optional location, notes, status, and thumbnail | Private client/contact details stay out of shared views unless explicitly included |
| PRJ-03 | Offer blank, template, AI-assisted, and later tracing entry points | Blank, validated single-/multi-storey templates, and AI paths work in P1; unavailable tracing paths are clearly identified and do not behave as working buttons |
| PRJ-04 | Autosave and show the actual save state | The editor distinguishes unsaved, saving, saved, offline, and failed states; “Saved” appears only after a durable server acknowledgement |
| PRJ-05 | Maintain recoverable revisions and named snapshots | A user can open an old revision, duplicate it, or restore it as a new revision without rewriting the historical record |
| PRJ-06 | Detect concurrent writes and stale tabs | A stale save is rejected or reconciled explicitly; it never overwrites a newer revision silently |
| PRJ-07 | Recover a local draft after a dropped connection or tab reload | When browser storage is available, the recovery copy is offered with its timestamp and server base revision; local storage failure is visible |
| PRJ-08 | Protect destructive actions | Deleting a project, clearing a plan, or restoring a different revision explains the impact; reversible actions have undo or a recoverable archive |

### Site and design brief

| ID | Requirement | Acceptance criteria |
| --- | --- | --- |
| SITE-01 | Accept a rectangular lot or a straight-edge polygon with explicit dimensions | The app validates a closed, non-self-intersecting boundary; unit labels and resulting area are visible |
| SITE-02 | Distinguish the lot boundary from the intended building footprint and supplied buildable envelope | A building cannot be presented as fitting solely because it fits the lot; unavailable restrictions are marked unknown |
| SITE-03 | Record the front/road side and optional north direction | Front labels and camera presets use the chosen front; north is independent of the front side |
| SITE-04 | Accept user- or professional-supplied setbacks and exclusions | Inputs record their source and status; the app does not silently supply legal setback values or claim that entered values prove compliance |
| SITE-05 | Capture room types, quantities, target areas, adjacency preferences, accessibility needs, and fixed objects | Required and preferred constraints are distinguishable; the saved brief can be edited before regeneration |
| SITE-06 | Capture floor count and vertical dimensions needed for visualization | Single- and multi-storey briefs work in P1; missing floor elevations, wall/ceiling heights, and roof heights receive clearly labeled editable assumptions |
| SITE-07 | Detect infeasible briefs before and after generation | Conflicting room requirements or out-of-bound footprints produce an explanation and choices; constraints are not silently relaxed |
| SITE-08 | Preserve input provenance | Measured, supplied, user-estimated, template-default, and AI-assumed values are distinguishable in the project and exported assumptions |

## 7 Functional requirements for drawing and AI

### The 2D editor

The editor should provide a focused drawing surface, a short tool palette, a selected-object properties panel, and a clear mode indicator. Numerical controls must offer the same essential operations as pointer gestures.

| ID | Requirement | Acceptance criteria |
| --- | --- | --- |
| EDT-01 | Provide pan, zoom, fit-to-plan, grid, snapping, rulers, and a visible scale | Zooming changes the view only; object dimensions remain unchanged; touch and pointer controls can be used without unexpected mode changes |
| EDT-02 | Draw straight exterior and interior walls with typed lengths, angles, thicknesses, and heights | Walls connect through shared endpoints; invalid lengths and zero-length segments are rejected with an explanation |
| EDT-03 | Provide room and perimeter drawing helpers | A rectangle/room tool creates valid wall geometry and merges shared boundaries rather than stacking duplicate walls |
| EDT-04 | Select, move, resize, split, join, duplicate, and delete supported objects | Every geometry change uses the same command/validation path and can be undone; deleting a wall explains its dependent openings and room changes |
| EDT-05 | Support orthogonal snapping and manually entered wall angles | Diagonal straight walls retain their dimensions across saves and views; curved walls are a separately scoped future capability |
| EDT-06 | Derive enclosed rooms and allow room names/types | The room list, area, and boundary reflect the wall geometry; open boundaries are identified instead of displaying a misleading enclosed area |
| EDT-07 | Place doors and windows in supporting walls | Openings stay attached using position along a wall; they remain within its length and height after edits, or the edit is blocked/flagged explicitly |
| EDT-08 | Edit door width, hinge side, swing, and opening angle, and window size and sill height | The 2D symbol and 3D opening represent the same parameters; invalid object dimensions do not enter a valid saved revision |
| EDT-09 | Add movable furniture, appliances, and fixtures from a dimensioned catalog | Objects have editable dimensions and rotation; scaled placeholders work even when a detailed 3D asset is unavailable |
| EDT-10 | Display wall dimensions, room dimensions, clear floor areas, and overall footprint dimensions | Measurement conventions are visible; values derive from geometry and match the export and 3D model within section 13 tolerances |
| EDT-11 | Add annotations and manually positioned dimension labels | Notes remain associated with a plan revision; moving a label does not change the measured object |
| EDT-12 | Provide object locking, visibility controls, and separate site/furniture/dimension layers | Locked geometry cannot be moved accidentally; hiding a layer changes presentation without deleting its content |
| EDT-13 | Provide undo and redo for manual changes and accepted AI proposals | One accepted AI proposal is one reversible operation; undo/redo keeps geometry, room identity, openings, and derived views consistent |
| EDT-14 | Show a room/object list and accessible numerical property controls | Users can find and edit objects without requiring precise canvas gestures; the selected object is highlighted in all active views |
| EDT-15 | Validate every committed edit and explain the consequence of dependent changes | Invalid topology is rejected or retained only as an explicitly incomplete draft; the last valid revision remains available |

### One plan model

| ID | Requirement | Acceptance criteria |
| --- | --- | --- |
| MOD-01 | Use one versioned, structured model for geometry and object properties | All rendered views, dimensions, exports, and quantity calculations reference the same plan revision and stable object IDs |
| MOD-02 | Store world-space lengths in integer millimetres, separate from screen pixels | Drawing scale, unit display, zoom, and repeated save/load operations do not change the plan's actual size |
| MOD-03 | Store walls, connections, openings, room labels, floor elevations, furniture, finishes, and roofs as editable entities | Loading a plan reconstructs editable objects; a generated image alone does not satisfy plan generation |
| MOD-04 | Distinguish supplied facts, editable assumptions, and derived values | Assumed heights/materials are identifiable in relevant views; calculated areas cannot be edited independently of their boundaries |
| MOD-05 | Update derived views after each accepted edit | Active views refresh against the committed local model; background output is labeled with its revision and identified as stale after later edits |
| MOD-06 | Validate schema version and support explicit migrations | An unsupported newer file cannot corrupt an existing project; migration makes a recoverable copy and records the source version |
| MOD-07 | Maintain stable relationships when rooms split, merge, or walls move | The app either preserves valid references or identifies objects, annotations, and estimates needing reassociation |
| MOD-08 | Share a documented geometry contract between browser and Laravel services | Client and server run the same acceptance fixtures and produce equivalent validation results; disagreement cannot silently produce a valid export |

### AI layout generation and revision

AI is a proposal interface. A schema and geometric validators determine whether its output can become an editable plan. The manual editor remains usable if an AI provider is unavailable.

| ID | Requirement | Acceptance criteria |
| --- | --- | --- |
| AI-01 | Convert a plain-language brief plus site inputs into a structured layout request | The user can inspect room counts, dimensions, hard constraints, and assumptions before generation; missing critical measurements require clarification |
| AI-02 | Generate up to three feasible single- or multi-storey candidate layouts per request | Candidates meet the requested floor count and hard constraints, including supported stair connections; fewer feasible candidates are explained and none is fabricated to fill a quota |
| AI-03 | Create actual editable walls, rooms, openings, and placement objects | Selecting a candidate opens the same editor used for manual plans, with dimensions and object identities preserved |
| AI-04 | Preview changes requested through plain-language instructions | The proposal lists affected objects, room/area changes, conflicts, and any changed assumptions before the user accepts |
| AI-05 | Respect locked objects, fixed boundaries, required rooms, and explicit constraints | A blocked instruction explains the conflict and asks for a permitted tradeoff; the system does not unlock or remove required elements silently |
| AI-06 | Validate outputs for schema, bounds, topology, opening placement, non-overlap, and declared access relationships | Invalid candidates are repaired within a bounded retry budget or returned as a failed/infeasible request; no valid saved revision is replaced |
| AI-07 | Handle ambiguity and unsupported requests | “Make it bigger” asks for a size/constraint tradeoff when needed; structural questions are routed to the professional-review boundary rather than producing member sizes |
| AI-08 | Support English and Filipino requests in the launch evaluation | Each language has pilot briefs and understandable clarification/error copy; Ilokano input is evaluated separately before it is advertised as supported |
| AI-09 | Run generation as a trackable job | The user sees queued/running/ready/failed/cancelled states, can cancel, and can retry without an accidental duplicate apply or unbounded charge |
| AI-10 | Preserve base revision, request provenance, provider usage, assumptions, and the acceptance decision | A proposal based on an old revision cannot overwrite newer work; the app requires regeneration or explicit reconciliation |
| AI-11 | Apply proposals through explicit user acceptance | Unaccepted proposals never modify the authoritative plan; accepting twice or refreshing a result page cannot apply the same operation twice |
| AI-12 | Limit disclosure and spending | Only the project context needed for the task reaches the configured provider; server-side quotas, timeouts, token/request budgets, and retry limits are enforced |

## 8 Functional requirements for views and simulation

### Connected views

| ID | Requirement | Acceptance criteria |
| --- | --- | --- |
| VIEW-01 | Provide 2D plan, 3D dollhouse, exterior orbit, and first-person walkthrough modes | Switching views preserves the current plan revision, floor context, and selected object; users can inspect one floor or the whole building |
| VIEW-02 | Provide a dollhouse/cutaway view with roof and wall visibility controls | Users can inspect every room; visual cutaways do not delete geometry or change area calculations |
| VIEW-03 | Provide exterior orbit and front, back, left, and right camera presets | Orientation follows the site/front setting; facade openings and heights match the underlying objects |
| VIEW-04 | Build visible walls, openings, ceilings/floors, and simple supported roofs from model parameters | Missing heights and unspecified roofs are identified; an exterior view does not invent a complete roof or structural system |
| VIEW-05 | Provide walkthrough movement for keyboard/mouse with accessible control alternatives | The user can enter, look around, move through permitted openings and supported stairs, stop movement, and exit the mode; phone/tablet review provides a suitable navigation control |
| VIEW-06 | Use configurable eye height and collision-aware navigation | The camera cannot pass through modeled closed barriers in normal walk mode; an optional overview/teleport control is explicitly distinct from walking |
| VIEW-07 | Preserve and share named viewpoints | A saved camera can be reopened against its associated revision; an invalid camera after a design change receives a safe reset |
| VIEW-08 | Degrade gracefully when 3D cannot run | The 2D editor, object properties, saving, and 2D exports remain available; a failed GPU context does not lose a plan |
| VIEW-09 | Provide elevation and simple section views in P2 | Cuts and elevation labels derive from the plan and vertical metadata, show their viewing direction, and identify unmodeled construction details |

### Spatial simulation

P1 simulation means exploring modeled space, object fit, opening movement, and appearance. Simulation output must identify which geometry, dimensions, assumptions, and revision it uses.

| ID | Requirement | Acceptance criteria |
| --- | --- | --- |
| SIM-01 | Check furniture/appliance fit against room and wall geometry | Oversized or intersecting placements are highlighted in 2D and 3D; visible dimensions allow the user to understand the issue |
| SIM-02 | Preview door swing and report modeled intersections | Moving/opening a door shows its swept area and conflicts with modeled walls or objects; users can inspect or change hinge/swing parameters |
| SIM-03 | Preview circulation and user-specified clearance guides | Users can follow a route and see narrow or blocked locations; thresholds are described as supplied preferences or validated rules with provenance |
| SIM-04 | Preview basic wall/floor finishes and colors | The same material assignment appears in supported views; a material change cannot alter geometry or imply a structural specification |
| SIM-05 | Distinguish missing model information from a passing result | Missing furniture, heights, barriers, or assumptions remain visible; absence of a collision is never presented as a complete safety/compliance check |
| SIM-06 | Add a sun/shadow preview in P2 | Location, north, date, time, and timezone are required or explicitly assumed; the result is labeled a visual approximation and does not claim thermal or daylight certification |
| SIM-07 | Support a shareable walkthrough in P1 and optional recorded walkthrough output later | A reviewer can navigate the permitted revision without editing it; video export is a separately enabled feature rather than a fake download action |

## 9 Functional requirements for saving sharing and exports

### Imports and design handoff

| ID | Requirement | Acceptance criteria |
| --- | --- | --- |
| IO-01 | Export a dimensioned concept plan as PDF | The user chooses paper size, orientation, scale/fit, visible layers, and units; room labels, legend, revision, assumptions, and concept status remain legible |
| IO-02 | Export 2D PNG and SVG and a current 3D image where supported | Output reflects the selected revision and view; SVG is sanitized and contains consistent geometric scale information |
| IO-03 | Export and import a native structured project package | Round-trip preserves editable geometry, units, identities, assumptions, and referenced asset definitions; external references are handled without unsafe file paths |
| IO-04 | Distinguish print-to-scale from fit-to-page | A chosen scale that cannot fit the sheet is rejected or moved to a larger sheet; fit-to-page labels its actual scale and includes a calibration dimension/scale bar |
| IO-05 | Associate every export with an immutable revision | Exports requested during unsaved edits require saving a snapshot or explicitly selecting a saved revision; later edits cannot change a running export's source |
| IO-06 | Provide export progress, retry, and authorized download | Failure does not produce a success notification or a broken placeholder file; regeneration uses the same source revision unless the user chooses otherwise |
| IO-07 | Provide DXF export and supported DXF import in P2 | Supported entity types, layer mapping, model-space units, and import losses are documented; known dimensions survive an agreed reference-file round-trip |
| IO-08 | Support image/PDF underlay tracing in P2 | The user sets scale from a known measurement and confirms orientation before tracing; a photo's pixels are not treated as measured dimensions |
| IO-09 | Add assisted sketch/plan conversion in P2 after manual tracing | Detected geometry is a proposal for correction and validation; uncertainty and unreadable measurements are flagged before acceptance |
| IO-10 | Assemble a professional-review handoff | The packet includes selected concept drawings, brief, supplied site information, object/room schedule, assumptions, and unresolved issues; it does not imitate a professional seal |

P1 does not advertise DWG or BIM compatibility. CAD exchange is a defined interoperability feature, with a supported subset and reference-file checks, rather than a promise that every professional file can be opened unchanged.

### Sharing and review

| ID | Requirement | Acceptance criteria |
| --- | --- | --- |
| REV-01 | Create revocable, expiring read-only access to a chosen saved revision | The owner sees the granted revision and allowed exports; unshared revisions, client details, and assets remain inaccessible |
| REV-02 | Keep shared reviews stable | Editing the owner's working plan leaves an already shared revision unchanged; the owner can publish a replacement deliberately |
| REV-03 | Allow invited editors in P2 | Project-scoped permissions cover drawing, AI usage, exports, and sharing separately; concurrent saves follow the conflict policy |
| REV-04 | Allow pinned feedback and replies in P2 | Comments store author, revision, object/location, view context, and status; deleted or moved geometry does not silently relocate a comment |
| REV-05 | Record reviewed/accepted design direction in P2 | The decision names the reviewer and exact revision; it remains separate from professional certification |
| REV-06 | Compare two saved revisions in P2 | Added/removed/moved objects and area changes are identifiable; presentation does not alter either source revision |

### Administration and support

| ID | Requirement | Acceptance criteria |
| --- | --- | --- |
| ADM-01 | Manage pilot invitations, account status, and access limits | Only authorized administrators can change access; changes have an audit record |
| ADM-02 | Manage approved templates and dimensioned catalogs | Catalog entries record dimensions, source/license, and version; updates do not silently change objects already used in saved plans |
| ADM-03 | Monitor and control AI/export jobs, quotas, and provider settings | Keys remain server-side; operators can inspect safe failure details and retry/disable a service without exposing private plan content |
| ADM-04 | Provide support diagnostics and backup/restore procedures | Users can report an issue with a correlation ID; recovery is demonstrated before launch rather than assumed from the presence of backups |

## 10 Multi storey design and broader contractor functions

### Multiple floors and stairs in P1

Single-storey and multi-storey support are confirmed first-release requirements. The product cannot be declared complete after validating only a ground-floor plan. At minimum, the launch evaluation includes single-, two-, and three-storey residential examples; any advertised upper capacity must be verified separately.

| ID | Requirement | Acceptance criteria |
| --- | --- | --- |
| FLR-01 | Create, name, reorder, duplicate, and remove floors | Every floor has a stable ID, finished-floor elevation, floor-to-floor distance, clear ceiling height, and an editable plan; display order and physical elevation are distinct; removing a floor explains affected stairs, openings, annotations, and saved viewpoints |
| FLR-02 | Edit one floor while referencing adjacent floors | An optional faded overlay shows the selected reference floor without making its geometry editable accidentally; copying objects creates independent IDs |
| FLR-03 | Place supported straight, L-shaped, and U-shaped concept stairs with landings | The model stores width, run/rise information, tread count, landing geometry, and source/target floors; unsupported forms are explained before generation or placement |
| FLR-04 | Represent stairwells, floor openings, double-height spaces, and voids | Voids appear consistently in the floor surface, area calculation, dollhouse, and walkthrough; a floor surface is not rendered across a modeled stair opening |
| FLR-05 | Validate vertical connectivity and geometric consistency | Stairs connect the intended elevations, fit their supplied footprint, and have compatible landings/openings; failed connections are highlighted rather than replaced by a visually disconnected stair |
| FLR-06 | Support cross-floor walkthrough and floor selection | The user can walk via modeled stairs and switch floors through an explicit navigation control; camera height follows the path instead of moving through a floor slab |
| FLR-07 | Preview roof, ceiling, and upper-floor visibility separately | The exterior shows the combined building; dollhouse mode can isolate or explode floors using a visual offset that does not change stored elevations |
| FLR-08 | Generate and revise multi-storey concepts through AI | The generated plan includes the requested floors and supported connections; editing one floor lists affected vertical connections before acceptance |
| FLR-09 | Export all selected floors and vertical assumptions | A PDF can contain one labeled sheet per floor with common scale/orientation; native data preserves floor relationships and elevation units |
| FLR-10 | Separate visual alignment from structural approval | Stacked walls and visual supports may be shown for reference; the application does not infer that a support or load path is engineered correctly |

Stair dimensions and headroom inputs must identify whether they were measured, supplied, or assumed. Geometric checks may use user-supplied or professionally validated clearance rules, with the rule source visible. Default stair appearance does not constitute a construction specification.

### Extended design features in P2

| ID | Requirement | Acceptance criteria |
| --- | --- | --- |
| EXT-01 | Improve touch editing without removing numerical controls | Core edit journeys pass on the declared tablet devices; desktop shortcuts remain optional rather than the only way to perform an action |
| EXT-02 | Add richer finishes, furniture libraries, and roof editing | Assets record units, dimensions, source/license, and performance cost; detailed visuals preserve the underlying model's dimensions |
| EXT-03 | Provide an Ilokano interface and evaluated Ilokano instructions | A fluent speaker reviews navigation, drawing terms, clarification prompts, and professional-review labels; unknown translations fall back visibly to a supported language |
| EXT-04 | Capture voice instructions as editable text | Recording requires an explicit action and browser permission; the transcript is shown for correction before any plan or expense entry changes |
| EXT-05 | Explain supplied plan notes and terminology in plain language | Answers reference the supplied note/object and distinguish source text from inference; uncertain or engineering-specific instructions are identified for professional review |
| EXT-06 | Support plan references and project photo attachments | Users can label files, choose which are shared, and remove them; attachments are not treated as surveyed geometry without calibration and confirmation |

### Contractor operations in P3

These functions preserve the original contractor-assistant direction as optional extensions. They do not replace the confirmed floor-planning and visualization work. Estimates depend on approved measurement methods and explicit user rates; they are not generated from an attractive render alone.

| ID | Requirement | Acceptance criteria |
| --- | --- | --- |
| OPS-01 | Produce editable quantity schedules for supported modeled items | Every quantity identifies its plan revision, units, formula/method, included/excluded elements, and assumptions; unsupported structural or service quantities remain unknown |
| OPS-02 | Build estimates using supplier prices, labor rates, equipment, transport, overhead, contingency, and markup | Rates include source/date; currency and rounding rules are explicit; totals reconcile from editable line items and avoid counting shared walls or openings twice |
| OPS-03 | Create reviewed quotations and change orders | A draft can be edited before issue; issued versions are immutable snapshots with client/project details, inclusions, exclusions, and an acceptance record |
| OPS-04 | Track budgets and actual expenses with receipts | Categories, dates, amounts, payment status, supplier, and attachments are editable; reports reconcile recorded expenses and distinguish unpaid commitments from cash paid |
| OPS-05 | Track materials, purchase requests, deliveries, and remaining stock | Quantities use consistent units; received/used/returned quantities are recorded separately and do not silently convert into money movements |
| OPS-06 | Track workers, assignments, attendance, agreed rates, and payment records | Entries are reviewable and access-controlled; wage/payment reports reconcile the recorded inputs without claiming statutory payroll compliance |
| OPS-07 | Track tasks, dependencies, target dates, responsible workers, and progress | A dependency or plan revision change identifies affected tasks; schedules are editable and do not promise a completion date from floor area alone |
| OPS-08 | Record dated site notes and progress photos | Files retain project association and access controls; users can produce a progress report with a selected date range |
| OPS-09 | Prepare client updates from selected notes and photos | AI-assisted wording is previewed and approved before export or sharing; the application never sends a message automatically on the user's behalf |
| OPS-10 | Compare revised designs and financial impact | A new plan revision creates a proposed estimate difference; an accepted quotation, paid expense, or delivery history is not rewritten silently |
| OPS-11 | Support optional voice capture for daily records | The transcript and extracted amounts/units are confirmed before saving; ambiguous quantities or currencies require correction |
| OPS-12 | Produce project summaries and administration exports | CSV/PDF totals match the underlying records; private workforce/payment information is excluded from ordinary client shares |

## 11 User experience requirements

### Workspace structure

The main workspace contains a project/floor selector, a compact drawing toolbar, a large drawing/viewing area, and a context panel for the selected object. AI assistance, issues, and revision history open as focused panels. Users should not need to navigate away from the plan to change a common dimension.

| Screen or area | Required content |
| --- | --- |
| Account and onboarding | Sign-in, project setup guidance, units, and an editable sample project |
| Project dashboard | Search, recent projects, thumbnail, saved state, status, and create/open/archive actions |
| Setup and brief | Site, road/front, floors, rooms, constraints, unknown inputs, and generation action |
| Drawing workspace | Tools, floor selection, dimensions, selected-object properties, save state, undo/redo, and visible active mode |
| View controls | 2D, dollhouse, exterior, walkthrough, roof/floor visibility, and camera reset |
| AI panel | Brief/proposal, assumptions, differences, job state, and accept/reject controls |
| Issues panel | Severity, affected object/floor, understandable reason, and locate/fix action |
| Revision and share panel | Saved revisions, named snapshots, share permissions/expiry, and export selection |
| Administration | Pilot access, catalogs, provider limits, job health, and audit/support actions |

### Interaction and accessibility

| ID | Requirement | Acceptance criteria |
| --- | --- | --- |
| UX-01 | Use plain language, visible labels, and short contextual help | The primary user can identify draw, select, move, measure, save, and view modes without relying on unexplained icons |
| UX-02 | Prioritize laptop/desktop drawing | Core editing is evaluated at 1366 × 768 and 1920 × 1080; panels can collapse so the plan remains usable at the smaller viewport |
| UX-03 | Support responsive review on phones | Shared plans, floor/view switching, room schedules, and download controls work without squeezing a full desktop editor into the screen |
| UX-04 | Offer keyboard operation and non-drag alternatives | Controls have visible focus, labeled fields, understandable errors, and numerical editing; canvas-only information has an object/room-list equivalent |
| UX-05 | Avoid color-only status and excessive motion | Issues include text and symbols; walkthrough has a clear exit, reduced-motion preferences, and a camera-reset option |
| UX-06 | Preserve context during errors and authentication expiry | Unsaved work is recoverable; the app explains the failed action and provides a retry or reauthentication path |
| UX-07 | Provide a guided first project and undoable practice | The user can reset or leave the example without modifying real work; the tutorial includes a multi-storey example after the initial drawing steps |
| UX-08 | Keep important units and assumptions next to values | Dimensions show their units and measurement reference; a default or AI-assumed value cannot look identical to a confirmed site measurement |

Use the accessibility practices in [WCAG 2.2](https://www.w3.org/TR/WCAG22/) as the target for application controls, forms, focus, contrast, and alternatives. Canvas and 3D interactions require their own usability checks rather than an unsupported claim that a library automatically makes the editor accessible.

## 12 Laravel architecture proposal

Laravel is the selected application framework. The proposed architecture is a Laravel monolith with a browser-based interactive editor, rather than separate applications for identity, geometry, and project management. Geometry and rendering still require client-side JavaScript; pointer movement must not wait for server requests.

### Technology responsibilities

| Layer | Proposed choice | Responsibility and decision basis |
| --- | --- | --- |
| Application framework | Laravel 13 with a supported PHP runtime | Accounts, authorization, project APIs, validation, persistence, jobs, and administration. Laravel 13 requires PHP 8.3 or later within its supported matrix; recheck dependency compatibility at implementation. [Release policy](https://laravel.com/docs/13.x/releases) |
| Web UI | Vue with TypeScript and Inertia, using Laravel's Vue starter kit | Application screens and editor state live alongside Laravel routes/controllers. The official kit provides this integration; this is a proposed frontend choice, not a user-selected requirement. [Starter kits](https://laravel.com/docs/13.x/starter-kits) |
| 2D drawing | SVG-based interaction/rendering prototype backed by a dedicated geometry module | Clear vector dimensions, hit testing, selection, snapping, and export. A canvas renderer may replace SVG if the same contract and performance targets pass; this choice needs a prototype benchmark |
| 3D viewing | Three.js | Derived meshes, cameras, picking, materials, orbit, and walkthrough presentation. Its documented cameras and geometry primitives support the proposed rendering layer; collision/path behavior is application work. [Three.js documentation](https://threejs.org/docs/) |
| Authoritative data | PostgreSQL with structured revision documents and relational project/access metadata | Preserve immutable snapshots, transactional revision updates, and searchable project records; this is a design choice to validate against hosting |
| Background work | Laravel queues with Redis in production | AI generation, large exports, thumbnails, and conversion jobs need visible states, bounded retries, and restart recovery. [Queue documentation](https://laravel.com/docs/13.x/queues) |
| Files | Private object storage through Laravel's filesystem abstraction | Reference files, assets, and exports require authorized access and project ownership. [Filesystem documentation](https://laravel.com/docs/13.x/filesystem) |
| Access control | Laravel session authentication and project policies | A same-origin application uses secure sessions; policies authorize project operations and background outputs. [Authorization documentation](https://laravel.com/docs/13.x/authorization) |
| AI | Provider adapter behind Laravel services | Keep credentials, budgets, request context, and provider changes server-side. Evaluate the first-party AI SDK and provider capabilities during the prototype; no model is selected in this PRD |

PHP and JavaScript package versions, 3D asset formats, PDF renderer, DXF library, and hosting provider remain implementation decisions with compatibility and license checks. A managed identity service, microservices, vector database, and public API are not prerequisites for the initial workflow.

### Responsibility boundaries

- **Browser editor:** immediate commands, draft state, snapping, 2D interaction, 3D presentation, local recovery, and preview validation.
- **Geometry domain:** units, topology, room boundaries, floors, stairs, openings, transformations, and deterministic checks. It is independent of the UI and AI wording.
- **Laravel services:** identity, permissions, authoritative schema/domain validation, transactional revision acceptance, export/share access, and job orchestration.
- **Background workers:** process an immutable revision or proposal request, persist job state/results, and apply time/cost limits.
- **AI provider:** interprets requests and proposes structured intent/geometry. It does not approve plans or write arbitrary server code.
- **Storage:** retain project snapshots and private artifacts with their source revision and access context.

If the layout solver needs a worker/service outside PHP, Laravel remains the application framework and orchestrator. That implementation is a prototype decision; it must preserve the same validation contract and access boundaries.

### Required service behaviors

| Capability | Required contract |
| --- | --- |
| Read/save a plan | Requests identify the project, schema version, and base revision; permission and schema/domain validation occur before transactional acceptance |
| Optimistic concurrency | A stale base revision returns a conflict with the current revision ID; the user can duplicate/reload/reconcile instead of overwriting |
| Generate or revise | A request identifies an immutable base revision and brief; a persisted job returns a proposal that requires explicit acceptance |
| Accept a proposal | Permission, current base revision, geometry checks, and an idempotency key are verified before creating a new revision |
| Create an export | Capture the exact revision, view/floors, dimensions, scale, and rendering options at submission |
| Share a revision | Grant narrowly scoped, revocable access with expiry and permitted downloads; never expose a general storage directory |
| Fetch job status | Only an authorized project user can read results; status persists across page refresh and worker restarts |
| Import a project | Parse supported data under resource limits, validate in a new draft, and preview changes before replacing any working plan |

### Persistence and conflict policy

The server's latest accepted revision is authoritative. The browser may hold an incomplete draft and a recovery copy, but local data must not be presented as a server save. Save accepted commands after a short idle interval and offer a manual save at any time.

Commands include the base revision. A successful save creates a new immutable revision and updates the project head atomically. On conflict, keep both the local draft and the remote revision available. The first release may use explicit reconciliation rather than automatic merging, provided no work is silently discarded.

The same policy applies to AI proposals, restored revisions, and future collaborators. Restoring an earlier revision creates a new head; it does not delete later history. An export or share points to a specific revision, even if the working head changes.

## 13 Data model and geometry rules

### Main entities

| Entity | Essential information |
| --- | --- |
| User | Identity, display name, language, units, access state, and preferences |
| Project | Owner, title, private metadata, current plan head, status, and optional client |
| Project membership | User, project, permissions, invitation state, and revocation |
| Site | Lot polygon, buildable envelope, exclusions, front, north, location, units, and provenance |
| Plan document | Schema version, model metadata, floors, assumptions, and validation status |
| Plan revision | Immutable document, parent/base revision, author, creation time, content digest, and change summary |
| Floor | Stable ID, name, elevation, ceiling/floor settings, geometry, voids, and visibility settings |
| Wall and connection | Endpoints/topology, floor, thickness, height, material references, and lock state |
| Room | Stable label/type linked to a derived enclosure, clear floor area, and adjacency |
| Opening | Wall reference, along-wall offset, size, vertical position, door/window type, and swing parameters |
| Stair | Source/target floors, footprint, landings, rise/run/tread information, headroom assumptions, and travel path |
| Furniture or fixture | Catalog/source version, floor, transform, exact dimensions, material, and clearance guides |
| Roof and vertical feature | Supported roof parameters, elevations, ceilings, voids, and source of assumed values |
| Dimension and note | Referenced objects, measured convention, display placement, text, and revision context |
| Asset/template | Definition, unit scale, thumbnail, source/license, version, ownership, and supported capabilities |
| AI request and proposal | Input brief, base revision, provider/model, budgets, state, candidate geometry, validations, and acceptance |
| Validation issue | Rule/version, object/floor, severity, location, explanation, and resolution state |
| Export job | Revision, floors/view, format, scale/options, status, output, and expiry |
| Share grant | Revision, owner, scoped access, token digest, expiry, revocation, and allowed downloads |
| Review/comment | Revision, author, anchor, view context, reply history, and decision/status |
| Audit event | Actor, permission-sensitive action, project, revision, time, and safe diagnostic context |
| Operations records | Later-phase estimates, quotation revisions, rates, expenses, purchases, stock, workers, tasks, and photo reports |

The final schema may group plan entities inside an immutable revision document. Data that needs transactional access control, job management, audit, or independent searching should have explicit relational records. This table defines product relationships rather than database migrations.

### Geometry invariants

| ID | Rule | Verification requirement |
| --- | --- | --- |
| GEO-01 | World-space horizontal coordinates use a documented X/Z plane; height uses Y | Canvas pixels, view zoom, camera position, and exploded-floor offsets are separate transforms and never modify actual coordinates |
| GEO-02 | Persisted lengths use integer millimetres with explicit precision rules | Conversion/rounding happens at defined input boundaries; save/load without edits preserves the canonical geometry digest |
| GEO-03 | Walls share a topology and rooms derive from valid enclosures | Adjacent rooms do not create duplicated wall quantities; split/merge operations preserve or explicitly remap identities |
| GEO-04 | Openings belong to a wall and a floor | Along-wall extents and vertical bounds fit the host wall; openings cannot remain orphaned after a deletion or floor change |
| GEO-05 | Room area uses the clear internal finished boundary; gross footprint uses the exterior wall boundary | The UI, schedule, and export name the area convention; voids and stairs have explicit inclusion/exclusion rules |
| GEO-06 | Floors and vertical connections agree | Model elevations, stair endpoints, landings, and floor openings are mutually consistent; a valid multi-storey concept has reachable modeled floor connections |
| GEO-07 | Interior/footprint geometry respects the configured envelope | Validation uses wall faces/thickness rather than centerlines alone; an unknown legal restriction remains unknown |
| GEO-08 | Dimensioned geometry is consistent across saved views and exports | A known length differs by no more than 1 mm and an area by no more than 0.01 m² after conversion/display rounding in reference fixtures |
| GEO-09 | Placement checks have a documented tolerance and rule provenance | Valid contact, true overlap, door sweep, and optional clearance are distinguished; no arbitrary code minimum is embedded without a validated rule source |
| GEO-10 | Derived presentation is disposable and reproducible | Deleting cached meshes or thumbnails cannot delete the source plan; regenerating views uses the same revision and parameters |

Derived room polygons, stair travel paths, area conventions for voids, and rounding tolerances need a written geometry contract before implementation is considered ready for feature development. The contract must cover single- and multi-storey fixtures, including diagonal walls and split/merged rooms.

### Validation states

- **Draft/incomplete:** schema is readable, but a boundary, connection, or critical dimension is unresolved. Saving preserves the work with issues attached.
- **Geometry checked:** implemented geometric checks pass for the supported model. This label says nothing about structural safety or legal compliance.
- **Needs attention:** a formerly checked design has new conflicts, missing assumptions, or stale dependent outputs.

AI acceptance and exports presented as completed concept proposals require all blocking geometry issues to be resolved. A user may deliberately export an incomplete draft only when it is labeled as such and includes the unresolved issues. Invalid numeric data, unsafe imports, and unsupported schema versions are rejected rather than saved as ordinary incomplete geometry.

## 14 Reliability security and performance

### Reliability and recovery

| ID | Requirement | Proposed acceptance target |
| --- | --- | --- |
| REL-01 | Preserve acknowledged server saves | Acknowledged revisions survive application/worker restarts; save transactions do not expose partially written plan state |
| REL-02 | Maintain a recoverable local draft | When browser storage is available, committed local edits update the recovery copy; users are told when storage is unavailable or a draft has not reached the server |
| REL-03 | Recover background work safely | Jobs persist their source revision, status, attempts, and result; retry after worker restart does not duplicate proposal acceptance, exports, or financial records |
| REL-04 | Make dependent output freshness visible | Changes mark older exports, thumbnails, simulation reports, and later estimates as belonging to a previous revision |
| REL-05 | Back up and restore durable data | Proposed pilot target: backups at least daily, recovery point within 24 hours, and restore within 4 hours; verify with a representative restore exercise before launch |
| REL-06 | Handle provider and network failure | Manual editing, saving when connected, and existing project access continue through an AI outage; retry/cancel states preserve the brief and last valid plan |

### Security and privacy

| ID | Requirement | Acceptance criteria |
| --- | --- | --- |
| SEC-01 | Keep house plans and uploads private by default | Authentication/project permissions protect plan content, assets, job results, and exports, including direct object access attempts |
| SEC-02 | Protect sessions and state-changing requests | Production uses HTTPS, secure session settings, request-forgery protection, login throttling, and server-side input validation |
| SEC-03 | Treat links as access grants | Share tokens are unguessable, stored as digests, scoped to revisions/capabilities, expiring, and revocable; links are not listed publicly |
| SEC-04 | Recheck authorization for downloads | Signed storage URLs have short expiry; revocation blocks new URLs immediately, and the UI explains that already downloaded copies cannot be recalled |
| SEC-05 | Validate uploaded and imported files | Enforce type, size, parse time, entity counts, archive expansion, and path limits; sanitize SVG/text and reject executable or external-resource payloads |
| SEC-06 | Protect AI and external-service boundaries | Secrets remain server-side; prompts, attachments, and model output cannot execute shell/code, fetch arbitrary internal URLs, or change account permissions |
| SEC-07 | Minimize provider disclosure | Users can see the AI data-use setting; raw site/client/worker information is excluded unless needed and permitted by the configured workflow |
| SEC-08 | Keep operational logs useful and restrained | Record correlation IDs, timings, revision IDs, rule failures, and job state; avoid full private drawings, unredacted tokens, and unnecessary personal information |
| SEC-09 | Audit sensitive actions | Access grants, deletion, professional-review records, provider configuration, support access, and quota changes record actor and time |
| SEC-10 | Enforce resource and spending limits | Project/file/entity limits, job concurrency, provider usage, and export limits are configurable and return understandable errors |
| SEC-11 | Define retention and deletion | Proposed pilot policy: deleted projects are recoverable for 30 days; active shares stop immediately; backup copies age out within a documented maximum of 30 days; temporary outputs expire by policy |
| SEC-12 | Separate product source from user project storage | GitHub stores application code and product documentation; private house plans, API keys, receipts, and customer files are not committed to the repository |

Retention periods are proposed product settings that must be confirmed against actual hosting and provider behavior. Do not offer deletion promises that cannot be honored by those services. A user-triggered account export must not include another owner's project solely because the user once had review access.

### Performance targets

These are initial engineering targets for evaluation, not measured results. The prototype must name the actual laptop/desktop, browser versions, GPU capability, and network used for acceptance. Test 3D on a midrange integrated-GPU machine as well as a stronger development machine.

The reference project contains three storeys, approximately 300 wall segments, 60 enclosed rooms, 120 openings, and 300 furniture/fixture objects across the building, using optimized catalog assets. It is a benchmark fixture, not a promised upper product limit.

| ID | Operation | Initial target |
| --- | --- | --- |
| PERF-01 | Local select, move, dimension edit, and undo feedback | 95th percentile visible response within 100 ms on the agreed reference laptop |
| PERF-02 | Update the active derived 3D view after a committed edit | 95th percentile within 500 ms; expensive mesh updates can show progress without freezing input |
| PERF-03 | Navigate dollhouse and walkthrough | Aim for at least 30 frames per second with the reference project and declared quality settings |
| PERF-04 | Open the reference project from the server | Usable 2D plan within 5 seconds over a test connection of at least 10 Mbps and no more than 150 ms round-trip latency; detailed assets may load progressively |
| PERF-05 | Autosave a connected draft | Start after approximately 2 seconds of idle time; 95th percentile server acknowledgement within 5 seconds under the declared test load |
| PERF-06 | AI generation | Proposed total job budget of 120 seconds with visible progress, cancellation, bounded repair attempts, and a configured provider-spend limit |
| PERF-07 | Export a standard multi-floor PDF | Complete within 30 seconds for the reference project or provide a clear background-job status and a recoverable failure |
| PERF-08 | Remain usable under constrained graphics | Lower asset/texture quality or use 2D fallback without losing editing, saving, or access to existing revisions |

Record usage and outcomes without collecting private plan content unnecessarily: project creation, first saved plan, selected views, accepted/rejected AI candidates, validation failures, export completion, save failures/conflicts, and recovery success. The administrator can review aggregated usage and job/model spend.

## 15 Acceptance scenarios and release evidence

Each implemented requirement needs evidence at its own scope. Geometry unit checks alone cannot establish a usable walkthrough, and a successful AI response cannot establish a valid plan.

| Scenario | Required outcome | Main requirement coverage |
| --- | --- | --- |
| AT-01 Account and private project | An invited user signs in, creates a project, and a second unrelated account is denied all reads/writes/downloads | ACC, PRJ, SEC |
| AT-02 Measured single-storey drawing | Draw a known-dimension house manually, add openings and furniture, change a dimension, and compare displayed/exported measurements | SITE, EDT, MOD, GEO |
| AT-03 Multi-storey drawing | Create two- and three-storey examples with straight/L/U stairs, landings, voids, and explicit elevations; validate floor relationships | FLR, EDT, MOD, GEO |
| AT-04 Feasible AI briefs | Evaluate 10 agreed feasible briefs including single-, two-, and three-storey houses and both launch request languages; candidates are editable and geometrically checked | AI, SITE, FLR |
| AT-05 Infeasible or ambiguous AI brief | Missing dimensions, conflicting locked geometry, and insufficient area produce clarification or infeasibility; requested floors/rooms are not silently removed | AI, SITE |
| AT-06 Accept reject and undo | Preview an AI change, reject it, accept a new one, undo, and redo; object identities and all views remain consistent | AI, EDT, MOD |
| AT-07 Connected view consistency | Modify a wall, opening, floor height, and stair; inspect 2D, dollhouse, exterior presets, and walkthrough against the same revision | VIEW, FLR, MOD, GEO |
| AT-08 Continuous walkthrough | Enter on the ground floor, move through doors and supported stairs to upper floors, change view, and exit safely; closed barriers block normal walking | VIEW, FLR, SIM |
| AT-09 Furniture and door checks | Demonstrate an oversized item, a blocked swing, and a narrow configured passage; locate the issues in 2D and 3D | SIM, EDT, GEO |
| AT-10 Unknown assumptions | Open a model with assumed heights/roof/stair inputs and unspecified site restrictions; the user can identify and correct them before handoff | SITE, MOD, FLR, SIM, IO |
| AT-11 Loss of connection and stale tab | Save, disconnect, edit, reload, reconnect, and introduce a concurrent edit; recover drafts and resolve the conflict without silent overwrite | PRJ, REL, MOD |
| AT-12 Revision restoration | Save named revisions, restore an earlier one, and confirm newer history and prior shared revisions remain intact | PRJ, REV, MOD |
| AT-13 Export and native round-trip | Export selected floors to PDF, PNG/SVG, and native data; reimport into a new project and verify geometry, scale, assumptions, and identities | IO, FLR, GEO |
| AT-14 Share and revoke | A reviewer opens only the granted revision, switches permitted views, and downloads only allowed files; expiry/revocation stops new access | REV, ACC, SEC |
| AT-15 Job retry and AI outage | Restart an export worker, retry a failed generation, and cancel a request; no duplicate acceptance occurs and manual editing remains usable | AI, IO, ADM, REL |
| AT-16 Unsupported graphics or import | Lose the GPU context and submit unsafe/unsupported project data; preserve the existing project and provide a usable fallback/error | VIEW, IO, SEC, REL |
| AT-17 Primary-user usability | The father completes the independent editing, viewing, and sharing tasks in section 2 on his laptop/desktop | UX, EDT, VIEW, PRJ, IO |
| AT-18 Capacity and recovery | Run the declared performance fixture and restore a backup; publish measured capacity and recovery behavior | PERF, REL, ADM |
| AT-19 Extended design phase | Verify DXF dimensions, calibrated tracing, uncertain conversion, comments tied to revisions, sunlight inputs, and reviewed Ilokano copy before shipping the corresponding P2 features | IO, REV, VIEW, SIM, EXT |
| AT-20 Contractor operations phase | Reconcile plan quantities, quotation revisions, expenses, deliveries, worker payments, and progress reports; changed geometry must not rewrite issued/paid records | OPS, MOD, SEC |
| AT-21 Account lifecycle and retention | Export the owner's account data, delete/archive a project, and simulate expiry/retention; another owner's data and revoked shares remain excluded | ACC, PRJ, SEC, REL |
| AT-22 Catalog and administrator controls | Update a template/catalog version and adjust a job quota; existing saved assets remain unchanged and sensitive actions are audited | ADM, MOD, SEC |
| AT-23 Accessible editing and recovery | Complete key edits through numerical controls and keyboard navigation, change display units/language, and recover from session expiry without losing work | UX, ACC, GEO, REL |

### P1 release checklist

- All P1 requirements in ACC, PRJ, SITE, EDT, MOD, AI, FLR, ADM, UX, GEO, REL, SEC, and PERF have their acceptance evidence recorded.
- P1 coverage also includes VIEW-01 through VIEW-08; SIM-01 through SIM-05 and SIM-07's shareable walkthrough; IO-01 through IO-06 and IO-10; and REV-01 through REV-02.
- Single-, two-, and three-storey journeys pass, including generated floor plans, supported stairs, and cross-floor walkthroughs.
- Golden geometry fixtures cover exact dimensions, shared walls, openings at limits, diagonals, room splits/merges, floor voids, stair connections, and unit conversions.
- Relevant server and browser checks agree on accepted/rejected fixture cases and source revision identity.
- End-to-end checks cover ownership, save conflicts, actual persistence, exports, sharing/revocation, retries, and fallback states.
- Geometry/security invariants pass all fixtures. For AI's bounded-success target, document the accepted failure cases and confirm they fail safely.
- The primary user completes the defined usability tasks, and critical confusion/data-loss issues are resolved.
- Hosting, backups/restoration, provider-spend limits, asset licenses, retention settings, and supported browser/device capacity are recorded.
- Exported/shared plans identify concept status and assumptions and include the professional-review handoff.

P2 and P3 have their own feature acceptance gates before those capabilities are advertised. Their delivery is not a prerequisite for the defined P1 release, but they remain part of this full product specification.

## 16 Delivery plan and dependencies

No calendar commitment is implied by these stages. Implementation starts only after the user authorizes application development.

| Stage | Deliverable | Dependency and exit gate |
| --- | --- | --- |
| D0 Product definition | PRD, confirmed scope, open product decisions, and repository documentation | Documentation review; current work stops at this stage |
| D1 Technical validation | Geometry contract, multi-floor/stair model, browser interaction prototype, AI output evaluation, export proof, and performance baseline | Demonstrate consistent single- and multi-storey geometry in 2D/3D before relying on a rendering or solver choice |
| D2 Laravel foundation | Accounts, project authorization, revisions, storage, job records, recovery, and deployment foundation | Private access, durable saves, stale-write rejection, and recoverable job tests pass |
| D3 Manual planner | Site/brief setup, core editor, dimensioned catalog, floors, stairs/voids, and validation | Manual single- and multi-storey reference fixtures pass without AI |
| D4 Connected visualization | Dollhouse, exterior, cross-floor walkthrough, spatial checks, and basic finishes | All views reflect the same geometry and handle incomplete/unsupported details visibly |
| D5 AI and handoff | Generated candidates, preview/accept/reject, job limits, PDF/native exports, and read-only review links | AI and export/share scenarios pass against immutable revisions |
| D6 P1 pilot | Complete laptop/desktop workflow with the father | The release checklist passes and the confirmed first-release scope is demonstrated |
| D7 P2 expansion | Richer design, interoperability, touch, languages, and collaboration | Accept each feature against its own traced requirements |
| D8 P3 expansion | Contractor financial and project-administration tools | Validate quantities/rates and reconcile operational records before use |

The critical dependency chain is geometry and persistence before derived 3D, followed by validated AI proposals and reliable handoff. A polished render does not close the geometry, multi-storey, or save/recovery gates.

## 17 Product decisions and open questions

### Recorded decisions

| Decision | Status | Rationale |
| --- | --- | --- |
| Product name Patakder | Confirmed by user | Ilokano identity connected to the father's construction work |
| Repository will3452/patakder | Confirmed by user | Shared home for future source and product documentation |
| Web application with Laravel | Confirmed by user | Selected framework for the product |
| Laptop/desktop as the main drawing device | Confirmed by user | The primary editing workflow targets this device class |
| Both single- and multi-storey houses in the first release | Confirmed by user | Floor/stair functionality is a launch requirement, not a deferred feature |
| Simple CAD-style editing with multiple views and simulation | Confirmed product direction | Combines manual editing, AI assistance, 3D exploration, and practical spatial checks |
| Documentation before application code | Confirmed by user | The current work produces a PRD without starting implementation |
| Vue/TypeScript/Inertia and Three.js | Proposed technical choices | Suit the interactive-browser architecture; validate through D1 |
| Metric geometry and private pilot | Planning assumptions | Match the initial use case; remain reversible product decisions |

### Decisions to resolve before the relevant implementation

These questions do not block completion of the PRD. They identify choices that would otherwise become hidden implementation assumptions.

1. What laptop/browser does the father actually use, and what level of camera/drawing interaction is comfortable for him?
2. Which real project examples and room/stair patterns should form the pilot fixtures? What maximum building/floor capacity should be advertised after validation?
3. Is Ilokano required for the first launch interface or instructions, or should its translation/evaluation remain P2? Who can review the terminology?
4. Which jurisdictions and professionally supplied site/rule templates should the pilot use? Which inputs will remain explicitly unknown?
5. Which AI provider, data terms, per-project budget, and monthly usage limit are acceptable?
6. Which hosting environment supports the agreed Laravel/PHP versions, PostgreSQL, private storage, queues, and tested backups?
7. Which professional or intended CAD application should validate the handoff and the later DXF subset?
8. Which contractor operations, if any, should move forward after the design workflow works? What measured sources/rates will support estimates?
9. What retention and account-deletion settings can the chosen infrastructure actually honor?

## 18 Main risks and mitigations

| Risk | Product response |
| --- | --- |
| AI produces attractive but inconsistent layouts | Require editable structured output, deterministic geometry validation, visible assumptions, bounded retries, and explicit acceptance |
| Multi-storey connections look plausible but do not join | Validate floor elevations, stair paths, openings, and walk connectivity with multi-floor fixtures before launch |
| 2D and 3D drift apart | Derive both from the same revision and object IDs; compare geometry and dimensions across views and exports |
| Traditional CAD complexity returns through too many controls | Prioritize common tools and numerical alternatives, use contextual panels, and test independent tasks with the father |
| Weak laptop graphics make the product unusable | Benchmark integrated graphics, optimize assets, lower visual quality, and preserve useful 2D fallbacks |
| Saved work is lost or overwritten | Use immutable revisions, transactional saves, local recovery, explicit conflict handling, and tested restoration |
| Concept plans are mistaken for construction approval | Keep status and assumptions visible and provide a professional-review handoff |
| CAD import/export expectations exceed actual support | Name the supported format/entity subset, show conversion losses, and validate reference files in the intended receiving application |
| Uncontrolled AI or asset costs | Enforce quotas, spending/time limits, licensed catalogs, usage visibility, and manual workflows during outages |
| Broad contractor features delay the core planner | Keep the full functional inventory while delivering the confirmed P1 design workflow first; promote later modules through explicit scope decisions |

## 19 Requirements inventory and document maintenance

Requirement prefixes group the full functional scope: ACC accounts, PRJ projects, SITE brief/site, EDT drawing, MOD shared model, AI generation, VIEW visualization, SIM spatial simulation, IO import/export, REV review, ADM administration, FLR multi-storey design, EXT extended design/language, OPS contractor operations, UX usability, GEO geometry invariants, REL recovery, SEC privacy/security, and PERF performance.

The requirement ID is the stable reference for future issues and acceptance evidence. Changes to priority or acceptance criteria must update the phase inventory and release checklist together. Implementation tickets should name the applicable requirements and demonstrate their observable outcomes.

The next product review should confirm the proposed choices and resolve the questions needed for D1. Completing this documentation does not authorize application scaffolding, dependencies, or production deployment.


