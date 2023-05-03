<!--
This file is part of OpenScan-Hardware
Copyright 2023 OpenScan Contributors
SPDX-License-Identifier: CC-BY-SA-4.0
-->

# Conventions for documenting optical breadboard assemblies

## Introduction

The goal of this part is to propose conventions and guidelines for documenting
optics and photonics systems built on an optical breadboard (metal support with
a grid of mounting holes), in a manner useful for sharing the design as open
source hardware.

The goal of [open source hardware](https://www.oshwa.org/definition/) is to
share designs so that not only can they be studied and replicated, but also
modified and those modified designs redistributed. In basic research settings
(by which this document is motivated), we can hope that this will enable newly
developed techniques, as well as implementations of existing ones, to be shared
between labs. Furthermore, given a full description of the design, it should be
possible for specific improvements or extensions to be added and re-shared,
without the need to duplicate the work of documenting the whole system from
scratch.

For a design to be open source hardware, it is important that the "source" or
design files—the preferred format for making modifications—are shared
{cite}`OSHWSoP`. In fields such as electronics and mechanical devices, the
"source" is usually the design files in the form of CAD (computer-aided design)
files. In the case of electronics, both the schematic (circuit diagram) and the
PCB (printed circuit board) layout are important, because both the logical
design and the physical realization are needed to facilitate replication,
troubleshooting, modification, and extension.

Here we are interested in applying these principles to _prototype_ optical and
photonic systems, especially related to microscopy. By "prototype", we
generally mean systems built on optical breadboards, with or without a
microscope body. This is in contrast to many successful open source hardware
microscope projects {cite}`Eisenstein2021` which aim either to provide a
modular platform (TODO Cite) or to provide a tightly refined and/or low-cost
design for a particular technique (TODO Cite).

TODO: Such projects could be used in combination with our approach here for the
breadboard portion, as the microscope body

We believed that there is a niche for a complementary approach where
off-the-shelf parts[^ots-oshw] are preferred whereever possible, and the
overall design is optimized for straightforward layout and ease of future
changes—i.e., how a researcher in a lab most typically would set up a
particular microscopy technique from scratch. This is usually the format in
which new microscopy techniques are prototyped and developed, and we believe
there is a need for an easier way to share such designs _as is_ without the
need for extra effort to turn it into a polished product or to plug into a
particular mechanical interface.

The challenge here is that there is no obvious equivalent to CAD files for
these builds. We could imagine using mechanical CAD, but mechanical CAD cannot
capture the optical relationship between components; yet it frequently
overspecifies incidnetal mechanical details. Optical CAD programs, on the other
hand, can model optical elements and systems, but (to this writer's knowledge)
are not geared toward high-level specification of entire breadboard builds.

In the absence of a systematic method to describe optical breadboard builds,
one approach that is sometimes taken is to provide detailed step-by-step
instructions for assembly and alignment of the whole build. While this can be
very useful when done well, it often suffers from brittlenness arising from the
linear nature of such instructions that do not reflect the structure of the
principles by which the the system operates: if anything in the system is
changed, the whole sequence of instructions may need to be reviewed and updated
where necessary.

In the following, we propose to separate the description of components from the
description of their optical interrelationships and alignment methods. We hope
that this approach can facilitate the documentation of optical breadboard
builds in a modular fashion, more amenable to reuse and modification. In a
second part, a concrete example is (to be) given for a basic two-photon
scanning microscope.

TODO: This is a living document; contributions are welcome (need contributor
guide).

## On modularity

Modularity is sometimes confused with being composed of components that present
specifically engineered interfaces that plug into a system, in the manner of
Lego blocks or Tinkertoy sets. That is one useful way to achieve
interchangeable subsystems, but comes at a cost: any new component must first
be reengineered to work with the specific interface before it is integrated.
This can be a hurdle when prototyping a technique.

TODO: Term this "stackable" design? See if it well describes existing
approaches.

A different tradeoff can be made by consciously making use of what might be
called _universal interfaces_: interfaces that are simple and so widely used
that almost anything can connect to. Examples in software include simple text
files that list one item per line (see also the so-called
[Unix philosophy](https://en.wikipedia.org/wiki/Unix_philosophy)). Examples in
electronics include 5 V TTL signals, plain analog voltages, 50 Ω transmission
lines, and the like. Slightly more specific interfaces (software: NumPy arrays;
electronics: serial communication lines) can also be regarded effectively
universal, depending on the context.

What these universal interfaces have in common is that while they require a
little more expertise on the part of the user than solutions intended for "end
users", they offer vastly more power and flexibility. They serve as a lingua
franca for modules, allowing components to be combined even if they were not
explicitly designed to work together.

Optical systems have a universal interface: the light beam and its geometry.
The exact properties of the beam (for example, polarization) that need to be
specified depends on the context, but it is the light beam itself that serves
as the interface between components—not any mechanical interconnect per
se[^obvious].

We believe that modular description is possible with free-space optics on an
optical breadboard. While this requires more knowledge on the part of the
reader than with components that snap or screw together, and more thought on
the part of the writer, it greatly reduces the amount of nonessential
engineering required and keeps the design maximally flexible for modifications
and extensions.

### Why we need modular descriptions

- Easier to understand when the interrelationship between components is made
  explicit
- Changing a specific component or aspect of the system is less likely to
  require changes to the whole
  - Especially helpful in collaboration, GitHub pull requests
- Clarifies required skills and knowledge

### Separation of concerns

- Avoid intermingling overall design and layout considerations with placement
  and alignment of individual components

### Degrees of freedom

- Importance of thinking in terms of DoFs and making it explicit in build
  description
  - When the correct adjustments are provided, layout can be made flexible
    (e.g. add or remove mirrors)
- Collimated laser beam: position, orientation
  - But also polarization, beam diameter, etc.

### Practical tradeoffs

- Not all components need to have the best fine adjustment for aligning

### Optomechanical systems

- Free-space, cage, lens tubes

### Overall layout and design

- ...
- Breadboard coordinates (used in auxiliary, not primary, role)

## Alignment

- Explain common alignment procedures here so that they don't have to be
  explained in the actual build docs
- The possible variations may be quite large; prioritize those needed in our
  concrete build(s)

### Aligning a laser beam to the table

### Aligning a laser beam to an optical component

- Two mirrors and (usually) a power meter

### Aligning optical components to a laser beam

### Aligning with a periscope

### Aligning with a microscope body

- What are the different possibilities here? Keep it specific to tube lens for
  now?

## Thoughts on hardware beyond optics and photonics

```{bibliography}
```

[^ots-oshw]: Not to say that these off-the-shelf parts cannot themselves be open source
    hardware (in fact, that would be ideal). The point is to emphasize the
    high-level build over reinventing components when standard ones are available.

[^obvious]: If it sounds like this paragraph is stating the obvious, that's because it is.
    The point I'm trying to make is about a change of perspective: let's not allow
    the geometric relationships in the light path to get buried under details about
    all the mechanical and optomechanical components.
