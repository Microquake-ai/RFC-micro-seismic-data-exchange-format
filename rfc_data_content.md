
# Request For Comment &mdash; Content for &mu;Seismic Data Exchange

## Introduction

### 
The purpose of this document is to invite comments on a suggested format to allow for standardized and consistent access to μseismic data collected by mine μseismic systems. This format aims to enable a seamless and lossless exchange between different platforms.

Access to the μseismic data is inconsistent among various vendors, and occasionally within different implementations by the same vendor. Variations arise from site to site, often driven by third-party requirements. Such inconsistencies lead to inefficiencies, complicating the integration of various systems that offer complementary products and services, and making data usage unnecessarily challenging.

In light of recent advancements in μseismic monitoring technology, the imperative for a standardized format becomes even more salient. The microseismic monitoring landscape is evolving, marked by the deployment of increasingly expansive monitoring systems. These systems, given their extensive scale, inherently produce voluminous data sets. Furthermore, the incursion of Distributed Acoustic Sensing (DAS) in mining adds another layer of complexity, both in terms of data volume and diversity. DAS, with its high spatial resolution and continuous monitoring capability, generates data at an unprecedented scale. A robust and standardized format is crucial to efficiently manage, process, and interpret these burgeoning data streams, ensuring that the rich information they provide can be harnessed effectively and cohesively.

This document proposes a file structure and format. At this stage, we seek alignment on the content of the information transferred and consensus on the container format.


### Scope

With the increasing volume of μseismic data collected, especially in modern expansive monitoring systems and with technologies like Distributed Acoustic Sensing (DAS), there is a need for efficient management and handling of this data. The scope of this document is to define the content and nature of μseismic information provided to third parties and to suggest a format that facilitates efficient storage, retrieval, and exchange, even for vast datasets.

The document focuses on the foundational elements and seismic information intrinsic to μseismic monitoring. The pertinent information can be divided into three categories:

-   **Waveform** — This includes the raw, unprocessed waveform for both continuous and triggered data.
-   **Catalog Information** — This covers event information derived from waveform data processing.
-   **Inventory or System Information** — Details about the instruments (sensors and data acquisition modules) used in the data acquisition chain are included here, along with other critical data processing information, such as velocity, density, and attenuation values or models.

### Rationale

#### Need for a New Standard

The increase in microseismic data, particularly from expansive monitoring systems and new technologies like Distributed Acoustic Sensing (DAS), underscores the pressing need for a more efficient and unified data format. Currently, the varied nature of microseismic data formats hinders streamlined integration and analysis, posing challenges in managing and deriving value from large datasets. Given the critical importance of microseismic data in ensuring the safety and improving the operations of underground mining, establishing a standardized format becomes imperative. This standard would facilitate more straightforward data access, efficient storage, and smoother data exchanges across different platforms.

#### Goal

The goal is to introduce a proposed data content and packaging format that fosters interoperability, consistency, and innovation.

## Proposal

### Overview

Our proposal encompasses three categories of data: the waveforms, the catalog data, and the inventory and system information. It also concerns system metadata such as velocity models. 

We propose using the `ASDF` format with a `.asdf` extension to store the waveforms, the catalog (QuakeML), and inventory (StationXML) information. Note that the `ASDF` format also provides a convenient way to store auxiliary data such as:

-   **Ambient Noise Correlations**: Pre-computed cross-correlations between stations, which can be essential for techniques like ambient noise tomography.
    
-   **Adjoint Sources**: Useful for full waveform inversion (FWI) methods, where the difference between observed and simulated waveforms is back-propagated into the subsurface to update model parameters.
    
-   **Receiver Functions**: Derived from teleseismic recordings, these provide insights into the subsurface structure, especially at the crust-mantle boundary.
    
-   **Instrument Response**: Though typically part of the StationXML, having the actual instrument response curves stored as auxiliary data can facilitate more direct correction of the waveforms without referencing external databases.
    
-   **Processing Logs**: Detailed logs of processing steps performed on the data, which can be crucial for ensuring data quality and understanding potential issues or artifacts.

and provenance information to provide traceability and data history.

To ensure interoperability, the information in the provided files must be consistent. The sensor naming convention shall remain consistent throughout. Additionally, the locations of sensors and events should be expressed using a unified coordinate system, which must also be used for grids, if applicable.

### Why `ASDF` format?

The Adaptable Seismic Data Format (ASDF) was introduced in response to the need for an efficient and comprehensive format for seismic data storage and retrieval. Developed through collaborative research by experts in the seismology field, ASDF aims to address the complexities associated with managing diverse seismic datasets. The format has been designed with adaptability in mind, ensuring both ease of use and interoperability across various platforms. As a result of its well-structured architecture, ASDF has gained widespread acceptance within the seismology community. 

The `ASDF` format was designed to efficiently store and access large-scale array-oriented scientific data. Its design addresses the challenges posed by cloud and distributed storage, allowing for concurrent reads and writes. The format shines in scenarios where data needs to be analyzed in chunks without loading the entire dataset into memory, making it particularly apt for multidimensional arrays. With built-in support for compression and chunking, ASDF facilitates rapid data access regardless of the storage backend, whether it's file systems, object storage, or databases.

The ASDF (Adaptable Seismic Data Format) file structure is meticulously designed to cater to the specific needs of modern seismology. Central to its architecture is the seamless integration of seismic waveforms and associated metadata, ensuring the cohesiveness of data and information. The format organizes its content into two principal components: (1) the seismic waveforms, stored as time series data, and (2) the metadata, which incorporates station and event information using standardized formats such as StationXML and QuakeML. This harmonization ensures uniformity in the representation of seismic stations and earthquake event details. Processing histories, provenance details, as well as auxiliary data like adjoint sources and receiver functions, can also be embedded, further enhancing the format's comprehensive nature. A distinguishing trait of ASDF is its optimization for parallel I/O operations, positioning it as a prime choice for high-performance computing environments. The principles, design considerations, and virtues of the ASDF architecture have been expansively documented in the Geophysical Journal International (Krischer et al., 2016) [1](https://chat.openai.com/c/78d37cb0-1768-4534-8a30-dad9fe3d2f0c#user-content-fn-1%5E).

By adopting `ASDF`, we can store data and metadata in formats closely aligned with those widely accepted by the seismology community. It's feasible to encapsulate waveforms, event catalogs, and system information within a single container, enabling standalone usage. Additionally, the format can be tailored to accommodate variations in the information required for triggered and continuous data.

The benefits of using the `ASDF` format become particularly evident when considering the nature of waveform data. More broadly, the main benefit of employing the `ASDF` format to store seismic data and metadata include:

1.  1.  **Integrated Storage and Organization**: ASDF is a comprehensive and well-defined format that seamlessly integrates seismological waveform data and its associated metadata, such as event and station information. This unified approach simplifies concurrent access, data organization, and sharing, addressing many of the limitations of previous fragmented systems. By encapsulating all essential components in a standardized manner, ASDF reduces the dependency on custom, non-reusable scripts and enhances collaboration between research groups with varied internal structures.
    
2.  **Full Provenance Support**: One of the standout features of ASDF is its capability to store the complete provenance graph, capturing the history and evolution of data. This provenance tracking ensures that the origins of the data and the operations performed on it are documented, addressing issues arising from team changes, software updates, or potential processing bugs. The format thereby offers transparency and traceability, features absent or limited in other data formats.
    
3.  **Synthetic Seismogram Storage**: ASDF pioneers in accommodating proper storage and exchange of synthetic seismograms. This includes comprehensive documentation on the numerical solver used, earthquake parameters, the Earth model, and all factors influencing the simulation's outcome. Given the computational intensity of generating high-frequency waveform simulations in realistic Earth models, this preservation and thorough documentation add immense value to the seismological community.
    
4.  **Efficiency in File Management**: ASDF offers a significant reduction in the number of files required for various tasks. A single ASDF file can effectively replace tens to hundreds of thousands of individual waveform files. This consolidation not only results in raw performance and organizational advantages but also addresses challenges like file count quota limits, especially on supercomputing platforms. Furthermore, the format supports efficient parallel I/O on suitable hardware, making it ideal for scalable parallel data processing workflows.
    
5.  **Versatile Data Type Support**: Beyond seismograms, ASDF is adept at accommodating diverse data types prevalent in seismology. Whether it's spectral estimations, cross-correlations, adjoint sources, receiver functions, or other data types, ASDF ensures organized, self-describing storage. This versatility ensures that seismologists can maintain a consistent data storage paradigm across various facets of their research.

This format is particularly suited to meet the requirements of tomorrow.

### Data Format and Adaptation

The key is to ensure consistencies between the data and information to allow efficient cross-referencing, retrieval and usage.

In this section we will conver four types of data:

- **Waveforms**
- **Catalog**
- **Inventory/System**
- **Grids**

### Waveform Data

The waveform data represents the raw vibrations recorded directly by the sensors. For convenience, waveform data can be provided in physical units native to the instrument recording the data of $m$, $m/s$​, or $m/s^2$ for displacement, velocity, and acceleration, respectively. However, if size is a concern, storing the ADC counts as integers is more suitable. Storing the ADC count as integers allows for the more efficient use of compression algorithm and will allow the data to be more compact.

Alongside the amplitude values, additional metadata describing the instrument recording the data and time series parameters should be provided for each trace. Each trace should be stored with its own metadata information.

The required metadata for each trace includes:

-   **Trace Identifier**: A unique identifier for the trace (see Obspy [documentation](https://docs.obspy.org/master/packages/autogen/obspy.core.event.resourceid.ResourceIdentifier.html) for information on the recommended _resource identifier_ structure). The trace identifier format and convention should be normalized and align with the SEED naming convention including the standard part. Deviation regarding the naming shall, howerver, be permitted to allow longer names to be utilized.  
-   **Location Identification**: The location identification convention described [here](https://ds.iris.edu/ds/newsletter/vol1/no1/1/specification-of-seismograms-the-location-identifier/). This convention has been adapted for μseismic monitoring in the mining context.
    -   **Network Code [network_code]** — Code representing the network.
    -   **Station Code [station_code]** — Code representing the station containing the digitizer.
    -   **Location Code [location_code]** — Code representing the instrument.
    -   **Channel Code [channel_code]** — The three (3) alphanumerical code representing the channel shall follow the FDSN standard naming convention of August 2000 described in the SEED document [Appendix A](http://www.fdsn.org/pdf/SEEDManual_V2.4_Appendix-A.pdf). For instance, a typical 14 Hz or 15 Hz omnidirectional geophone code would be GH?, where ? would be replaced by the appropriate component orientation code.
-   **Sampling Rate [sampling_rate]** — The signal's sampling rate in samples per second.
-   **Calibration Factor [calib]** — This value is optional and defaults to 1.0 if not provided. It represents the calibration factor should the sensor deviate from the typical response.
-   **Start Time [starttime]** — The trace's start time.

#### Notes:

**Naming Convention**: The full name of a component or channel is usually created as follows: **channel name** = **network code**.**station code**.**location code**.**channel code**. 

**Network, Station, and Location Codes**: The mentioned convention is often not strictly adhered to by μseismic system providers. Flexibility in applying the convention is crucial. Typically, no distinction is made between the station and location, and each instrument is assigned a unique code that may or may not refer to the data acquisition station. In such cases, we suggest using the **Station Code** field to store the _instrument_ code and populate the location code with 01 or 00. In a network, each combination of **Station Code**.**Location Code**.**Channel Code** should be unique.

#### Suggested Naming Convention and Usage

- **Network Code**: The network code should represent a physical or logical network. A mine or mining complex can include multiple networks. The network name should be compact but be easily recognized and understood. For instance, the Oyu Tolgoi Hugo North Underground Lift 1 network can be represented by the following accronym: OTHNL1.
- **Station Code**: We recommend associating the station code to the location of a junction box where the data acquisition is talking place. The name should provide clear indication of where the JB is located within a mine. We suggest using the naming convention adopted at the operation. For example, assuming there are two junction box location in the Haulage Level Access Drive 2 referred to as HLAD2, the code for the two stations can be HLAD2_01 and HLAD2_02.
- **Location Code**: We suggest using the location c


### Catalog

The catalog includes information related to an event, a trigger, or a series of events. We suggest storing the catalog information in a QuakeML-like format adapted to μseismic data (see QuakeML [documentation](https://quake.ethz.ch/quakeml)). The suggested changes affect the following QuakeML objects:

-   **Event** — The event_type field is restricted to specific values that are not suited for μseismic monitoring.
-   **Origin** — The position is expressed in latitude and longitude. This will need to be changed to x, y, and z.
-   **Magnitude** — The magnitude object would benefit from adding fields to store the corner frequency, as well as P-wave and S-wave Energy. This allows for a broad range of source parameters to be computed on the fly, rather than stored in the magnitude object. This approach is typically used in mine seismology.

#### Event — Event Type

There are two approaches to modifying the event types:

1.  Redefine the schema and allow for event types related to mining to be stored in a μQuakeML file; and
2.  Map each μseismic type to an existing QuakeML event type.

| Event Type (&mu;seismic)            | Event Type (QuakeML)        |
|-------------------------------------|-----------------------------|
| earthquake/large event              | earthquake                  |
| seismic event                       | induced or triggered event  |
| offsite event                       | atmospheric event           |
| rock burst                          | rock burst                  |
| fall of ground/rockfall             | cavity collapse             |
| blast                               | explosion                   |
| blast sequence                      | accidental explosion        |
| development blast                   | industrial explosion        |
| production blast                    | mining explosion            |
| far away blast/open pit blast       | quarry blast                |
| offsite blast                       | nuclear explosion           |
| paste firing                        | chemical explosion          |
| calibration blast                   | controlled explosion        |
| other blast/slashing                | experimental explosion      |
| mid-shift blast/slash blast         | industrial explosion        |
| raise bore                          | hydroacoustic event         |
| crusher noise                       | road cut                    |
| orepass noise                       | collapse                    |
| drilling noise                      | acoustic noise              |
| electrical noise                    | thunder                     |
| scaling noise                       | anthropogenic event         |
| mechanical noise                    | crash                       |
| test pulse                          | sonic boom                  |
| unidentified noise/other noise      | other event                 |
| duplicate                           | boat crash                  |
| unknown                             | plane crash                 |
| tap test/test                       | avalanche                   |


The above mapping is implemented in the μQuake library.

#### Origin — Coordinate System

The QuakeML format was designed to store seismic data at a regional and global scale. Consequently, the QuakeML format has adopted a spherical coordinate system, storing location information in latitude and longitude.

Using latitude and longitude to describe mining-related events is possible but not convenient. It would require precisely knowing the series of transformations needed to convert the position expressed in the local coordinate system into longitude and latitude. We suggest overriding the latitude and longitude information and using x, y, and z instead. The use of x, y, and z notation, as opposed to the right-handed (easting, northing, and elevation) or (northing, easting, down) triplets, is to provide flexible notation and ensure consistency between the mine and the seismic system's coordinate systems.

The μQuake implementation exploits the QuakeML extra parameters to store the x, y, and z values and associated errors.

#### Magnitude — Corner Frequency, and Energy

The QuakeML standard does not include objects suited to store the corner frequency. The energy could be stored in the amplitude or station_magnitude object. However, this isn't convenient, as it's preferable for the magnitude and energy information to be used in tandem.

The μQuake implementation stores the `corner_frequency`, the `energy_p`, `energy_s`, and associated error alongside the magnitude information in the extra parameter of the Magnitude object.

### System or Inventory Information

The system or inventory information is crucial for any seismic network as it provides comprehensive details about the stations and channels that are a part of that network. The StationXML format, which is an established standard in the seismic community, is designed to hold such inventory metadata.

However, when tailoring the use of StationXML to the μseismic domain, especially in the mining context, certain modifications are required to better fit the data's unique characteristics and requirements.

#### Station & Channel Coordinates

In a typical seismic application, the StationXML format uses latitude and longitude to describe the geographical position of stations and channels. While this makes sense for global and regional seismic networks, it's not as intuitive or convenient for the μseismic monitoring within a mining setup. In such a scenario, the geographical location is often best described using a local coordinate system.

Thus, we recommend replacing the latitude and longitude fields in the StationXML format with `x`, `y`, and `z` coordinates for both the station and channel locations. This change aligns with the earlier modifications made for QuakeML, ensuring consistency across different components of the μseismic system.


### System Metadata and Grid Data

To fully enable the interpretation of the seismic data, accessing information on the velocity models is essential. Other important although not as essential are the attenuation model if they exist and the density values or model. The velocity values/models are increasingly represented on a grid. 

In this section, we propose using a simple grid format for data exchange.  


#### Grid Format Overview

The grid format is structured to represent the velocity model in a systematic, gridded layout. It comprises two main components: the header, which contains metadata about the grid, and the grid values that reflect the actual data.

##### Header

The header is a crucial section that offers insights into the grid's specifications, such as its dimensions, the starting point (origin) of the grid, and the intervals or spacings between grid points. A sample header in Python dictionary format might look like:

```python
header = {
    "NX": 100,            # Number of grid points in the X direction
    "NY": 100,            # Number of grid points in the Y direction
    "NZ": 50,             # Number of grid points in the Z direction
    "ORIGX": 0.0,         # X coordinate of the grid origin
    "ORIGY": 0.0,         # Y coordinate of the grid origin
    "ORIGZ": 0.0,         # Z coordinate of the grid origin
    "SPACEX": 0.1,        # Grid spacing in the X direction
    "SPACEY": 0.1,        # Grid spacing in the Y direction
    "SPACEZ": 0.2,        # Grid spacing in the Z direction
}
```

This dictionary effectively captures the grid's spatial boundaries, intervals, and the type of data contained.

##### Grid Data

The grid data values follow the header. Organized in a sequence, they represent their positions within the grid. For simplicity and efficiency, these values can be represented as a 2D numpy array:

```python
import numpy as np

# For demonstration purposes, initializing a 2D array with NXxNYxNZ dimensions
grid_values = np.zeros((header["NX"], header["NY"], header["NZ"]))

```

In this representation, the grid values are stored in a structured manner, allowing for easy indexing and operations. Each cell in the array corresponds to a grid point in the velocity model, with its value representing the velocity (or density or attenuation, depending on the grid type) at that point.

## Implementation using the &mu;Quake Python Library

Within the context of this RFC, which proposes specific adaptations and formats tailored to microseismic monitoring, the `uquake` library stands as a practical implementation of these recommendations. Developed in Python and built upon ObsPy, `uquake` has been specially crafted to cater to the distinctive needs of microseismic monitoring in mining environments. This includes, amongst other capabilities, the handling of local coordinate systems and unique event types frequently observed in mine microseismic monitoring. For those seeking an immediate and tangible application of the proposed data format recommendations from this RFC, `uquake` offers a ready solution. It is open source, distributed under AGPL license and publicly available and can be easily accessed and installed via pip using `pip install uquake`.

Note that there are alternative libraries addressing the same issues &mu;Quake tackles. The author is, howerver, less familiar with the alternatives. Notable alternatives are

The Niosh [Obsplus](https://github.com/niosh-mining/obsplus)
[DASCORE](https://github.com/DASDAE/dascore)

### Implementation using `uquake`

The `uquake` library provides the necessary tools to handle gridded data in seismology. One of the primary objects to manage such grid data is the `Grid` class from `uquake.core.grid.base`. This class is designed to hold regular grids and can be employed for both 2D and 3D data structures.

#### Initializing a Grid

The `Grid` object can be initialized using either a numpy array representing the grid data or by specifying the grid dimensions. When using the latter approach, the grid is initialized with a default or specified value.

Let's consider a simple example to illustrate this:
```python
from uquake.core.grid.base import Grid
import numpy as np

# Define grid header specifications

header = {
    "NX": 100,  
    "NY": 100,  
    "NZ": 50,   
    "ORIGX": 0.0,  
    "ORIGY": 0.0,  
    "ORIGZ": 0.0,  
    "SPACEX": 0.1,  
    "SPACEY": 0.1,  
    "SPACEZ": 0.2,  
}

# Initialize the grid using grid dimensions and set default value as 0

grid_dims = (header["NX"], header["NY"], header["NZ"])
spacing = (header["SPACEX"], header["SPACEY"], header["SPACEZ"])
origin = (header["ORIGX"], header["ORIGY"], header["ORIGZ"])

grid = Grid(data_or_dims=grid_dims, spacing=spacing, origin=origin, value=0)

# Write the modified or new grid to a file
grid.write("path_to_new_grid_file.grid")
```
Here, we've initialized a 3D grid using the dimensions specified in the header. The grid is filled with a default value of 0.

#### Reading and Writing Grids

The `Grid` object is equipped with methods to read and write grid data, making it easy to load existing grids or store newly created or modified grids.

Here's an example of how one might read from and write to a grid file:

```python
# all versions
from uquake.core.grid import read_grid

# Reading a grid from a file
grid_from_file = Grid.read("path_to_grid_file.grid")
```

```python
# from version 2.0.1
# Reading a grid from a file
grid_from_file = Grid.read("path_to_grid_file.grid") 
```

This approach simplifies the process of handling gridded data in seismology by using the `uquake` library's built-in functionalities.

## Rationale

Understanding the motivations and reasons behind any proposed change is essential to ensure clarity, gain stakeholder buy-in, and ensure the technical merits of the proposal align with the broader goals.

1. **Choice of Data Packaging Formats**: The decision to employ ASDF for the encapsulation of both catalog information and waveform data, combined with the inherent XML structure for QuakeML, is rooted in their respective proficiencies and widespread acceptance within the industry. ASDF provides an efficient mechanism for storing and fetching data, especially when dealing with substantial datasets. Concurrently, the integral XML structure guarantees interoperability with systems customarily developed for QuakeML processing.

2.  **Adapting QuakeML for μseismic Data**: Traditional QuakeML formats, while robust for standard seismology needs, are not fully suited to address the nuances of μseismic data. The unique requirements of μseismic monitoring, especially in mining contexts, necessitate modifications to the existing format. By making these adjustments, the system becomes more flexible and directly applicable to the μseismic domain, allowing for better data representation and interpretation.
    
3.  **Use of a Grid for Velocity Models**: Seismic data interpretation largely hinges on the accuracy and clarity of velocity models. The industry trend has shifted towards grid representations of these models due to their higher precision and ease of use. Implementing a standard grid format for data exchange promotes interoperability and standardization across systems and tools.
    
4.  **Introduction of the `uquake` Library**: The seismic domain has various software libraries that cater to specific needs. The choice of `uquake` was driven by its capabilities to manage the proposed data structures efficiently and its extensibility to cater to future modifications. By embedding support for the newly proposed data structures directly within `uquake`, users can expect a seamless experience and avoid the complexities associated with multi-tool workflows.
    
5.  **Latitude and Longitude to X, Y, Z Conversion**: Given that most mining operations work on local coordinate systems rather than global ones, expressing positions in terms of x, y, and z becomes not just convenient, but also essential. This shift eliminates the need for complex transformations and ensures data is immediately useful for local operations and analysis.

In summary, the proposed changes and implementations in this RFC stem from a direct need to address the unique challenges posed by μseismic monitoring, especially in mining contexts. They reflect both the evolution of seismic data interpretation methodologies and the tools used in the industry. Through these proposals, we aim to establish a more streamlined, precise, and standardized approach to μseismic data management and interpretation.

### Feedback Mechanism

Feedback and contributions from the community are essential to refining and improving this RFC. There are two primary ways through which stakeholders, developers, and interested parties can provide their feedback:

1.  **GitHub Issues**:
    
    -   Navigate to the [RFC micro-seismic data exchange format repository on GitHub](https://github.com/Microquake-ai/RFC-micro-seismic-data-exchange-format/tree/main).
    -   Go to the `Issues` tab.
    -   Click on `New Issue` to create a new issue.
    -   Provide a concise title and detailed description of your feedback, suggestions, or concerns.
    -   Once submitted, the issue will be visible to the community, and the project maintainers will review and address it as appropriate.
 
2.  **Email**:
    
    -   If you prefer a more direct approach or have feedback that you'd like to keep private, you can send an email to [rfc_format@microquake.ai](mailto:rfc_format@microquake.ai).
    -   Please provide a clear subject line relevant to your feedback to ensure swift handling of your email.
    -   While we appreciate all feedback, do note that due to the volume of emails, it might take some time before you receive a response.

Whether you choose to leave an issue on GitHub or send an email, your feedback is invaluable. It aids in ensuring that the proposed micro-seismic data exchange format is robust, relevant, and addresses the needs of the community.

Thank you for taking the time to review this RFC and for your contributions towards its continual improvement.

## References

Krischer, L., Smith, J. A., Lei, W., Lefebvre, M., Ruan, Y., & Tromp, J. (2016). An Adaptable Seismic Data Format. Geophysical Journal International, 207(2), 1003–1011. [Link](https://academic.oup.com/gji/article/207/2/1003/2583765)









<!--stackedit_data:
eyJoaXN0b3J5IjpbLTUwNzQwMDQyLDEzMzkzNTAxMywtMjE0NT
Q4NTQyMSwtMTg3MzY0MjgyNCwtODAzNDE3NDg0LC0xMTI0NTk5
OTM5LDE0MjkxOTI5MjYsMjkxNjg5MTM2LDE5OTQ0OTU2MzIsLT
Y0MjIxODEyMyw5ODY5NTE2NzYsLTEyODgxMzE2OSwtMzg5NDM1
OTkzLC03NTQzNzE4MTksLTE5ODcwNDMwOTksLTE5NDU3Mjc4NT
gsLTMxMjAyODIzOCw0NjM2ODQ0OTcsLTE2MTYxNzM1ODIsMTA0
NDQwNTE1NF19
-->