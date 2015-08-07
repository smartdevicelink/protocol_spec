## 1. SmartDeviceLink Protocol
**Note: this is a work in progress. I will remove this note when I believe this document is accurate**

The SmartDeviceLink protocol specification describes the method for establishing communication between an application and head unit and registering the application for continued communication with the head unit

## 2. Frames
All transported data is formed with a header followed by an optional payload. The combination of header and payload is referred to as a frame.
### 2.1 Version 1 Frame Header
<table>
  <tr>
    <th colspan="3" width="25%">Byte 1</th>
    <th width="25%">Byte 2</th>
    <th width="25%">Byte 3</th>
    <th width="25%">Byte 4</th>
  </tr>
  <tr>
    <td width="12.5%">Version</td>
    <td width="3.125%">C</td>
    <td width="9.375%">Frame Type</td>
    <td>Service Type</td>
    <td>Frame Info</td>
    <td>Session ID</td>
  </tr>
</table>

<table>
  <tr>
    <th width="250">Byte 5</th>
    <th width="25%">Byte 6</th>
    <th width="25%">Byte 7</th>
    <th width="25%">Byte 8</th>
  </tr>
  <tr>
    <td colspan="4" align="center">Data Size</td>
  </tr>
</table>

### 2.2 Version 2 Frame Header

<table>
  <tr>
    <th colspan="3" width="25%">Byte 1</th>
    <th colspan="1"width="25%">Byte 2</th>
    <th width="25%">Byte 3</th>
    <th width="25%">Byte 4</th>
  </tr>
  <tr>
    <td width="12.5%">Version</td>
    <td width="3.125%">E</td>
    <td width="9.375%">Frame Type</td>
    <td>Service Type</td>
    <td>Frame Info</td>
    <td>Session ID</td>
  </tr>
</table>

<table >
  <tr>
    <th width="300">Byte 5</th>
    <th width="25%">Byte 6</th>
    <th width="25%">Byte 7</th>
    <th width="25%">Byte 8</th>
  </tr>
  <tr>
    <td colspan="4" align="center">Data Size</td>
  </tr>
</table>

<table>
  <tr>
    <th width="300">Byte 9</th>
    <th width="25%">Byte 10</th>
    <th width="25%">Byte 11</th>
    <th width="25%">Byte 12</th>
  </tr>
  <tr>
    <td colspan="4" align="center">Message ID</td>
  </tr>
</table>

### 2.3 Frame Header Fields
<table width="100%">
  <tr>
    <th>Field</th>
    <th>Size</th>
    <th width="75%">Description</th>
  </tr>
  <tr>
    <td>Version</td>
    <td>4 bit</td>
    <td>
      <b>Protocol Version</b><br>
      0x1 Protocol version 1 - uses a version 1 Frame Header<br>
      0x2 Protocol version 2 - uses a version 2 Frame Header<br>
      0x3 Protocol version 3 - uses a version 2 Frame Header<br>
      0x4 Protocol version 4 - uses a version 2 Frame Header<br>
      0x5 - 0xF Reserved
    </td>
  </tr>
  <tr>
    <td>C</td>
    <td>1 bit</td>
    <td><b>Compression Flag</b> <br> 0x0 This packet is not compressed <br> 0x1 This packet is compressed <br> <b>Note:</b> Only available in Protocol Version 1</td>
  </tr>
  <tr>
    <td>E</td>
    <td>1 bit</td>
    <td><b>Encryption Flag</b> <br> 0x0 This packet is not encrypted <br> 0x1 This packet is encrypted <br> <b>Note:</b> Only available in Protocol Version 2</td>
  </tr>
  <tr>
    <td>Frame Type</td>
    <td>3 bit</td>
    <td>
      0x00 Control Frame <br>
      0x01 Single Frame <br>
      0x02 First Frame <br>
      0x03 Consecutive Frame <br>
      0x04 - 0x07 Reserved
    </td>
  </tr>
  <tr>
    <td>Service Type</td>
    <td>8 bit</td>
    <td>
      0x00 Control Service<br>
      0x01 - 0x06 Reserved<br>
      0x07 Remote Procedure Call (RPC) Service<br>
      0x08 - 0x09 Reserved<br>
      0x0A PCM Service<br>
      0x0B Video Service<br>
      0x0C - 0x0E Reserved<br>
      0x0F Bulk Data (Hybrid Service)<br>
      0x10 - 0xFF Reserved
    </td>
  </tr>
  <tr>
    <td>Frame Info</td>
    <td>8 bit</td>
    <td>
      <b>Frame Type = 0x00 (Control Frame)</b><br>
      0x00 Heartbeat<br>
      0x01 Start Service<br>
      0x02 Start Service ACK<br>
      0x03 Start Service NACK<br>
      0x04 End Service<br>
      0x05 End Service ACK<br>
      0x06 End Service NACK<br>
      0x07 - 0xFD Reserved<br>
      0xFE Service Data ACK<br>
      0xFF Heartbeat ACK<br>
      <b>Frame Type = 0x01 (Single Frame)</b><br>
      0x00 - 0xFF Reserved<br>
      <b>Frame Type = 0x02 (First Frame)</b><br>
      0x00 - 0xFF Reserved<br>
      <b>Frame Type = 0x03 (Consecutive Frame)</b><br>
      0x00 Last Frame<br>
      0x01 - 0xFF Frame Number
    </td>
  </tr>
  <tr>
    <td>Session ID</td>
    <td>8 bit</td>
    <td>The session identifier</td>
  </tr>
  <tr>
    <td>Data Size</td>
    <td>32 bit</td>
    <td>
      <b>Frame Type = 0x00 (Control Frame)</b><br>
      0x0 - 0xFFFFFFFF reserved.<br>
      <b>Frame Type = 0x02 (First Frame)</b><br>
      0x08 The data size for a first frame is always 8 bytes. In the payload, the first four bytes denote the Total Size of the data contained in all consecutive frames, and the second four bytes denotes the number of consecutive frames following this one<br>
      <b>Frame Type = 0x01 or 0x03 (Single or Consecutive Frame)</b><br>
      The total bytes in this frame's payload
    </td>
  </tr>
  <tr>
    <td>Message ID</td>
    <td>32 bit</td>
    <td>The message identifier, used to uniquely identify this message</td>
  </tr>
</table>

### 2.4 Structured Payloads with Subtags
Structured payloads will contain an aribtrary number of sets of subtags. A subtag is formed as follows. The total length of the payload is still determined using the data size in the frame header.

<table>
  <tr width="100%">
    <th>Byte 1</th>
  </tr>
  <tr>
    <td>Subtag ID</td>
  </tr>
  <tr>
    <th>Byte 2</th>
  </tr>
  <tr>
    <td>Number of Octets (n)</td>
  </tr>
  <tr>
    <th>Bytes 3 through n</th>
  </tr>
  <tr>
    <td>Long form length (number of data octects)</td>
  </tr>
  <tr>
    <th>Bytes n + 1 through n + number of data octects</th>
  </tr>
  <tr>
    <td>Data</td>
  </tr>
</table>

The first octect is the subtag ID. This is used to identify how the data should be used. For example, the subtag ID for MTU Size tells the mobile device to interpret the data as the number of bytes to use for the MTU Size for a given transport.

The next octect gives the number of octects, in long form (base 256), which will define the length of the data for the subtag. For example, a value of 00000010 means that the next 2 octects, when intepreted, will output the length of the data.

Once the number of length octects are determined, you can determine the number of data octects. The most significant bit for the long form data length is always first.

If `n` is the number of length octects and `data` is an array of bytes containing the long form data length, the total data length can be determined as

```C
int length = 0;
for (int i = 0; i < n; i++) {
  length += data[i] * pow(256, n - i - 1);
}
```

#### 2.4.1 Available Subtags

The following subtags are currently available for use.

<table width="100%">
  <tr>
    <th>id</th>
    <th>name</th>
    <th>description</th>
  </tr>
  <tr>
    <td>0x01</td>
    <td>MTU Size</td>
    <td>The MTU Size which should be used by the mobile device</td>
  <tr>
  <tr>
    <td>0x02</td>
    <td>Hash ID</td>
    <td>A hash ID unique to the service being connected and the application connecting to that service</td>
  </tr>
</table>

## 3. Establishing Communication

### 3.1 Transport Layer
>Required: All Protocol Versions

A physical transport must be established between a head unit and an application.

### 3.2 Version Negotiation
>Required: All Protocol Versions

Once a transport is established, each application must negotiate the maximum supported protocol version with the head unit. To establish basic communication and register with the head unit, the application must start an RPC service (Service Type: 0x07), using a *Version 1 Protocol Header*.

##### Application -> Head Unit
<table width="100%">
  <tr>
    <td>Version</td>
    <td>C</td>
    <td>Frame Type</td>
    <td>Service Type</td>
    <td>Frame Info</td>
    <td>Session Id</td>
    <td>Data Size</td>
  </tr>
  <tr>
    <td></td>
    <td>no</td>
    <td>Control</td>
    <td>RPC</td>
    <td>Start Service</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>0b0001</td>
    <td>0b0</td>
    <td>0b000</td>
    <td>0x07</td>
    <td>0x01</td>
    <td>0x00</td>
    <td>0x00000000</td>
  </tr>
</table>

#### 3.2.1 Success

If the head unit can start the RPC service it will respond with a Start Service ACK containing its maximum supported protocol version. If its maximum supported version is 1, the packet will contain an 8 byte header, otherwise it will contain a 12 byte header. From this point forward, the application is expected to use the minimum of the head unit's maximum supported version, and its maximum supported version. The payload of the Start Service ACK will be structured with subtags to contain information such as a hash id for the service unique to the application which started it, and the mtu size which should be used by the application to continue communication henceforth.

##### Head Unit -> Application
<table width="100%">
  <tr>
    <td>Version</td>
    <td>E</td>
    <td>Frame Type</td>
    <td>Service Type</td>
    <td>Frame Info</td>
    <td>Session Id</td>
    <td>Data Size</td>
    <td>Message ID</td>
  </tr>
  <tr>
    <td></td>
    <td>no</td>
    <td>Control</td>
    <td>RPC</td>
    <td>Start Service ACK</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>0b0100</td>
    <td>0b0</td>
    <td>0b000</td>
    <td>0x07</td>
    <td>0x02</td>
    <td>0x00</td>
    <td>0x00000000</td>
    <td>0x00000000</td>
  </tr>
</table>

#### 3.2.2 Failure
If a session has already been started, or can't be started, a NACK will be sent in response to the Start Service packet.

##### Head Unit -> Application
<table width="100%">
  <tr>
    <td>Version</td>
    <td>E</td>
    <td>Frame Type</td>
    <td>Service Type</td>
    <td>Frame Info</td>
    <td>Session Id</td>
    <td>Data Size</td>
    <td>Message ID</td>
  </tr>
  <tr>
    <td></td>
    <td>no</td>
    <td>Control</td>
    <td>RPC</td>
    <td>Start Service NACK</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>0b0100</td>
    <td>0b0</td>
    <td>0b000</td>
    <td>0x07</td>
    <td>0x03</td>
    <td>0x00</td>
    <td>0x00000000</td>
    <td>0x00000000</td>
  </tr>
</table>

### 3.3 Registration
>Required: All Protocol Versions

Each application registers for continued communication with the head unit by sending a RegisterAppInterface Request RPC to the head unit via the RPC Service. Additional services can only be started after a successful RegisterAppInterface Response RPC has been sent from the head unit to the application.

### 3.4 Heartbeat
>Required: Protocol Versions 3 and higher

After a successful start service exchange between the application and head unit both the application and head unit are required to be able to respond to heartbeat messages if the negotiated protocol version is 3 or higher. After sending a heartbeat, if the application or head unit does not respond within a timeout (custom per app/head unit), the sender will disconnect. The sender's timer for the heartbeat timeout should be reset every time any message is received. Heartbeats are sent using the Control Service Type (0x00)

#### 3.4.1 Heartbeat Request

>Note the request can originate from either the Head Unit or the Application

##### Head Unit -> Application
<table width="100%">
  <tr>
    <td>Version</td>
    <td>E</td>
    <td>Frame Type</td>
    <td>Service Type</td>
    <td>Frame Info</td>
    <td>Session Id</td>
    <td>Data Size</td>
    <td>Message ID</td>
  </tr>
  <tr>
    <td></td>
    <td>no</td>
    <td>Control</td>
    <td>Control</td>
    <td>Heartbeat</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>0b0100</td>
    <td>0b0</td>
    <td>0b000</td>
    <td>0x00</td>
    <td>0x00</td>
    <td>0x00</td>
    <td>0x00000000</td>
    <td>0x00000000</td>
  </tr>
</table>

#### 3.4.2 Heartbeat ACK

>Note the response ACK will originate from the Head Unit or the Application based on the origin of the request

##### Application -> Head Unit
<table width="100%">
  <tr>
    <td>Version</td>
    <td>E</td>
    <td>Frame Type</td>
    <td>Service Type</td>
    <td>Frame Info</td>
    <td>Session Id</td>
    <td>Data Size</td>
    <td>Message ID</td>
  </tr>
  <tr>
    <td></td>
    <td>no</td>
    <td>Control</td>
    <td>Control</td>
    <td>Heartbeat ACK</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>0b0100</td>
    <td>0b0</td>
    <td>0b000</td>
    <td>0x00</td>
    <td>0xFF</td>
    <td>0x00</td>
    <td>0x00000000</td>
    <td>0x00000000</td>
  </tr>
</table>

#### 3.4.3 Heartbeat NACK
There is no heartbeat NACK.

## 4. Services
Messages sent have a priority based on their Service Type. Lower values for service type have higher delivery priority. A message's payload's format is based on the different service types defined below.

### 4.1 RPC Service
>Required: All Protocol Versions

The RPC service is used to send requests, responses, and notifications between an application and a head unit. Valid messages are defined in the [RPC Specification](https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/MOBILE_API.xml).

The payload of a message sent via the RPC service, which directly follows the Frame Header in the packet, consists of a Binary Header, and JSON data representing the RPC.

##### RPC Payload

<table>
  <tr>
    <td align="center" width="5000">Binary Header</td>
  </tr>
  <tr>
    <td align="center">JSON Data</td>
  </tr>
</table>

#### 4.1.2 Binary Header
>Available: Protocol Version 2 and greater

<table>
  <tr>
    <th colspan="2" width="25%">Byte 1</th>
    <th width="25%">Byte 2</th>
    <th width="25%">Byte 3</th>
    <th width="25%">Byte 4</th>
  </tr>
  <tr>
    <td width="12.5%">RPC Type</td>
    <td colspan="4" align="center">RPC Function ID</td>
  </tr>
  <tr>
    <td colspan="5" align="center">Correlation ID</td>
  </tr>
  <tr>
    <td colspan="5" align="center">JSON Size</td>
  </tr>
</table>

##### 4.1.2.1 Binary Header Fields
<table>
  <tr>
    <th>Field</th>
    <th>Size</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>RPC Type</td>
    <td>4 bit</td>
    <td>
      0x0 Request<br>
      0x1 Response<br>
      0x2 Notification<br>
      0x3 - 0xF Reserved
    </td>
  </tr>
  <tr>
    <td>RPC Function ID</td>
    <td>28 bit</td>
    <td>The Function ID of each RPC is specific to each version of the <a href="https://github.com/smartdevicelink/sdl_core/blob/develop/src/components/interfaces/MOBILE_API.xml#L2146-2207">RPC Specification</a> but in general do not change from version to version.
  </tr>
  <tr>
    <td>Correlation ID</td>
    <td>32 bits</td>
    <td>The Correlation ID is used to map a request to its response. In Protocol Version 1, when the Binary Header did not exist, the Correlation ID was included as part of the JSON and has a max value of 65536</td>
  </tr>
  <tr>
    <td>JSON Size</td>
    <td>32 bits</td>
    <td>The size of the JSON Data following the Binary Header in the RPC Payload</td>
  </tr>
</table>

### 4.2 Hybrid (Bulk Data) Service
>Required: Protocol Version 2 and greater

The Hybrid Service does not need to be explicitly started, all applications that have successfully registered have access to the Hybrid Service.

The Hybrid Service is similar to the RPC Service but adds a bulk data field. The payload of a message sent via the Hybrid service consists of a Binary Header, JSON Data, and Bulk Data.

The size of the Bulk Data field is the Data Size (Found in the Frame Header) minus the 12 Bytes of the Binary Header minus the JSON Size (Found in the Binary Header).

The Binary Header of a message using the Hybrid Service is the same as the Binary Header of a message using the RPC Service.

##### Hybrid Service Payload

<table>
  <tr>
    <td align="center" width="5000">Binary Header</td>
  </tr>
  <tr>
    <td align="center">JSON Data</td>
  </tr>
  <tr>
    <td align="center">Bulk Data</td>
  </tr>
</table>

### 4.3 Audio Service (PCM)
>Available: Protocol Version 3 and greater

The application can start the audio service to send PCM audio data to the head unit. The payload for the Audio Service is only PCM audio data.

### 4.4 Video Service (H.264)
>Available: Protocol Version 3 and greater

The application can start the video service to send H.264 video data to the head unit. The payload for the Video Service is only H.264 video data.

## 5 Ending Communication
To close out a communication session with the head unit, an application sends
