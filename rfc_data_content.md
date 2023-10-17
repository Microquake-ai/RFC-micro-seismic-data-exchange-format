
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

The catalog includes information related to an event or trigger or a series of events. We suggest storing the catalog information in a QuakeML like format adapted to &mu;seismic data (see QuakeML documentation [documentation](https://quake.ethz.ch/quakeml)). The suggested changes affect the following QuakeML objects:

- **Event** &mdash; The event_type field is restricted to specific values that are not suited for &mu;seismic monitoring  
- **Origin** &mdash; The position is expressed in latitude and longitude. This will need to be changed to x, y and z.
- **Magnitude** &mdash; The magnitude object would benefit from adding a field to store the corner frequency, P-wave and S-wave Energy. This would allow for the a large range of source parameters to be computed on the fly and not stored in the magnitude object.  This approach what is typically done in mine seismology. 

#### Event &mdash; Event Type

There are two approaches to modifying the event types
1. Redfine the schema and allow for event type related to mining to be stored in a &mu;QuakeML file; and
2. Map each &mu;seismic type to the an existing QuakeML event type

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

The above mapping is implemented in the &mu;Quake library.

#### Origin &mdash; Coordinate System

The QuakeML format was designed to store seismic data at a regional and global scale. Consequently, the QuakeML format has adopted a spherical coordinate system storing the location information in latitude and longitude. 

Using Latitude and Longitude to describe the mining related events is possible but not convenient. It would require precisely knowing the series of transformations required to convert the position expressed in the local coordinate system into longitude and latitude. We suggest, overriding the Latitude and Longitude information and use x, y and z instead. The use of x, y and z notation instead of the right handed (easting, northing and elevation) or (northing, easting, down) tripplets is to provide flexibility notation and ensure consistency between the mine and the seismic system coordinate system.

The &mu;Quake implementation exploits the QuakeML extra parameters to store the x, y and z values and associated errors.

#### Magnitude &mdash; Corner Frequency, and Energy

The QuakeML standard does not include objects suited to store the corner frequency. The energy could be stored in the amplitude or station_magnitude object. This is not convenient, however, as it is preferrable to for the magnitude and energy information to be used in tandem. 

The  &mu;Quake implementation stores the `corner_frequency`, the `energy_p`, `energy_s` and associated error along side the magnitude information in the extra parameter of the Magnitude object. 

#### Catalog Information Packaging

##### Zarr 
The catalog information can easily be packaged with the waform in a `Zarr` file. The QuakeML simply needs to be serialized using the json library. The example below show how an `Obspy` or `uQuake` catalog can be stored in a `Zarr` file and retrieved:

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

The file can also be strore in the native XML format.

### System or Inventory information








<!--stackedit_data:
eyJoaXN0b3J5IjpbNDYzNjg0NDk3LC0xNjE2MTczNTgyLDEwND
Q0MDUxNTQsLTE2NDU5MTcwOTQsODQwMTQ1MDU5LC0xNDMxODkw
MzkzLC0xMTc3MjI3OTQ3LDEwODEwMTc2NjYsMTEyNDExNDE5My
wtNzEyNDE5MTkxLDE1NDYyMjcxOTIsMTAxNzY1MDkwOSw1NTQ3
NTcwMDcsMTcxNDk5ODI0MCwtNDY2MjgwNjUwLDE2MzAxNTI3Mj
QsLTEzNzM3MDIzNTcsLTEzODU5NzAzNTBdfQ==
-->