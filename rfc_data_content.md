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

The waveform data is the raw vibration recorded directly by the sensors. For convenience, the waveform data can be provided in physical units native to the instrument recording the data of $m$, ${m}/{s}$, or $m/s^2$ for displacement, velocity and acceleration, respectively. However, if size is of concern, storing the ADC count is more appropriate. Storing the ADC count represented as integers allow the usage of the Steim1 and Steim2 differential compression algorithms. 

Along the amplitude values additional metadata describing the instrument recording the data and time series parameter should be provided for each trace. Each trace shall be stored with its own metadata information

The required metadata for each trace are:

- **Location identification**: The location identification convention described [here](https://ds.iris.edu/ds/newsletter/vol1/no1/1/specification-of-seismograms-the-location-identifier/). The convention has been adapted for the purpose of &mu;seismic monitoring in the mining context. 
  - **Network Code [network_code]** &mdash; Represents the code of the network and shall be expressed with two character
  - **Station Code [station_code]** &mdash; A five (5) alphanumerical code representing the station contains the digitizer. The station is defined as the location where the data acquisition units reside.
  - **Location Code [location_code]** &mdash; A two (2) alphanumerical  code representing the recording site or the location of an instrument comprising one or more components. For each station, the location code of individual instrument shall be unique. 
  - **Channel Code [channel_code]** &mdash; The three (3) alphanumerical code that represents the channel and shall follow the FDSN standard naming convention of August 2000 described in the SEED document [Appendix A](http://www.fdsn.org/pdf/SEEDManual_V2.4_Appendix-A.pdf). The first letter represents the band code, the second the instrument code and the third the orientation code. For instance, a typical $14 Hz$ or $15 Hz$ omnidirectional geophones code would be GH?, where ? would be replaced by the appropriate component orientation code. 
 - **Sampling Rate [sampling_rate]** &mdash; The signal sampling rate in sample per second.
 - **Calibration Factor [calib]** &mdash; The calibration factor this value is optional and will be set to 1.0 if not provided. This value represents the calibration factor should the sensor deviate from the typical response.
 - **Start Time [starttime]** &mdash; The trace start time.  

**Suggested data format:** We recommend using the _miniSEED_ format using a 4096 block size to package the waveform data. The _miniSEED_ format is widely adopted in seismology. It is an efficient, flexible and easy to read seismic data container. The _miniSEED_ format can accommodate traces from different instrument q
          






<!--stackedit_data:
eyJoaXN0b3J5IjpbNDY3MDM0MDIsMTcxNDk5ODI0MCwtNDY2Mj
gwNjUwLDE2MzAxNTI3MjQsLTEzNzM3MDIzNTcsLTEzODU5NzAz
NTBdfQ==
-->