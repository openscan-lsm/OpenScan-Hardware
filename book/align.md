<!--
This file is part of OpenScan-Hardware
Copyright 2023 OpenScan Contributors
SPDX-License-Identifier: CC-BY-SA-4.0
-->

# Alignment procedures for optical breadboard assemblies

## Aligning optical components to a pre-existing laser beam path

### Aligning a Faraday isolator

TODO.

(align-eot-isolator)=

#### Aligning an EOTech Faraday isolator

TODO.

### Aligning an electro-optic modulator (EOM)

**Readout:** Laser power transmitted by the device.

**Alignment goal:** Maximum extinction ratio of laser power through the device.

**Degrees of freedom:** X, Y, pitch, yaw, roll

Note: Alignment procedure will likely differ depending on the manufacturer of
the device. For now we describe alignment for Conoptics EOMs. Further research
is needed to derive aspects of alignment (if any) that are common to other
EOMs.

(align-eom-conoptics)=

#### Aligning a Conoptics EOM

Here we describe how to align a Conoptics 350-80 EOM to a pre-existing optical
axis defined by a laser beam. We assume an EOM configured for amplitude
modulation and with minimal transmission near zero bias voltage.

Make sure to read the manufacturer manual first, as incorrect handling can
damage the EOM. The (linear) polarization angle of incoming laser beam must be
known, at least approximately.

- Set up a power meter on the output side of where the device will be placed.
  - With the device out of the beam path, align the power meter to the beam;
    note down the measurement.
- Place the alignment tool (Conoptics Model 103) on the EOM mount.
- **Initialize** the mount's alignment screws (see
  [mount instructions](conoptics-102-mount)).
- **Coarse-adjust and place** the mount such that the beam passes through the
  alignment tool; clamp down to table (see
  [mount instructions](conoptics-102-mount)).
  - For this step, it may be easier to place an alignment screen in front of
    the power meter and observe the beam profile.
  - Goal of coarse alignment: beam should be minimally clipped by the alignment
    tool.
- Using the power meter, **fine-adjust the X, Y, pitch, and yaw** for maximum
  power transmission (see [mount instructions](conoptics-102-mount)).
- With the laser off, swap the alignment tool for the actual EOM, noting
  correct direction (hole for rejected beam on the output end).
- **Roughly set the roll angle** of the device such that the crystal axes are
  at 45 degrees to the incoming laser's polarization angle.
  - The square crystal should be visible through the input aperture; the
    incoming laser polarization should match the diagonal of the crystal
- Set up a **beam dump** at about 30 mm away from the EOM rejected beam port.
  The rejected beam will be tilted by 22.5° toward the input end.
- With the driver turned off, connect the cables.
- Turn on the driver and set the bias voltage to zero.
- **Fine-adjust the roll** by iteratively optimizing for minimum transmission,
  alternating between adjusting the bias voltage (near zero) and adjusting the
  roll angle.
- **Fine-adjust the X, Y, pitch, and yaw** to maximize the extinction ratio,
  which is the ratio of the maximum and minimum transmission.
  - Do this by adjusting in small increments to reduce the minimum
    transmission, and checking that you have not reduced the maximum
    transmission.
- Iterate over the last two steps (fine adjustment of roll; X, Y, pitch, and
  yaw) until no further improvement can be made to the extinction ratio.

## Aligning a laser beam path to optical components

TODO: General: pair of mirrors.

### Aligning a laser beam to a beam expander

TODO.

## Apendix: Instructions for specific optomechanical mounts

(conoptics-102-mount)=

### Conoptics Model 102 EOM mount

TODO: Photograph.

This mount offers non-independent adjustments for the X, Y, pitch, and yaw. The
roll adjustment is independent but with no fine adjustment screw.

- Initialization
  - Set the 4 thumb screws to abut the cylinder radially (i.e., each normal to
    the cylinder surface). (TODO: good vs bad diagram)
  - Set the pitch angle close to zero: using a beam height ruler (or similar)
    to measure the height above the table of each end of the cylinder, ensure
    that the cylinder is parallel to the table to within ~0.2 mm.
- Coarse adjustment and positioning
  - The lab jack (bottom portion of the mount) Y adjustment is used for coarse
    positioning.
  - X and yaw coarse positioning done by sliding on table.
    - Clamp down once correctly positioned.
- Fine adjustment of X, Y, pitch, and yaw with the 4 thumb screws:
  - None of these screws are independent from the other 3, so iterative
    adjustment is necessary.
  - The two thumb screws at the same end of the mount are 120° (not 90°) apart,
    so adjustment is relatively interdependent. In contrast, the two screws on
    one side of the mount mostly affect the input and output apertures of the
    device, and are less interdependent. (TODO verify).
  - Therefore, it makes sense to alternate between adjusting the pairs of thumb
    screws at each end of the mount; for each pair, also alternate between the
    two screws to iteratively optimize.
- The upper half-rings should be closed and the top screws tightened only after
  all alignment is complete. Use only reasonable force, and check that
  alignment is not affected while tightening.
