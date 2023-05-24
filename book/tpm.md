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

TODO Diagram (and photographs).

### Optical axes

#### Optical axis 1: laser control and modulation

Parallel to table at ~100 mm height. Established by Ti:Sapphire laser emission.

#### Optical axis 2: beam expander

Parallel to table at ~100 mm height. Determined by beam expander placement.
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
| Faraday isolator | Electro-Optics Technology | TBD     |
| Isolator mount   |                           | TODO    |

**BOM Notes:** This part is obsolete. Possible substitutes include products
from
[Coherent](https://www.coherent.com/components-accessories/rotators-isolators)
(which acquired EOTech),
[Conoptics](https://www.conoptics.com/optical-isolators/), or
[Thorlabs](https://www.thorlabs.com/navigation.cfm?guide_id=2015).

#### Isolator alignment

**Dependencies:** Optical axis 1.

**Incoming beam:** Collimated laser beam with linear polarization at 0°
(horizontal).

**Outgoing beam:** Collimated laser beam with linear polarization at 45°.

Align the device to the pre-existing optical axis 1. Maximize extinction with
the device reversed 180°. See {ref}`align-eot-isolator`.

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

**Incoming beam:** Collimated laser beam with linear polarization at 45°.

**Outgoing beam:** Collimated laser beam with linear polarization at 45°.

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
  (slider slot) position in the LAPP main branch. With a 110-mm focal length
  scan lens, its position should be near the back of the LAPP main branch.

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
