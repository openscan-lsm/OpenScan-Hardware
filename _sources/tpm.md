<!--
This file is part of OpenScan-Hardware
Copyright 2023 OpenScan Contributors
SPDX-License-Identifier: CC-BY-SA-4.0
-->

# OpenScan two-photon microscope

Here we describe a concrete build for a scanning two-photon excitation
fluorescence microscope.

## Introduction

TODO: Choices made: use a commercial microscopy body (ergonomic for biologist
users, and simplifies integration with other imaging modalities). The
limitation is that you don't have full control over the optics (tube lenses).

## Overall layout

The overall layout of a simple two-photon microscope for fluorescence lifetime
imaging is as follows:

```{mermaid}
    flowchart LR
%%      subgraph Illumination ["Illumination beam line"]
      Laser["Ti:Sapphire Laser"] --> Isolator;
      Isolator["Isolator"] --> EOM;
      EOM("Electro-optic modulator") --> Shutter;
      Shutter --> Dump[Beam dump];
      EOM --> EOMDump[Beam dump];
      Shutter --> HWP;
      HWP("Half wave plate") --> QWP;
      QWP("Quarter wave plate")  --> Mirror1("Mirror M1");
      Mirror1_break("Mirror M1") --> Mirror2;
      Mirror2("Mirror M2") --> Expander;
      Expander("Beam Expander") --> Tap;
      Tap["Tap Beam Splitter"] --> Mirror3("Mirror M3");
      Mirror3 --> Periscope(("Periscope
      (M4, M5)"));
      Periscope_break(("Periscope
      (M4, M5)")) -->Galvos;
      Galvos("Galvo mirrors") -->Scan;
%%      end
      Scan("Scan Lens") --> EpiTube;
%%      subgraph MB ["Microscope base"];
      EpiTube("Tube Lens") --> EpiDM;
        EpiDM["Dichroic mirror"] <--> Objective;
        Objective("Obj") <--> Sample;
        EpiDM -->PMTTube;
%%      end
    PMTTube("Tube Lens") -->Filter;
%%      subgraph PMTA ["Camera line"]
      Filter("Filter");
          Filter -->PMT;
          PMT("PMT")-.-Datalines-.-BH("SPC boards");
%%      end
%% Subgraphs commented because including them causes the diagram to run bottom to top.

%% Links to main body sections
    click Laser href "#ultrafast-pulsed-laser" _blank
    click Isolator href "#optical-isolator" _blank
    click EOM href "#intensity-control-electro-optic-modulator-eom" _blank
    click Shutter href "#shutter-alignment" _blank
    click HWP href "#polarization-control-wave-plates" _blank
    click QWP href "#polarization-control-wave-plates" _blank
    click Mirror1 href "#mirrors-m1-m2" _blank
    click Mirror2 href "#mirrors-m1-m2" _blank
    click Expander href "#beam-expander" _blank
    click Periscope_break href "#mirror-m3-periscope-m4-m5" _blank
    click Periscope href "#mirror-m3-periscope-m4-m5" _blank
    click Galvos href "#Galvanometer-scanners" _blank
    click Scan href "#scan-lens" _blank
    click EpiTube href "#microscope-body" _blank
    click PMTTube "#photomultiplier-tubes" _blank
    click PMT href "#photomultiplier-tubes" _blank
    click BH href "#electronics" _blank
```

### Optical axes

#### Optical axis 1: laser control and modulation

Parallel to table at ~110 mm height. Established by Ti:Sapphire laser emission.

#### Optical axis 2: beam expander

Parallel to table at ~110 mm height. Determined by beam expander placement.
Aligned using mirrors M1 and M2.

#### Optical axis 3: galvanometer scanner input

Parallel to table. Determined by position of galvo scanners, which in turn
depends on optical axis 4 and its components. Aligned using mirrors M3 and M5.

#### Optical axis 4: microscope epi illumination path

Parallel to table. Established by microscope body placement.

### Overall alignment procedure

- Fix the components that establish optical axes and do not depend on
  alignment:
  - Ti:Sapphire laser (optical axis 1)
  - Beam expander (optical axis 2)
  - Microscope body (optical axis 4)
- Align components on optical axes 1 and 4.
- Co-align optical axes 1 and 2 using mirrors M1 and M2.
- Align galvanometer scanners to optical axis 4, establishing optical axis 3.
- Co-align optical axes 2 and 3 using mirrors M3 and M5.

## Photonic/optic components

### Optical table

4x8-foot, 8-inch thick, with vibration isolation (e.g., TMC). 1-inch-spaced
mounting holes tapped for 1/4-20.

TODO: More details; 8-inch hole if using bottom port.

- The optical table will very slightly deform when heavy items are placed on
  it, or when it is floated vs not floated. Alignment should therefore be done
  after fixing heavy components and floating the table.

### Ultrafast pulsed laser

| Component                     | Manufacturer    | Part No    |
| ----------------------------- | --------------- | ---------- |
| Mode-locked Ti:Sapphire laser | Spectra-Physics | Mai Tai HP |

**BOM Notes:** Mode-locked Ti:Sapphire lasers broadly come in two categories:
turn-key systems with automatic control (Spectra-Physics Mai Tai or Coherent
Chameleon), and manually tunable optical oscillators (Spectra-Physics Tsunami
or Coherent Mira). The necessary pump laser is usually purchased as part of the
package.

#### Laser alignment

**Dependencies:** None.

**Outgoing beam:** Collimated laser beam with linear polarization at 0°
(horizontal).

We use the laser emission, as is, to define optical axis 1. It should be
parallel to the table at a height of about 110 mm. It is convenient to have the
laser be aligned to a row of mounting holes on the table, but this is not
essential.

### Optical isolator

| Component        | Manufacturer              | Part No |
| ---------------- | ------------------------- | ------- |
| Faraday isolator | Electro-Optics Technology | BB8-5i  |
| Isolator mount   |                           | TODO    |

**BOM Notes:** EOTech is now part of Coherent, but this optical isolator is
obsolete. It is an isolator designed for Ti:Sapphire lasers. The closest
equivalent current model is a Coherent EURYS Broadband Faraday Isolator with a
5 mm aperture.

#### Isolator alignment

**Dependencies:** Optical axis 1.

**Incoming beam:** Collimated laser beam with linear polarization at 0°
(horizontal).

**Outgoing beam:** Collimated laser beam with linear polarization at 90°
(vertical).

Align the device to the pre-existing optical axis 1. See
{ref}`align-eot-isolator`.

### Intensity control: electro-optic modulator (EOM)

The EOM (consisting of a Pockels cell together with a polarizing beam splitter)
provides electronic control over excitation laser power.

| Component       | Manufacturer | Part No | Name                                                                             |
| --------------- | ------------ | ------- | -------------------------------------------------------------------------------- |
| EOM             | Conoptics    | 350-80  | KD\*P Modulator                                                                  |
| EOM Mount       | Conoptics    | 102     |                                                                                  |
| Alignment tool  | Conoptics    | 103     |                                                                                  |
| EOM driver      | Conoptics    | 302RM   |                                                                                  |
| Beam dump       | Thorlabs     | LB1     | Beam Block, 400 nm - 2 µm, 10 W Max Avg. Power, Pulsed and CW, Includes TR3 Post |
| Beam dump mount |              |         | TODO                                                                             |

**BOM Notes:** The EOM should be configured for amplitude modulation, with
minimum transmission near zero bias voltage.

#### EOM alignment

**Dependencies:** Optical axis 1. Usually the upstream optical isolator should
be aligned first.

**Incoming beam:** Collimated laser beam with linear polarization at 90°
(vertical).

**Outgoing beam:** Collimated laser beam with linear polarization at 0°
(horizontal).

Align the device to the pre-existing optical axis 1. See
{ref}`align-eom-conoptics`.

#### EOM design notes

- For the EOM, Conoptics 350-80LA, which has a larger, 3.5 mm aperture, may be
  desirable. The 350-80 used here has a 2.7 mm aperture.
- The Conoptics EOMs can now be purchased with an integrated beam block.
- The EOM can be purchased with input polarizing beam splitter, in case the
  input beam does not have clean linear polarization. This is not necessary
  when the immediately previous component is the Faraday isolator (whose last
  component is a polarizer).
- The EOM must have sufficient aperture size for the beam diameter. See
  manufacturer manual for details.
- EOM drivers are available optimized for different applications, e.g., fast
  switching vs high extinction ratio. For routine power control in TPM, we do
  not need fast switching. The 302RM is fast enough for laser blanking during
  retrace (not yet implemented in OpenScan).
- The choice of an EOM over an AOM is advantageous for Ti:Sapph lasers which
  have a wide bandwidth (wavelength distribution). However, power control could
  also be achieved at significantly lower cost using the combination of a
  motorized half-wave plate and a polarizing beam splitter, albeit with slower
  switching.
- The EOM should be located before the main shutter in the beam path, so that
  it does not experience temparature shifts when the shutter is opened or
  closed.

### Shutter

| Component       | Manufacturer       | Part No       |
| --------------- | ------------------ | ------------- |
| Shutter         | Vincent (Uniblitz) | TBD (VS14?)   |
| Shutter mount   |                    | TBD           |
| Shutter driver  | Vincent (Uniblitz) | TBD (VCM-D1?) |
| Beam dump       | Thorlabs           | LB1           |
| Beam dump mount |                    | TBD           |

#### Shutter alignment

**Dependencies:** Optical axis 1.

Place such that beam passes through near the center of the aperture when the
shutter is open. Fix at ~5° from perpendicular to the optical axis, such that
the reflected beam (when the shutter is closed) is offset from the incoming
beam. Block the reflected beam with the beam dump.

### Polarization control: wave plates

A half-wave plate (HWP) followed by a quarter-wave plate (QWP) allows the
excitation beam to be precisely adjusted to have circular polarization at the
specimen.

```{admonition} How does this work?
The HWP allows the linear polarization of the incoming beam to be rotated to any
chosen angle. The QWP, when aligned, converts the linearly polarized beam to a
circularly polarized beam. It may therefore seem that the HWP is unnecessary.

In practice, even if the QWP produces perfectly circularly polarized light,
anisotropy in the optics between the QWP and the specimen will cause the beam to
end up elliptically polarized. We can compensate for this effect by adjusting
the QWP such that it emits a beam that is elliptically polarized in the
orthogonal direction, so that it will end up circularly polarized at the
specimen. However, in order to adjust the QWP in such a way, we need to be able
to supply it with a linearly polarized beam at an angle of our choosing, which
is why we need the HWP.

The wave plates can also be adjusted to obtain linearly polarized light for
applications such as fluorescence anisotropy measurements (the QWP is not
necessary for this).
```

| Component          | Manufacturer | Part No     | Name                                                                      |
| ------------------ | ------------ | ----------- | ------------------------------------------------------------------------- |
| Half-wave plate    | Thorlabs     | AHWP05M-980 | Ø1/2" Mounted Achromatic Half-Wave Plate, Ø1" Mount, 690 - 1200 nm        |
| HWP mount          | Thorlabs     | CRM1P       | Precision Cage Rotation Mount with Micrometer Drive, Ø1" Optics, 8-32 Tap |
| Quarter-wave plate | Thorlabs     | AQWP05M-980 | Ø1/2" Mounted Achromatic Quarter-Wave Plate, Ø1" Mount, 690 - 1200 nm     |
| QWP mount          | Thorlabs     | PRM1        | High-Precision Rotation Mount for Ø1" (25.4 mm) Optics                    |
| Posts and clamps   |              | TBD         |                                                                           |

**BOM Notes:** Thorlabs CRM1P is obsolete and superseded by CRM1PT "Precision
Cage Rotation Mount with Micrometer Drive, Ø1" Optics, 8-32 Tap".

#### Wave plate alignment

**Dependencies:** Optical axis 1. (Final polarization adjustment requires the
full system to be built so that specimens can be imaged.)

Place the two wave plates perpendicular to optical axis 1, centered around the
beam.

TODO: Back reflection must overlap with the beam (link to alignment section)

TODO: Obtaining circularly polarized light at the QWP (use PBS, power meter,
and mirror; link to alignment section)

TODO: Polarization adjustment procedure using giant unilamellar vesicles.

### Mirrors M1, M2

These mirrors affort beam steering to establish optical axis 2.

#### M1-M2 alignment

**Dependencies:** Optical axis 1 and optical axis 2. These mirrors co-align the
two optical axes.

See TODO (beam steering with two kinematic mirrors) and TODO (aligning a laser
beam into a beam expander).

### Beam expander

| Component           | Manufacturer   | Part No         | Name                        |
| ------------------- | -------------- | --------------- | --------------------------- |
| Beam expander       | Special Optics | 56-30-2-8X-1064 | Variable Zoom Beam Expander |
| Beam expander mount |                | TBD             |                             |

#### Beam expander alignment

**Dependencies:** None. Optical axis 2 will be determined by the beam expander.

TODO: Magnification adjustment to fill objective back aperture.

### Mirror M3; periscope M4-M5

#### M3-M5 design notes

- The Thorlabs periscope assembly has fine tilt adjustment screws only on the
  upper mirror, so we use M3 and M5 to steer the laser beam onto optical axis
  3\. Some periscopes (e.g. Newport) have fine tilt adjustment also on the lower
  mirror, in which case M3 is not necessary (adjustment can be done with the
  periscope mirrors M4 and M5).

#### M3-M5 alignment

**Dependencies:** Optical axis 2 and optical axis 3-4.

See TODO (beam steering with two kinematic mirrors) and TODO.

### Galvanometer scanners

TODO BOM. Mount should have XYZ translation (no need for micrometers)

#### Galvo alignment

**Dependencies:** Optical axis 4 and all its components.

TODO.

### Scan lens

TBD.

#### Scan lens design notes

- The scan lens should focus a collimated beam at the epi field diaphragm
  (slider slot) position in the LAPP main branch. This requires a scan lens
  with a focal length of at least ~120 mm (exact requirement for mechanical
  clearance may be slightly larger).

#### Scan lens alignment

**Dependencies:** Optical axis 4.

See TODO (aligning a scan lens using transmitted illumination).

### Microscope body

| Component                | Manufacturer      | Part No         |
| ------------------------ | ----------------- | --------------- |
| Inverted microscope body | Nikon Instruments | Eclipse Ti2-E/B |

TODO: LAPP details, fixing plate. Filter cube.

**BOM Notes:** The Ti2 can be configured with a large array of options. We use
the LAPP Main Branch in the epi illumination port; this includes the tube lens
for the excitation light. Details TBD.

#### Microscopy body alignment

**Dependencies:** None. Placement determines optical axis 4.

It is conveninet to fix the microscope so that its front-back center axis is
aligned to a row of mounting holes on the optical table.

### Emission beam splitter

TBD.

### Photomultiplier tubes

TODO.

## Electronics

TODO:

- NI DAQ for scan waveforms and PMT signals
- (optional) BH SPC for FLIM
- BH DCC-100 (or Hamamatsu) for PMT power supply and control
- Power supply for PMT preamps
- Control of shutter and EOM
