# Request For Comment &mdash; Content for &mu;Seismic Data Exchange

## Introduction

### Purpose

The purpose of this document is to invite comments on a suggested format to allow standardized and consistent access to the &mu;seismic data collected by mine &mu;seismic system and enable seamless and lossless exchange between different platforms. 

Access to the &mu;seismic data is not consistent between the different vendors, and sometimes between different implementation by the same vendors. There are variations from site to site, often driven by third party requirements. This leads to inefficiencies that make the integration of different systems providing complementary products and services and the usage of data unnecessarily difficult. 

The current document does not propose or prescribe specific format at this stage. This will come later. At this stage, we seek alignment regarding the content of the information transferred, agreement on the container should come at a later stage. Deviation from the previous statement will concern the packaging of the waveforms. Waveforms receive a special treatment because they can be voluminous and there exists significant divergence between the different vendors regarding the nature and packaging. Efficient processing requires the data format to be prescribed.

### Scope

The scope is to define the content and nature &mu;seismic information provided to third parties. This document is concerned with the building blocks and seismic information that are the essence of &mu;seismic monitoring. The information of concern can be classified in three categories:

- **Waveform** &mdash; The raw unprocessed waveform both for the continuous and triggered data.
- **Catalog Information** &mdash; The event information derived from the processing of the waveform data
- **Inventory or System Information** &mdash; This includes the information regarding the instruments (sensor and data acquisition modules) along the data acquisition chain and other information critical to processing the data (e.g., velocity, density and attenuation values or models).

### Rationale

#### Need for a New Standard

Microseismic data plays a important role in ensuring the safety and enabling the efficiency of underground mining operations. While it's one of several essential tools in the arsenal of modern mining techniques, the current scenario of diverse data formats can sometimes complicate seamless integration and analysis.

#### Goal

The proposed data content is mainly designed to promote interoperability and innovations.

## Proposal

### Overview

Our proposal concerns three categories of data, the waveforms, the catalog data and the inventory and system information.

To ensure interoperability, the information in the provided files shall be consistent. The sensor naming convention shall be the same all across, the locations of sensors and events shall be expressed using one single coordinate system, which should also be used for the grids, if applicable.

### Waveform data

The waveform data is the raw vibration recorded directly by the sensors. For convenience, the waveform data should be provided in physical units native to the instrument recording the data of $m$, ${m}/{s}$, or $m/s^2$ for displacement, velocity and acceleration, respectively. 

The vibration data shall be provided with the metadata required to appropriately read and interpret the waveform.

The required metadata are:

- **Network Code** &mdash; Represents the code of the network and shall be expressed with two character
- **Station Code** &mdash; Represents the code of the station that contains the digitizer.
- **Location Code** &mdash; A two character numerical code representing the recording site (where the sensor is deployed). For each station, the location code shall be unique. 
- **Channel Code** &mdash; The three character channel code shall follow the FDSN standard naming convention of August 2000 described [here](https://ds.iris.edu/ds/nodes/dmc/data/formats/seed-channel-naming/). The first letter represents the band code, the second the instrument code and the third the orientation code. For instance, a typical $14 Hz$ or $15 Hz$ omnidirectional geophones code would be GH?, where ? would be replaced be the component orientation code.

| Orientation Code | Description                                              |
|------------------|----------------------------------------------------------|
| Z N E            | Traditional (Vertical, North-South, East-West)           |
| A B C            | Triaxial (Along the edges of a cube turned up on a corner)|
| T R              | For formed beams (Transverse, Radial)                     |
| 1 2 3            | Orthogonal components but non traditional orientations   |
| U V W            | Optional components                                       |
| Dip/Azimuth:     | Ground motion vector (reverse dip/azimuth if signal polarity incorrect) |
| Signal Units:    | M, M/S, M/S**2, (for G & M) M/S**2 (usually)              |
| Channel Flags:   | G                                                        |
|





<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwOTE0NDI4OSwtMTM3MzcwMjM1NywtMT
M4NTk3MDM1MF19
-->