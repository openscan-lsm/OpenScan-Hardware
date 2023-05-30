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
design for a particular technique (TODO Cite). (Such projects can, or course,
be used in combination with our approach here, as one component of a larger
system.)

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

We want the optical systems we build to be modular and to be documented in a
fashion that clearly exhibits the modularity. In a modular system,

- It is easier to understand the whole system because each component is
  abstracted and the interrelationship between them is made explicit.
- Changing a specific component or aspect of the system is less likely to
  require changes to the whole. This is not only beneficial for initial
  development, but facilitates adoption/adaptation by others as well as open,
  collaborative development.
- The skills and knowledge required to assemble the system are easier to
  clarify.

Modularity is sometimes confused (in this writer's opinion) with being composed
of components that present specifically engineered interfaces that plug into a
system, in the manner of Lego blocks or Tinkertoy sets. That is one very useful
way to achieve interchangeable subsystems, but comes at a cost: any new
component must first be reengineered to work with the specific interface before
it is integrated. This can be a hurdle when prototyping a technique or when the
goal is not to develop a marketable product but to build an instrument that can
be used to collect data in your own lab.

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
users", they offer vastly more power and flexibility with much less engineering
overhead. They serve as a lingua franca for modules, allowing components to be
combined even if they were not explicitly designed to work together.

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

## Modular description of optical breadboard builds

How might we create such descriptions?

The basic idea is what any developer of an optical system would intuitively do:
decide what the major components of the system need to be, how they should be
laid out, how they need to be aligned and otherwise adjusted. This is usually
an iterative process, and too often only the final result gets documented (if
that), because there are so many moving parts and a lot of implicit knowledge
of their interrelationships go into aligning the system. We propose a style of
documentation that makes these interrelationships more explicit.

This involves separating out, or otherwise clearly indicating, several pieces
of information:

- Keep descriptions of individual components separated from overall layout.
- Keep exact component positions (if given) separated from the logical (block
  diagram) layout. The latter may be reused in builds that cannot exactly
  replicate the former due to various constraints.
- Indicate alignment goals separately from component identity and specs.
  Alignment goals describe the relationship of the component with others in the
  system, so this is a way of saying: separate interface from implementation.
- Keep alignment _instructions_ separated from component specs and placement.
  Whenever practical, these should be written so that they can be understood
  (and potentially reused) outside of the context of any particular build or
  design. Some alignment procedures will necessarily be specific to particular
  components (but preferably not the particular build); others can be more
  generic.
- Component descriptions should specify alignment dependencies, i.e., which
  other components or subsystems need to be aligned before aligning the
  component in question.

### Documenting alignment procedures

Alignment is the task of bringing each component into the correct (mainly
geometrical) relationship to all of the other components. It is therefore not
always easy or possible to talk about the alignment of a single component in
isolation. And because each alignment procedure, taken as a whole, defines the
relationship between components (which is a desired state, not a process), it
makes sense to treat the procedure as an independent unit.

We want to provide concrete [instructions](./align.md) for how to align optical
components, but we do not want to end up with a long set of instructions to be
blindly followed without an understanding the logic behind the procedure.

For this reason we want to make the following items clear for every alignment
procedure we describe:

- Alignment requires having a well-defined readout that is repeatable.
  - The readout can be a lot of different things, such as an optical power
    meter readout, or visual inspection of a laser beam being centered on an
    iris.
- The goal/target of the alignment procedure must be clearly defined in terms
  of the readout.
- Alignment procedures should indicate the degrees of freedom being
  adjusted/optimized and which of those require fine adjustments (and, in some
  cases, how fine) and/or independent adjustments (and why, including any known
  tradeoffs).
  - This is valuable information because fine and independent adjustments
    require more expensive components, yet may be less stable than
    non-adjustable mounts. In many cases, components without adjustment screws
    can be hand-aligned to ~0.1 mm precision if a good readout is available.
- The goal/target of the alignment must constrain all of the degrees of freedom
  being adjusted.
  - If this is not the case, the alignment could be "finished" and yet end up
    in many different states.
- Fine adjustments should be initialized to a neutral or centered position
  before coarse alignment or positioning. The method for this initialization
  should be noted.

## Thoughts on hardware beyond optics and photonics

(To be written.)

______________________________________________________________________

```{bibliography}
```

[^ots-oshw]: Not to say that these off-the-shelf parts cannot themselves be open source
    hardware (in fact, that would be ideal). The point is to emphasize the
    high-level build over reinventing components when standard ones are available.

[^obvious]: If it sounds like this paragraph is stating the obvious, that's because it is.
    The point I'm trying to make is about a change of perspective: let's not allow
    the geometric relationships in the light path to get buried under details about
    all the mechanical and optomechanical components.
