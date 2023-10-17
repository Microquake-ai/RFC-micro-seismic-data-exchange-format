# Request For Comment &mdash; Content for &mu;Seismic Data Exchange

## Introduction

### Purpose

The purpose of this document is to invite comments on a suggested format to allow standardized and consistent access to the &mu;seismic data collected by mine &mu;seismic system and enable seamless and lossless exchange between different platforms. 

Access to the &mu;seismic data is not consistent between the different vendors, and sometimes between different implementation by the same vendors. There are variations from site to site, often driven by third party requirements. This leads to inefficiencies that make the integration of different systems providing complementary products and services and the usage of data unnecessarily difficult. 

The current document does proposed a file structure and format. At this stage, we seek alignment regarding the content of the information transferred and agreement on the container. 

### Scope

The scope is to define the content and nature &mu;seismic information provided to third parties and suggest a format to package the information. This document is concerned with the building blocks and seismic information that are the essence of &mu;seismic monitoring. The information of concern can be classified in three categories:

- **Waveform** &mdash; The raw unprocessed waveform both for the continuous and triggered data.
- **Catalog Information** &mdash; The event information derived from the processing of the waveform data
- **Inventory or System Information** &mdash; This includes the information regarding the instruments (sensor and data acquisition modules) along the data acquisition chain and other information critical to processing the data (e.g., velocity, density and attenuation values or models).

### Rationale

#### Need for a New Standard

Microseismic data plays a important role in ensuring the safety and enabling the efficiency of underground mining operations. While it's one of several essential tools in the arsenal of modern mining techniques, the current scenario of diverse data formats can sometimes complicate seamless integration and analysis.

#### Goal

The proposed data content and a packaging format to promote interoperability, consistency and innovations.

## Proposal

### Overview

Our proposal concerns three categories of data, the waveforms, the catalog data and the inventory and system information. We propose storing this information in a unique `Zarr` file with a `.zarr` extension. 

To ensure interoperability, the information in the provided files shall be consistent. The sensor naming convention shall be the same all across, the locations of sensors and events shall be expressed using one single coordinate system, which should also be used for the grids, if applicable.

### Why `Zarr` format

Using the `Zarr` format to package the information was suggested by personnel at the IRIS data center. IRIS now encourage submission using the `TimeDB` or `Zarr` format. Storing the data to `TileDB` require a database engine and would therefore require additional components to be installed alongside the server.  

The `Zarr` format was developed to efficiently store and access large-scale array-oriented scientific data. Its design specifically addresses the challenges posed by cloud and distributed storage by allowing for concurrent reads and writes. The format excels in scenarios where data needs to be analyzed in chunks without reading the entire dataset into memory, making it especially suitable for multidimensional arrays. With built-in support for compression and chunking, Zarr enables high-speed data access regardless of the storage backend, be it file systems, object storage, or databases.

Adopting `Zarr` allows for the data and metadata to be stored in format that are 

The main advantage of adopting the `Zarr` format to store the event data are particularly pronounced for the waveforms:

1.  **Chunked Storage and Access**: Zarr's inherent design supports chunking, allowing users to efficiently read or write small sections of large seismic datasets without needing to access the entire file. This is particularly beneficial for processing long-duration continuous seismic recordings.
    
2.  **Concurrent Reads and Writes**: Zarr is built for cloud and distributed storage environments, making it feasible for multiple processes or even multiple users to read and write data simultaneously without conflicts, facilitating collaborative analysis.
    
3.  **Flexible Compression**: Zarr supports a variety of compression algorithms. This means seismic data, which can be extensive, can be efficiently compressed to save storage space while maintaining quick access times.
    
4.  **Multidimensional Support**: Seismic data often comes in multi-dimensional arrays (e.g., time, depth, latitude, longitude). Zarr natively handles multidimensional datasets, streamlining data organization and access.
    
5.  **Metadata Storage**: Zarr allows for the inclusion of metadata directly within the dataset. This capability ensures that seismic trace metadata, acquisition details, and any processing history can be stored alongside the waveform data, ensuring comprehensive context for the stored seismic information.

### Waveform data

The waveform data is the raw vibration recorded directly by the sensors. For convenience, the waveform data can be provided in physical units native to the instrument recording the data of $m$, ${m}/{s}$, or $m/s^2$ for displacement, velocity and acceleration, respectively. However, if size is of concern, storing the ADC count is more appropriate. Storing the ADC count represented as integers allow the usage of the Steim1 and Steim2 differential compression algorithms. 

Along the amplitude values additional metadata describing the instrument recording the data and time series parameter should be provided for each trace. Each trace shall be stored with its own metadata information

The required metadata for each trace are:

- **Trace Identifier**: A unique identifier for the trace (see Obspy [documentation](https://docs.obspy.org/master/packages/autogen/obspy.core.event.resourceid.ResourceIdentifier.html) for information on the recommended _resource identifier_ structure.
- **Location identification**: The location identification convention described [here](https://ds.iris.edu/ds/newsletter/vol1/no1/1/specification-of-seismograms-the-location-identifier/). The convention has been adapted for the purpose of &mu;seismic monitoring in the mining context. 
  - **Network Code [network_code]** &mdash; Code representing the network. 
  - **Station Code [station_code]** &mdash; Code that representing the station containing the digitizer. 
  - **Location Code [location_code]** &mdash; Code representing the instrument 
  - **Channel Code [channel_code]** &mdash; The three (3) alphanumerical code that represents the channel and shall follow the FDSN standard naming convention of August 2000 described in the SEED document [Appendix A](http://www.fdsn.org/pdf/SEEDManual_V2.4_Appendix-A.pdf). The first letter represents the band code, the second the instrument code and the third the orientation code. For instance, a typical $14 Hz$ or $15 Hz$ omnidirectional geophones code would be GH?, where ? would be replaced by the appropriate component orientation code. 
 - **Sampling Rate [sampling_rate]** &mdash; The signal sampling rate in sample per second.
 - **Calibration Factor [calib]** &mdash; The calibration factor this value is optional and will be set to 1.0 if not provided. This value represents the calibration factor should the sensor deviate from the typical response.
 - **Start Time [starttime]** &mdash; The trace start time.

#### Notes:

**Network, Station and Location Codes**: The above convention is usually not strictly followed by the &mu;seismic system suppliers. Flexibility in applying the convention is necessary. There is usually no distinction between the station and location, and each instrument receives a unique code that may or may not refer to the data acquisition station. In this case, we suggest using the **Station Code** field to store the _instrument_ code and  fill the location code using 01 or 00. In a network, each combination of **Station Code** &ndash; **Location Code** &ndash; **Channel Code** should be unique.

#### Waveform Data Packaging

##### MiniSEED

The _miniSEED_ format is widely adopted in seismology and is very convenient for storing seismic data. The _miniSEED_ format can accommodate traces from different instrument type (geophone, accelerometer, seismometer, etc.) acquired at a different sampling rate, with variable start times and end times. It is important to note, however, that to use _miniSEED_ the network, station, location, and channels code must adhere to a strict convention described below:

- **Network Code** &mdash; Two (2) alphanumerical characters
- **Station Code**: Five (5) alphanumerical characters
- **Location Code**: Two (2) alphanumerical characters
- **Channel Code**: Three (3) alphanumerical characters, following the FDSN guidelines of August 2000.

##### Alternative Formats
The IRIS DMC recommends the use of the `Zarr` formats (or TileDB) over the `HDF5` based format like the `ASDF` format. The `Zarr` format can conveniently be used to store the waveform data. Althought we strongly encourage the naming convention for the presented in the previous section to be followed givin each component a unique name that can be composed as follows `network_code.station_code.location_code.channel_code`.

**Note**: The `Zarr` file format could be extended to include the catalogue and inventory information.

Below is an example written in Python that allows the information contained in an _Obspy_ Stream object to be written in a `.zarr` file:

```python
import zarr
import obspy

def stream_to_zarr_group(stream, zarr_group_path):
    """
    Converts an ObsPy stream to a Zarr group.
    Each trace is stored as a separate Zarr array with its associated metadata.
    :param stream: ObsPy Stream object
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
st = obspy.read()

# Convert and store the stream in a Zarr group
stream_to_zarr_group(st, 'seismic_data_group.zarr')
```

### Catalog
The catalog includes information related to an event or trigger or a series of events. We suggest storing the catalog information in QuakeML like object slightly adapted to &mu;seismic data. convention and hierachy closely following the QuakeML standard [documentation](https://quake.ethz.ch/quakeml) but with some deviations to enable the simpler mapping of the information stored by the system. Also, we suspect there will be a strong preference to allow for the exchange format to 

We propose to organize the catalog information as follows
- **Level 0**: Catalog
  - **Level 1**: Events &mdash; an array of events
    - **Level 2**: Origin &mdash; contains the information related to the location (an array in QuakeML)
    - **Level 2**: Magnitude &mdash; contains the information related to the magnitude (an array in QuakeML)
    - **Level 2**: Picks &mdash; an array representing all the _P_ and _S_ picks
    - **Level 2**: Focal Mechanism/Moment Tensor &mdash; contains a focal mechanism associated to the event.






<!--stackedit_data:
eyJoaXN0b3J5IjpbMTI2NzMwMjg1OCwxMDgxMDE3NjY2LDExMj
QxMTQxOTMsLTcxMjQxOTE5MSwxNTQ2MjI3MTkyLDEwMTc2NTA5
MDksNTU0NzU3MDA3LDE3MTQ5OTgyNDAsLTQ2NjI4MDY1MCwxNj
MwMTUyNzI0LC0xMzczNzAyMzU3LC0xMzg1OTcwMzUwXX0=
-->