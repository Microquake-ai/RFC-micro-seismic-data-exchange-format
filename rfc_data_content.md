
# Request For Comment &mdash; Content for &mu;Seismic Data Exchange

## Introduction

### Purpose

The purpose of this document is to invite comments on a suggested format to allow for standardized and consistent access to &mu;seismic data collected by mine &mu;seismic systems. This format aims to enable a seamless and lossless exchange between different platforms.

Access to the &mu;seismic data is inconsistent among various vendors, and occasionally within different implementations by the same vendor. Variations arise from site to site, often driven by third-party requirements. Such inconsistencies lead to inefficiencies, complicating the integration of various systems that offer complementary products and services, and making data usage unnecessarily challenging.

This document proposes a file structure and format. At this stage, we seek alignment on the content of the information transferred and consensus on the container format.

### Scope

The scope of this document is to define the content and nature of &mu;seismic information provided to third parties and to suggest a format for packaging this information. It focuses on the foundational elements and seismic information intrinsic to &mu;seismic monitoring. The pertinent information can be divided into three categories:

- **Waveform** &mdash; This includes the raw, unprocessed waveform for both continuous and triggered data.
- **Catalog Information** &mdash; This covers event information derived from waveform data processing.
- **Inventory or System Information** &mdash; Details about the instruments (sensors and data acquisition modules) used in the data acquisition chain are included here, along with other critical data processing information, such as velocity, density, and attenuation values or models.

### Rationale

#### Need for a New Standard

Microseismic data plays an essential role in ensuring the safety and enhancing the efficiency of underground mining operations. Although it is one of the many vital tools in the repertoire of modern mining techniques, the existing diverse data formats can impede smooth integration and analysis.

#### Goal

The goal is to introduce a proposed data content and packaging format that fosters interoperability, consistency, and innovation.


## Proposal

### Overview

Our proposal encompasses three categories of data: the waveforms, the catalog data, and the inventory and system information. We propose storing this information in a single `Zarr` file with a `.zarr` extension.

To ensure interoperability, the information in the provided files must be consistent. The sensor naming convention should remain consistent across all files. Additionally, the locations of sensors and events should be expressed using a unified coordinate system, which must also be used for grids, if applicable.

### Why `Zarr` format?

The recommendation to use the `Zarr` format for packaging the information came from personnel at the IRIS data center. IRIS now encourages submissions in the `TimeDB` or `Zarr` format. Storing data in `TileDB` requires a database engine, necessitating additional components to be installed alongside the server.

The `Zarr` format was designed to efficiently store and access large-scale array-oriented scientific data. Its design addresses the challenges posed by cloud and distributed storage, allowing for concurrent reads and writes. The format shines in scenarios where data needs to be analyzed in chunks without loading the entire dataset into memory, making it particularly apt for multidimensional arrays. With built-in support for compression and chunking, Zarr facilitates rapid data access regardless of the storage backend, whether it's file systems, object storage, or databases.

By adopting `Zarr`, we can store data and metadata in formats closely aligned with those widely accepted by the seismology community. It's feasible to encapsulate waveforms, event catalogs, and system information within a single container, enabling standalone usage. Additionally, the format can be tailored to accommodate variations in the information required for triggered and continuous data.

The benefits of using the `Zarr` format become particularly evident when considering the nature of waveform data. More broadly, the main advantages of employing the `Zarr` format to store seismic data and metadata include:

1.  **Chunked Storage and Access**: Zarr's inherent design supports chunking, allowing users to efficiently read or write small sections of large seismic datasets without accessing the entire file. This feature is especially advantageous for processing extensive continuous seismic recordings.
    
2.  **Concurrent Reads and Writes**: Built with cloud and distributed storage environments in mind, Zarr facilitates simultaneous reads and writes by multiple processes or users without conflicts, promoting collaborative analysis.
    
3.  **Flexible Compression**: Zarr is compatible with various compression algorithms. This flexibility ensures that extensive seismic data can be compactly stored while still ensuring rapid access times.
    
4.  **Multidimensional Support**: Seismic data often presents itself in multi-dimensional arrays (e.g., time, depth, latitude, longitude). Zarr natively supports these multi-dimensional datasets, simplifying data organization and access.
    
5.  **Metadata Storage**: Zarr permits the inclusion of metadata directly within the dataset. This feature ensures that seismic trace metadata, acquisition details, and processing histories can coexist alongside the waveform data, providing a comprehensive context for the stored seismic information.

### Waveform Data

The waveform data represents the raw vibrations recorded directly by the sensors. For convenience, waveform data can be provided in physical units native to the instrument recording the data of �m, ��sm​, or �/�2m/s2 for displacement, velocity, and acceleration, respectively. However, if size is a concern, storing the ADC count is more suitable. Storing the ADC count as integers allows for the use of the Steim1 and Steim2 differential compression algorithms.

Alongside the amplitude values, additional metadata describing the instrument recording the data and time series parameters should be provided for each trace. Each trace should be stored with its own metadata information.

The required metadata for each trace includes:

-   **Trace Identifier**: A unique identifier for the trace (see Obspy [documentation](https://docs.obspy.org/master/packages/autogen/obspy.core.event.resourceid.ResourceIdentifier.html) for information on the recommended _resource identifier_ structure).
-   **Location Identification**: The location identification convention described [here](https://ds.iris.edu/ds/newsletter/vol1/no1/1/specification-of-seismograms-the-location-identifier/). This convention has been adapted for μseismic monitoring in the mining context.
    -   **Network Code [network_code]** — Code representing the network.
    -   **Station Code [station_code]** — Code representing the station containing the digitizer.
    -   **Location Code [location_code]** — Code representing the instrument.
    -   **Channel Code [channel_code]** — The three (3) alphanumerical code representing the channel shall follow the FDSN standard naming convention of August 2000 described in the SEED document [Appendix A](http://www.fdsn.org/pdf/SEEDManual_V2.4_Appendix-A.pdf). For instance, a typical 14��14Hz or 15��15Hz omnidirectional geophone code would be GH?, where ? would be replaced by the appropriate component orientation code.
-   **Sampling Rate [sampling_rate]** — The signal's sampling rate in samples per second.
-   **Calibration Factor [calib]** — This value is optional and defaults to 1.0 if not provided. It represents the calibration factor should the sensor deviate from the typical response.
-   **Start Time [starttime]** — The trace's start time.

#### Notes:

**Network, Station, and Location Codes**: The mentioned convention is often not strictly adhered to by μseismic system providers. Flexibility in applying the convention is crucial. Typically, no distinction is made between the station and location, and each instrument is assigned a unique code that may or may not refer to the data acquisition station. In such cases, we suggest using the **Station Code** field to store the _instrument_ code and populate the location code with 01 or 00. In a network, each combination of **Station Code** – **Location Code** – **Channel Code** should be unique.

#### Waveform Data Packaging

##### Zarr

The IRIS DMC recommends using the `Zarr` formats (or TileDB) over the `HDF5` based formats, such as the `ASDF` format. The `Zarr` format can be conveniently used to store waveform data. We strongly encourage adhering to the naming convention presented in the previous section, giving each component a unique name composed as `network_code.station_code.location_code.channel_code`.

**Note**: The `Zarr` file format can also incorporate the catalog and inventory information.

Below is an example written in Python to store information from an _Obspy_ Stream object in a `.zarr` file:

```python
import zarr
# import obspy
import uquake  # the uquake library can be used instead of Obspy 

def stream_to_zarr_group(stream, zarr_group_path):
    """
    Converts an ObsPy/uQuake stream to a Zarr group.
    Each trace is stored as a separate Zarr array with its associated metadata.
    :param stream: ObsPy/uQuake Stream object
    :param zarr_group_path: Path to create/save the Zarr group
    """

    # Create or open a Zarr group
    group = zarr.open_group(zarr_group_path, mode='a')

    for tr in stream:

        # Create Zarr array for this trace
        arr = group.create_dataset(trace_id, data=tr.data, 
        shape=(len(tr.data),), dtype='float32', overwrite=True)

        # Store selected stats as Zarr attributes
        for key in ['network_code', 'station_code', 'location_code', 
                    'channel_code', 'sampling_rate', 'starttime', 'calib', 'resource_id']:
            # Convert non-string objects to strings for easier storage and retrieval
            arr.attrs[key] = tr.stats[key]

# Load a sample ObsPy Stream (modify this to load your data)
st = uquake.read()

# Convert and store the stream in a Zarr group
stream_to_zarr_group(st, 'seismic_data_group.zarr')
```
##### MiniSEED

The _miniSEED_ format is another viable option. _MiniSEED_ is widely adopted in seismology and is exceptionally suitable for storing seismic data. The _miniSEED_ format can accommodate traces from various instrument types (geophone, accelerometer, seismometer, etc.), acquired at different sampling rates, with varying start and end times. However, it's essential to note that to use _miniSEED_, the network, station, location, and channel codes must adhere to a strict convention described below:

-   **Network Code** — Two (2) alphanumerical characters.
-   **Station Code**: Five (5) alphanumerical characters.
-   **Location Code**: Two (2) alphanumerical characters.
-   **Channel Code**: Three (3) alphanumerical characters, following the FDSN guidelines of August 2000.

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

#### Catalog Information Packaging

##### Zarr

The catalog information can easily be packaged with the waveform in a `Zarr` file. The QuakeML simply needs to be serialized using the json library. The example below shows how an `Obspy` or `uQuake` catalog can be stored in a `Zarr` file and retrieved:

```python
# import obspy
import uquake # similar to obspy with the modifications previously 
              # discussed above implemented
import zarr
import json

# Sample Catalog
cat = obspy.read_events()

# 1. Serialize the Catalog to JSON
json_string = cat.write(format="JSON", filename=None)

# 2. Store the Serialized JSON in Zarr
root = zarr.open_group("catalog.zarr", mode="w")
root.array("catalog_data", data=json_string)

# Reading Back from Zarr
stored_json_string = root["catalog_data"][:].tobytes().decode()

# 3. Deserialize back to Catalog object
new_cat = obspy.read_events(filename=None, format="JSON", 
data=stored_json_string)

print(new_cat)
```

##### QuakeML

The catalog can also be stored in the native XML format, which is QuakeML's traditional format. QuakeML, being a structured XML-based format, offers a robust platform for standardizing the description of seismic events and their associated parameters. For those familiar with the format and looking to integrate with other systems that recognize QuakeML, using the native XML format is advantageous. However, it's essential to be aware of the modifications made for the μseismic context to ensure compatibility.

### System or Inventory Information

The system or inventory information is crucial for any seismic network as it provides comprehensive details about the stations and channels that are a part of that network. The StationXML format, which is an established standard in the seismic community, is designed to hold such inventory metadata.

However, when tailoring the use of StationXML to the μseismic domain, especially in the mining context, certain modifications are required to better fit the data's unique characteristics and requirements.

#### Station & Channel Coordinates

In a typical seismic application, the StationXML format uses latitude and longitude to describe the geographical position of stations and channels. While this makes sense for global and regional seismic networks, it's not as intuitive or convenient for the μseismic monitoring within a mining setup. In such a scenario, the geographical location is often best described using a local coordinate system.

Thus, we recommend replacing the latitude and longitude fields in the StationXML format with `x`, `y`, and `z` coordinates for both the station and channel locations. This change aligns with the earlier modifications made for QuakeML, ensuring consistency across different components of the μseismic system.

#### System/Inventory Information Packaging

The inventory or system information of a seismic network, which details the stations and channels, can also be serialized and stored efficiently. We propose to options. The first option consist in storing the system or inventory information alongside the waveform and catalogue in a  `Zarr` file. The second option consist in directly using the `StationXML` format.

##### Zarr

Storing inventory information in a `Zarr` format offers flexibility, especially when dealing with large datasets. Like the catalog information, the StationXML data can be serialized using the json library and then stored in a `Zarr` file. This allows for efficient storage and quick retrieval of inventory data. An code example is provided below:

```python
# import Obspy
import uquake # Similar to obspy but tailored for μseismic
import zarr
import json

# Sample Inventory
inventory = uquake.read_inventory()

# 1. Serialize the Inventory to JSON
json_string = inventory.write(format="JSON", filename=None)

# 2. Store the Serialized JSON in Zarr
root = zarr.open_group("inventory.zarr", mode="w")
root.array("inventory_data", data=json_string)

# Reading Back from Zarr
stored_json_string = root["inventory_data"][:].tobytes().decode()

# 3. Deserialize back to Inventory object
new_inventory = uquake.read_inventory(filename=None, format="JSON", 
data=stored_json_string)

print(new_inventory)
```

##### StationXML

StationXML is the conventional format for representing inventory information in the seismic community. Being a structured XML-based format, it provides a comprehensive framework for detailing stations and channels. When using StationXML, especially in a μseismic context, it's vital to ensure that the modifications made (such as the introduction of `x, y, z` coordinates and unit vector for channel orientation) are incorporated correctly.

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

#### Implementation

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

1.  **Adapting QuakeML for μseismic Data**: Traditional QuakeML formats, while robust for standard seismology needs, are not fully suited to address the nuances of μseismic data. The unique requirements of μseismic monitoring, especially in mining contexts, necessitate modifications to the existing format. By making these adjustments, the system becomes more flexible and directly applicable to the μseismic domain, allowing for better data representation and interpretation.
    
2.  **Use of a Grid for Velocity Models**: Seismic data interpretation largely hinges on the accuracy and clarity of velocity models. The industry trend has shifted towards grid representations of these models due to their higher precision and ease of use. Implementing a standard grid format for data exchange promotes interoperability and standardization across systems and tools.
    
3.  **Introduction of the `uquake` Library**: The seismic domain has various software libraries that cater to specific needs. The choice of `uquake` was driven by its capabilities to manage the proposed data structures efficiently and its extensibility to cater to future modifications. By embedding support for the newly proposed data structures directly within `uquake`, users can expect a seamless experience and avoid the complexities associated with multi-tool workflows.
    
4.  **Latitude and Longitude to X, Y, Z Conversion**: Given that most mining operations work on local coordinate systems rather than global ones, expressing positions in terms of x, y, and z becomes not just convenient, but also essential. This shift eliminates the need for complex transformations and ensures data is immediately useful for local operations and analysis.
    
5.  **Choice of Data Packaging Formats**: The proposal to utilize `Zarr` for packaging catalog information and waveform data, and the native XML format for QuakeML, was driven by their respective capabilities and industry adoption. `Zarr` offers efficient storage and retrieval mechanisms, especially for large datasets, while the native XML format ensures compatibility with systems traditionally designed to handle QuakeML.
    

In summary, the proposed changes and implementations in this RFC stem from a direct need to address the unique challenges posed by μseismic monitoring, especially in mining contexts. They reflect both the evolution of seismic data interpretation methodologies and the tools used in the industry. Through these proposals, we aim to establish a more streamlined, precise, and standardized approach to μseismic data management and interpretation.











<!--stackedit_data:
eyJoaXN0b3J5IjpbOTg2OTUxNjc2LC0xMjg4MTMxNjksLTM4OT
QzNTk5MywtNzU0MzcxODE5LC0xOTg3MDQzMDk5LC0xOTQ1NzI3
ODU4LC0zMTIwMjgyMzgsNDYzNjg0NDk3LC0xNjE2MTczNTgyLD
EwNDQ0MDUxNTQsLTE2NDU5MTcwOTQsODQwMTQ1MDU5LC0xNDMx
ODkwMzkzLC0xMTc3MjI3OTQ3LDEwODEwMTc2NjYsMTEyNDExND
E5MywtNzEyNDE5MTkxLDE1NDYyMjcxOTIsMTAxNzY1MDkwOSw1
NTQ3NTcwMDddfQ==
-->