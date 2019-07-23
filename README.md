# SmartDeviceLink Protocol

**Current Version: 5.2.0**

## 1. Overview
The SmartDeviceLink protocol specification describes the method for establishing communication between an application and head unit and registering the application for continued communication with the head unit. The protocol is used as the base formation of packets sent from one module to another. 

All new SDL implementations should implement the newest version of the protocol.

### 1.1 Common Terms

| Term | Description |
|------|-------------|
|**Module / Head Unit**| Hardware implementing the sdl_core software|
|**Application**| Smart device application that implements the proxy library (iOS or Android)|

## 2. Frames
All transported data is formed with a header followed by an optional payload. The combination of header and payload is referred to as a frame.

### 2.1 Version 1 Frame Header
>Deprecated: Protocol versions 2 and higher. Only used as initial `StartService` packet for establishing communication and version negotiation from application


<table>
  <tr>
    <th colspan="3" width="25%">Byte 1</th>
    <th width="25%">Byte 2</th>
    <th width="25%">Byte 3</th>
    <th width="25%">Byte 4</th>
  </tr>
  <tr align="center">
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
>Required: Protocol versions 2 and higher

<table>
  <tr>
    <th colspan="3" width="25%">Byte 1</th>
    <th colspan="1"width="25%">Byte 2</th>
    <th width="25%">Byte 3</th>
    <th width="25%">Byte 4</th>
  </tr>
  <tr align="center">
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
    <td><b>Encryption Flag</b> <br> 0x0 This packet is not encrypted <br> 0x1 This packet is encrypted <br> <b>Note:</b> Only available in Protocol Version 2 and higher</td>
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
      0x0A Audio Service<br>
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
      0x03 Start Service NAK<br>
      0x04 End Service<br>
      0x05 End Service ACK<br>
      0x06 End Service NAK<br>
      0x07 Register Secondary Transport<br>
      0x08 Register Secondary Transport ACK<br>
      0x09 Register Secondary Transport NAK<br>
      0x0A - 0xFC Reserved<br>
      0xFD Transport Event Update<br>
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
      0x08 The data size for a first frame is always 8 bytes. In the payload, the first four bytes denote the Total Size of the data contained in all consecutive frames, and the second four bytes denote the number of consecutive frames following this one<br>
      <b>Frame Type = 0x01 or 0x03 (Single or Consecutive Frame)</b><br>
      The total bytes in this frame's payload
    </td>
  </tr>
  <tr>
    <td>Message ID</td>
    <td>32 bit</td>
    <td>The message identifier, used to uniquely identify this message.<br> 
    <b>Note:</b> Only included in protocol version 2 frame headers and higher</td>
  </tr>
</table>

### 2.4 Max Transport Units
The max transport unit (MTU) of a frame varies based on version. The MTU includes the frame header and payload. The current supported versions and their MTU's respectively are described below.

| Version | MTU (bytes) |
|------|-------------|
|**1**| 1500|
|**2**| 1500|
|**3**| 131,084 |
|**4**| 131,084|
|**5**| 131,084 default or negotiated *(See Control Frame Payloads)*


#### 2.4.1 Payload Size
The payload size is determined by the MTU - Frame Header Size. 

| Version | Max Payload Size (bytes) |
|------|-------------|
|**1**| 1488|
|**2**| 1488|
|**3**| 131,072 |
|**4**| 131,072|
|**5**| 131,072 default or (Negotiated MTU - 12 bytes) *(See Control Frame Payloads)*

#### 2.4.2 Encrypted MTU
While the supported MTU is the maximum size for that version, if a frame is encrypted it will be subject to the MTU of that encryption protocol as well. That means the MTU will have to be the minimum between SDL's MTU  and the encryption protocol's MTU. 

## 3. Frame Types

### 3.1 Control Frame 
Control frames are the lowest-level type of packets. They can be sent over any of the defined services. They are used for the control of the services in which they are sent.

#### 3.1.1 Special Header Definitions:
| Header Value |Expected values| Description |
|--------------|---------------|-------------|
|Frame Info| `0x00` - `0x06`, `0xFE`, `0xFF`|See below "Frame Info Definitions"|
|Data Size| `0x00`, `0x04`|`0x00` - Majority of control packets do not have payloads<br><br> `0x04` - Used for `StartServiceACK` where the payload is a HashID |


#### 3.1.2 Frame Info Definitions:
| Frame Info Value| Name | Description |
|------------|------|-------------|
| 0x00| Heartbeat| A ping packet that is sent to ensure the connection is still active and the service is still valid|
| 0x01 | Start Service |Requests that a specific type of service is started |
| 0x02 | Start Service ACK | Acknowledges that the specific service has been started successfully
| 0x03 | Start Service NAK | Negatively acknowledges that the specific service was *not* started
| 0x04 | End Service | Requests that a specific type of service is ended
| 0x05 | End Service ACK | Acknowledges that the specific service has been ended successfully
| 0x06 | End Service NAK |  Negatively acknowledges that the specific service was *not* ended or has not yet been started
| 0x07 | Register Secondary Transport | Request for a session registered on a primary transport to use a secondary transport. <br>This frame should only be sent on the Secondary Transport that the session is requesting.
| 0x08 | Register Secondary Transport ACK | Acknowledges that the supplied session is registered to use the requested Secondary Transport. The application is only allowed to send additional frames on the Secondary Transport after this frame is received. <br>This frame must be sent on the Secondary Transport in which the original request was sent. 
| 0x09 | Register Secondary Transport NAK | Negatively acknowledges that the session is not registered or able to use the current Secondary Transport. The application cannot use this transport for any other messages. <br>This frame must be sent on the Secondary Transport in which the original request was sent. 
| 0xFD | Transport Event Update | Indicates that status or configuration of one or more transports are updated. This frame must only be sent after the successful starting of the RPC service which includes the protocol version negotiation.
| 0xFE | Service Data ACK | *Deprecated*
| 0xFF | Heartbeat ACK | Acknowledges that a Heartbeat control packet has been received

#### 3.1.3 Payloads
>Added: Protocol Version 5<br>
>*Note: All parameters are optional*<br>

Control frames use [BSON](http://bsonspec.org) to store payload data. All payload types are directly from the BSON spec. Each control frame info type will have a defined set of available data. Most types will also have differently available data based on their service type.

If there is no data to send for a given parameter, the parameter should not be included. 

**Note:** Heartbeat, Heartbeat ACK, and Service Data ACK control frame types are not covered for any service as they were deprecated before payloads were introduced.

##### 3.1.3.1 Control Service

###### 3.1.3.1.1 Register Secondary Transport

>No parameters

###### 3.1.3.1.2 Register Secondary Transport ACK

>No parameters

###### 3.1.3.1.3 Register Secondary Transport NAK

| Tag Name | Type | Introduced | Description |
|----------|------|------------|-------------|
| reason | string | 5.1.0 | Specify a string describing the reason of failure |

###### 3.1.3.1.4 Transport Event Update

| Tag Name | Type | Introduced | Description |
|----------|------|------------|-------------|
| tcpIpAddress | String | 5.1.0 | IP address that can be used to establish a TCP connection. It can be IPv4 address (example: "192.168.1.1") or IPv6 address (example: "fd12:3456:789a::1").<br>An empty string indicates that the TCP transport becomes unavailable. |
| tcpPort | int32 | 5.1.0 |  TCP Port number that can be used along with the supplied `tcpIpAddress` to establish a TCP connection. If parameter is included, the `tcpIpAddress` parameter must also be included.|

##### 3.1.3.2 RPC Service

###### 3.1.3.2.1 Start Service
**Note:** While this includes a payload, it will remain a v1 frame header to ensure backwards compatibility with older systems.

| Tag Name | Type | Introduced | Description |
|----------|------|------------|-------------|
|protocolVersion|String| 5.0.0 | The max version of the protocol supported by client requesting service to start. Must be in the format *"Major.Minor.Patch"*|

###### 3.1.3.2.2 Start Service ACK
| Tag Name | Type | Introduced | Description |
|----------|------|------------|-------------|
|protocolVersion|String| 5.0.0 |The negotiated version of the protocol. Must be in the format *"Major.Minor.Patch"*. The frame header version should match the major version exactly.|
|hashId|int32| 5.0.0 | Hash ID to identify this session and used when sending an `EndService` control frame for the RPC service type|
|mtu| int64 | 5.0.0 | Max transport unit to be used for this service|. If not included the client should use the protocol version default.|
|secondaryTransports|String Array| 5.1.0 | Array of transport types which are allowed to be used as a Secondary Transport. Refer to the table below for possible values.<br>As of this protocol spec version (5.1.0) only a single Secondary Transport may be used beyond a primary transport for a given session.<br>If there are no currently available Secondary Transports or the functionality is not supported, this parameter should be omitted or be an empty array.|
|audioServiceTransports|int32 array| 5.1.0 | Ordered list of transport priority types that support the audio service (`0x0A`). Only the values of `1` ("Primary Transport") or `2` ("Secondary Transport") shall be used. If both the primary and secondary transport support the audio service, both should be included (`1` and `2`) in the order they are preferred; otherwise only the single transport priority type should be included. An application must not start the audio service on a transport priority type that is not listed in the array.<br>If this parameter is not included the Primary Transport should be used for the audio service.|
|videoServiceTransports|int32 array| 5.1.0 | Ordered list of transport priority types that support the video service (`0x0B`). Only the values of `1` ("Primary Transport") or `2` ("Secondary Transport") shall be used. If both the primary and secondary transport support the video service, both should be included (`1` and `2`) in the order they are preferred; otherwise only the single transport priority type should be included. An application must not start the video service on a transport priority type that is not listed in the array.<br>If this parameter is not included the Primary Transport should be used for the video service.|
|authToken|String| 5.2.0 | Included exclusively when communicating with cloud applications. This token is used by a cloud application to authenticate a user account associated with the vehicle. |

**list of transport type strings**

| String | Description |
|--------|-------------|
| IAP\_BLUETOOTH | iAP over Bluetooth |
| IAP\_USB | iAP over USB where it is not possible to distinguish between host or device mode|
| IAP\_USB\_HOST\_MODE | iAP over USB, and the phone is running as host |
| IAP\_USB\_DEVICE\_MODE | iAP over USB, and the phone is running as device |
| IAP\_CARPLAY | iAP over Carplay wireless |
| SPP\_BLUETOOTH | Bluetooth SPP. |
| AOA\_USB | Android Open Accessory |
| TCP\_WIFI | TCP connection over Wi-Fi |

###### 3.1.3.2.3 Start Service NAK
| Tag Name | Type | Introduced | Description |
|----------|------|------------|-------------|
| rejectedParams |String Array| 5.0.0 | An array of rejected parameters|

###### 3.1.3.2.4 End Service
| Tag Name | Type | Introduced | Description |
|----------|------|------------|-------------|
|hashId|int32| 5.0.0 | Hash ID supplied in the `StartServiceACK` for this service type|

###### 3.1.3.2.5 End Service ACK

###### 3.1.3.2.6 End Service NAK
| Tag Name | Type | Introduced | Description |
|----------|------|------------|-------------|
| rejectedParams |String Array| 5.0.0 | An array of rejected parameters such as: [`hashId`] |


##### 3.1.3.3 Audio Service
###### 3.1.3.3.1 Start Service
>No parameters

###### 3.1.3.3.2 Start Service ACK
| Tag Name | Type | Introduced | Description |
|----------|------|------------|-------------|
|mtu| int64 | 5.0.0 | Max transport unit to be used for this service. If not included the client should use the one set via the RPC service or protocol version default.|

###### 3.1.3.3.3 Start Service NAK
| Tag Name | Type | Introduced | Description |
|----------|------|------------|-------------|
| rejectedParams |String Array| 5.0.0 | An array of rejected parameters such as: [`videoProtocol`, `videoCodec`] |

###### 3.1.3.3.4 End Service
>No parameters

###### 3.1.3.3.5 End Service ACK
>No parameters

###### 3.1.3.3.6 End Service NAK
| Tag Name | Type | Introduced | Description |
|----------|------|------------|-------------|
| rejectedParams |String Array| 5.0.0 | An array of rejected parameters such as: [`hashId`] |

##### 3.1.3.4 Video Service

###### 3.1.3.4.1 Start Service
| Tag Name | Type | Introduced | Description |
|----------|------|------------|-------------|
|height|int32| 5.0.0 | Desired height from the client requesting the video service to start|
|width|int32| 5.0.0 | Desired width from the client requesting the video service to start|
|videoProtocol|String| 5.0.0 | Desired video protocol to be used. See `VideoStreamingProtocol ` RPC|
|videoCodec|String| 5.0.0 | Desired video codec to be used. See `VideoStreamingCodec` RPC|

###### 3.1.3.4.2 Start Service ACK
| Tag Name | Type | Introduced | Description |
|----------|------|------------|-------------|
|mtu| int64 | 5.0.0 | Max transport unit to be used for this service. If not included the client should use the one set via the RPC service or protocol version default.|
|height|int32| 5.0.0 | Accepted height from the client requesting the video service to start|
|width|int32| 5.0.0 | Accepted width from the client requesting the video service to start|
|videoProtocol|String| 5.0.0 | Accepted video protocol to be used. See `VideoStreamingProtocol ` RPC|
|videoCodec|String| 5.0.0 | Accepted video codec to be used. See `VideoStreamingCodec` RPC|


###### 3.1.3.4.3 Start Service NAK
| Tag Name | Type | Introduced | Description |
|----------|------|------------|-------------|
| rejectedParams |String Array| 5.0.0 | An array of rejected parameters such as: [`videoProtocol`, `videoCodec`] |

###### 3.1.3.4.4 End Service
>No parameters

###### 3.1.3.4.5 End Service ACK
>No parameters

###### 3.1.3.4.6 End Service NAK
| Tag Name | Type | Introduced | Description |
|----------|------|------------|-------------|
| rejectedParams |String Array| 5.0.0 | An array of rejected parameters such as: [`hashId`] |




### 3.2 Single Frame
A frame of type Single Frame contains all the data for a particular packet in the payload. The majority of frames sent over the protocol utilize this frame type.

<table width="100%">
  <tr>
  	<th colspan = "2" align="center">Single Frame</th>
  </tr>
  <tr>
    <td width="10%">Header</td>
    <td>Payload</td>
  </tr>
  <tr>
    <td width="20%" style="visibility:hidden;"></td>
    <td align="center">Data</td>
  </tr>
</table>

#### 3.2.1 Special Header Definitions:

| Header Value |Expected values| Description |
|--------------|---------------|-------------|
|Frame Info| `0x00`|Reserved|
|Data Size| 0x01-0xFFFFFFFF|Total payload size in bytes for this frame|
 
### 3.3 Multiple Frame Packets
Some payloads will be larger than the maximum transport unit will allow. If that is the case, the payload will be broken up over multiple frames. These frame types are First and Consecutive. 

<table width="100%">
  <tr style="visibility:hidden;">
    <td width="5%"></td>
    <td width="5%"></td>
    <td width="5%"></td>
    <td ></td>
    <td width="5%"></td>
    <td></td>
    <td width="5%"></td>
    <td></td>
    <td width="5%"></td>
    <td></td>
    <td></td>
  </tr>
    <tr>
    <td style="visibility:hidden;" colspan="3"></td>
    <td align="center" colspan="100%">Data</td>
  </tr>
  <tr>
  <td style="visibility:hidden;" colspan ="3"></td>
    <td align="center" colspan="2">Data Chunk 1 </td>
    <td align="center" colspan="2">Data Chunk 2 </td>
    <td style="border:0; background-color:white;" align="center" colspan="2">...</td>
    <td align="center" colspan="2">Data Chunk n </td>
  </tr>  
  <tr>
  	<th colspan = "2" align="center">First Frame</th>
  </tr>
  <tr>
    <td> Header</td>
    <td colspan = "1" >Payload</td>
  </tr>
  
  <tr>
     <th colspan = "2" style="visibility:hidden;"></th>
     <th colspan = "3">Consecutive Frame 1</th>
  </tr>
  <tr>
   <td colspan="2" style="visibility:hidden;"></td>
	<td>Header</td>
	<td colspan = "2" >Payload</td>  
  </tr>
  <tr>
     <th colspan = "4" style="visibility:hidden;"></th>
     <th colspan = "3">Consecutive Frame 2</th>
  </tr>
  <tr>
   <td colspan="4" style="visibility:hidden;"></td>
	<td>Header</td>
	<td colspan = "2" >Payload</td>  
  </tr>
  
  <tr style="border:0;">
     <td colspan = "7" style="visibility:hidden; border:0;"></td>
     <td style="font-size:25px; border:0; background-color:white;" align="center">...</td>
  </tr>
  <tr></tr>
  <tr>
     <th colspan = "8" style="visibility:hidden;"></th>
     <th colspan = "3">Consecutive Frame n</th>
  </tr>
  <tr>
   <td colspan="8" style="visibility:hidden;"></td>
	<td>Header</td>
	<td colspan = "2" >Payload</td>  
  </tr>

</table> 


#### 3.3.1 First Frame
The First Frame in a multiple frame payload contains information about the entire sequence of frames so that the receiving end can correctly parse all the frames and reassemble the entire payload. The payload of this frame is only eight bytes and contains information regarding the rest of the sequence.

##### 3.3.1.1 Payload:

<table width = "100%">
	<tr align="center">
		<th>Byte</th>
		<td width = "10%">0</td>
		<td width = "10%">1</td>
		<td width = "10%">2</td>
		<td width = "10%">3</td>
		<td width = "10%">4</td>
		<td width = "10%">5</td>
		<td width = "10%">6</td>
		<td width = "10%">7</td>
	</tr>
	<tr align="center">
		<td style="visibility:hidden;"></td>
		<td colspan ="4" >Total size of the original payload being parsed</td>
		<td colspan ="4" >Number of Consecutive Frames in this sequence</td>
	</tr>
</table>

##### 3.3.1.2 Special Header Definitions:

| Header Value |Expected values| Description |
|--------------|---------------|-------------|
|Frame Info| `0x00`|Reserved|
|Data Size| `0x08`|This frame contains a fixed data size (8 bytes) for the payload.|

#### 3.3.2 Consecutive Frame
The Consecutive Frames in a multiple frame payload contain the actual raw data of the original payload. The parsed payload contained in each of the Consecutive Frames' payloads should be buffered until the entire sequence is complete. 

##### 3.3.2.1 Special Header Definitions:

| Header Value |Expected values| Description |
|--------------|---------------|-------------|
|Frame Info| `0x00` - `0xFF`|Values `0x01` - `0xFF` are used incrementally as each consecutive frame is created and sent in the sequence. eg The first consecutive packet in the sequence will have the value `0x01`, the next consecutive frame that contains the next chunk of data in the sequence will have the value `0x02`. <br><br>If the sequence reaches `0xFF` with more frames to create, it shall rollover to `0x01` **not** `0x00` as it is reserved. <br><br>`0x00` is only used for the last consecutive frame in a multi-frame sequence and the last frame must have this value.|
|Data Size| `0x01` - `0xFFFFFFFF`|Payload size in bytes for only this frame |

## 4. Establishing Communication

### 4.1 Transport Layer
>Required: All Protocol Versions

A physical transport must be established between a head unit and an application before an SDL session can start. 

### 4.2 Version Negotiation
>Required: All Protocol Versions

#### 4.2.1 Overview
Once a physical transport is established, each application must negotiate the maximum supported protocol version with the head unit. To establish basic communication and register with the head unit, the application must start an RPC service (Service Type: 0x07), using a *Version 1 Protocol Header*.

There are two types of version negotiation. Protocol versions 1 through 4 use an old style of negotiation, where as versions 5 and newer use a faster and more intelligent negotiation scheme.

##### 4.2.1.1 Version 1-4 Negotiation
>Required for Protocol Versions 1 through 4

| Proxy| Direction | Core |
|------------|------|-------------|
|`StartService`<br> **Version:** v1 <br> **Payload:** no payload| ----------->|
|| <-----------|`StartServiceACK`<br> **Version:** Max supported by Core<br> **Payload:** raw bytes for hashID
|`SingleFrame` *(or other RPC supporting Frame Type)* <br> **Version:** Highest version supported by both Core and Proxy <br> **Payload:** Lots of bytes| ----------->| Sets negotiated version. 

##### 4.2.1.2 Version 5+ Negotiation
>Required for Protocol Versions 5 and newer

| Proxy| Direction | Core |
|------------|------|-------------|
|`StartService`<br> **Version:** v1 <br> **Payload:** Constructed payload [protocolVersion: 5.x.x]| ----------->| **v4 Core**: Ignores payload, sends protocol version 4 frame and uses previous negotiation scheme. <br> **v5+ Core:** Reads in payload data, uses this information to determine version. 
|| <-----------|`StartServiceACK`<br> **Version:** Highest version supported by both Core and Proxy<br> **Payload:** Constructed payload [protocolVersion: 5.x.x, hashId: 0x9873, mtu: 130687]
|`SingleFrame`<br> **Version:** Highest version supported by both Core and Proxy <br> **Payload:** Lots of bytes| ----------->|

#### 4.2.2 Starting Communication
The application sends a `StartService` frame to the module containing no payload.

##### Application -> Head Unit

#### 4.2.2.1 Versions 1 - 4

<table width="100%">
  <tr>
    <th>Version</th>
    <th>C</th>
    <th>Frame Type</th>
    <th>Service Type</th>
    <th>Frame Info</th>
    <th>Session Id</th>
    <th>Data Size</th>
  </tr>
  <tr>
    <td>1</td>
    <td>no</td>
    <td>Control</td>
    <td>RPC</td>
    <td>Start Service</td>
    <td>0</td>
    <td>0</td>
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

#### 4.2.2.2 Versions 5 and newer
>**Note:** Even though this is a Protocol Version 1 frame header it includes a payload. This is a very special exception.

Payload includes a constructed BSON object that has a single parameter of `protocolVersion` that describes the applications max supported Protocol Version.

<table width="100%">
  <tr>
    <th>Version</th>
    <th>C</th>
    <th>Frame Type</th>
    <th>Service Type</th>
    <th>Frame Info</th>
    <th>Session Id</th>
    <th>Data Size</th>
  </tr>
  <tr>
    <td>1</td>
    <td>no</td>
    <td>Control</td>
    <td>RPC</td>
    <td>Start Service</td>
    <td>0</td>
    <td>Size of Payload</td>
  </tr>
  <tr>
    <td>0b0001</td>
    <td>0b0</td>
    <td>0b000</td>
    <td>0x07</td>
    <td>0x01</td>
    <td>0x00</td>
    <td>0xNNNNNNNN</td>
  </tr>
  </table>
  
  <table width=100% >
  <tr>
  	<th colspan="8">Payload</th>
  </tr>
  <tr align="center">
  	<td colspan="8" > [protocolVersion: x.x.x]</td
  </tr>
</table>

#### 4.2.3 Success
If the head unit allows the RPC service to start, it will respond with a `StartServiceACK`. At this time the version will finish its negotiation process.

##### Head Unit -> Application

##### 4.2.3.1 Protocol Versions 1-4 

The `StartServiceACK` will contain the module's maximum supported protocol version. The packet structure will also match that of the supplied version; if the module's maximum supported version is 1, the packet will contain an 8 byte header (version 1), otherwise it will contain a 12 byte header (version 2). The application will then find the highest version supported by both the module and the application. This will be the determined version used for this session and will be used for all other packets sent from this point forward as well as all other services.

The payload of the `StartServiceACK` will contain a hash of the service which was started on the head unit if the payload size is greater than 0. This hash should be stored by an application and is needed in the end communication flow.


<table width="100%">
  <tr>
    <th>Version</th>
    <th>E</th>
    <th>Frame Type</th>
    <th>Service Type</th>
    <th>Frame Info</th>
    <th>Session Id</th>
    <th>Data Size</th>
    <th>Message ID</th>
  </tr>
  <tr>
    <td>Max Module Version (4)</td>
    <td>no</td>
    <td>Control</td>
    <td>RPC</td>
    <td>Start Service ACK</td>
    <td>Assigned Session</td>
    <td>4</td>
    <td>2</td>
  </tr>
  <tr>
    <td>0b0100</td>
    <td>0b0</td>
    <td>0b000</td>
    <td>0x07</td>
    <td>0x02</td>
    <td>0x01</td>
    <td>0x00000004</td>
    <td>0x0000000n</td>
  </tr>
</table>

##### 4.2.3.2 Protocol Versions 5 and newer 

The `StartServiceACK` will contain a negotiated version between what the application provided in the `StartService` frame and what the module's maximum supported protocol version. Thus, if it is determined that no such information was sent in the `StartService` frame, the module will assume the previous method of version negotiation and send version 4 to assume it's max version supplied to the application and wait for the incoming `RegisterAppInterface` RPC from the application to finally determine the negotiated version. Either way, the determined version will be used for this session including all other packets sent from this point forward as well as all other services.

The packet structure will also match that of the supplied version; if the module's maximum supported version is 1, the packet will contain an 8 byte header (version 1), otherwise it will contain a 12 byte header (version 2). If the protocol version was determined to be 5 or higher, the payload it contains will be constructed in nature as a BSON object. The application will then find the highest version supported by both the module and the application. This will be the determined version used for this session and will be used for all other packets sent from this point forward as well as all other services.

The payload of the `StartServiceACK` will contain the agreed upon full protocol version *"Major.Minor.Patch"*, a hash of the service which was started on the head unit, and the max transport unit for that session (0x07 RPC). The hash should be stored by an application and is needed in the end communication flow. The MTU should be used as the default MTU for all other services for that session unless otherwise provided in the corresponding `StartServiceACK` for that service. 

###### 4.2.3.2.1 Protocol Version Supplied in `StartService`

<table width="100%">
  <tr>
    <th>Version</th>
    <th>E</th>
    <th>Frame Type</th>
    <th>Service Type</th>
    <th>Frame Info</th>
    <th>Session Id</th>
    <th>Data Size</th>
    <th>Message ID</th>
  </tr>
  <tr>
    <td>Max major version supported by module and application </td>
    <td>no</td>
    <td>Control</td>
    <td>RPC</td>
    <td>Start Service ACK</td>
    <td>Assigned Session</td>
    <td>Size of payload</td>
    <td>2</td>
  </tr>
  <tr>
    <td>0bNNNN</td>
    <td>0b0</td>
    <td>0b000</td>
    <td>0x07</td>
    <td>0x02</td>
    <td>0x01</td>
    <td>0xNNNNNNNN</td>
    <td>0x0000000n</td>
  </tr>
</table>

<table width=100% >
	<tr>
  		<th colspan="8">Payload</th>
  	</tr>
  	<tr align="center">
  		<td colspan="8" > [protocolVersion: x.x.x, hashId: 0xNNNN, mtu: 130687]</td
  	</tr>
</table>

###### 4.2.3.2.2 Protocol Version Not Supplied  in `StartService`

<table width="100%">
  <tr>
    <th>Version</th>
    <th>E</th>
    <th>Frame Type</th>
    <th>Service Type</th>
    <th>Frame Info</th>
    <th>Session Id</th>
    <th>Data Size</th>
    <th>Message ID</th>
  </tr>
  <tr>
    <td>Max version application can possibly support (4)</td>
    <td>no</td>
    <td>Control</td>
    <td>RPC</td>
    <td>Start Service ACK</td>
    <td>Assigned Session</td>
    <td>4</td>
    <td>2</td>
  </tr>
  <tr>
    <td>0b0100</td>
    <td>0b0</td>
    <td>0b000</td>
    <td>0x07</td>
    <td>0x02</td>
    <td>0x01</td>
    <td>0x00000004</td>
    <td>0x0000000n</td>
  </tr>
</table>


##### 4.2.4 Failure
If a session has already been started, or can't be started, a `StartServiceNAK` will be sent in response to the `StartService` packet.

##### Head Unit -> Application

##### 4.2.4.1 Protocol Versions 1 through 4

<table width="100%">
  <tr>
    <th>Version</th>
    <th>E</th>
    <th>Frame Type</th>
    <th>Service Type</th>
    <th>Frame Info</th>
    <th>Session Id</th>
    <th>Data Size</th>
    <th>Message ID</th>
  </tr>
  <tr>
    <td>Max Module Version (4)</td>
    <td>no</td>
    <td>Control</td>
    <td>RPC</td>
    <td>Start Service NAK</td>
    <td>0</td>
    <td>0</td>
    <td>0</td>
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

##### 4.2.4.1 Protocol Versions 5 and Newer

<table width="100%">
  <tr>
    <th>Version</th>
    <th>E</th>
    <th>Frame Type</th>
    <th>Service Type</th>
    <th>Frame Info</th>
    <th>Session Id</th>
    <th>Data Size</th>
    <th>Message ID</th>
  </tr>
  <tr>
    <td>Max Module Version (4)</td>
    <td>no</td>
    <td>Control</td>
    <td>RPC</td>
    <td>Start Service NAK</td>
    <td>0</td>
    <td>Size of Payload</td>
    <td>0</td>
  </tr>
  <tr>
    <td>0b0100</td>
    <td>0b0</td>
    <td>0b000</td>
    <td>0x07</td>
    <td>0x03</td>
    <td>0x00</td>
    <td>0xNNNNNNNN</td>
    <td>0x00000000</td>
  </tr>
</table>

<table width=100% >
	<tr>
  		<th colspan="8">Payload</th>
  	</tr>
  	<tr align="center">
  		<td colspan="8" > [rejectedParams:[protocolVersion, x, x]]</td
  	</tr>
</table>

### 4.3 Registration
>Required: All Protocol Versions

Each application registers for continued communication with the head unit by sending a `RegisterAppInterface` Request RPC to the head unit via the RPC Service. Additional services can only be started after a successful `RegisterAppInterface` Response RPC has been sent from the head unit to the application.

### 4.4 Starting other services
While the RPC service is the default service that is started to establish a connection and a session, the application may wish to start other services. Similar to the process in Section 4, all services that are to be to started in a session require a `StartService` packet to be sent from the application. If the module supports and allows that service type to be started, it will respond with a `StartServiceACK` that has a payload of the hash ID for that service. If the module is unable to start that service or that application does not have access to that service, it will respond with a `StartServiceNAK`. 

### 4.5 Heartbeat
>**Deprecated: Protocol Versions 4 and higher** <br>
>Required: Protocol Version 3<br>
>Added: Protocol Version 3

After a successful start service exchange between the application and head unit both the application and head unit are required to be able to respond to heartbeat messages if the negotiated protocol version is 3. After sending a heartbeat, if the application or head unit does not respond within a timeout (custom per app/head unit), the sender will disconnect. The sender's timer for the heartbeat timeout should be reset every time any message is received. Heartbeats are sent using the Control Service Type (0x00)

#### 4.5.1 Heartbeat Request

>Note: The request can originate from either the Head Unit or the Application

##### Head Unit -> Application

<table width="100%">
  <tr>
    <th>Version</th>
    <th>E</th>
    <th>Frame Type</th>
    <th>Service Type</th>
    <th>Frame Info</th>
    <th>Session Id</th>
    <th>Data Size</th>
    <th>Message ID</th>
  </tr>
  <tr>
    <td>4</td>
    <td>no</td>
    <td>Control</td>
    <td>Control</td>
    <td>Heartbeat</td>
    <td>0</td>
    <td>0</td>
    <td>0</td>
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

#### 4.5.2 Heartbeat ACK

>Note: The response ACK will originate from the Head Unit or the Application based on the origin of the request

##### Application -> Head Unit

<table width="100%">
  <tr>
    <th>Version</th>
    <th>E</th>
    <th>Frame Type</th>
    <th>Service Type</th>
    <th>Frame Info</th>
    <th>Session Id</th>
    <th>Data Size</th>
    <th>Message ID</th>
  </tr>
  <tr>
    <td>4</td>
    <td>no</td>
    <td>Control</td>
    <td>Control</td>
    <td>Heartbeat ACK</td>
    <td>0</td>
    <td>0</td>
    <td>0</td>
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

#### 4.5.3 Heartbeat NAK
There is currently no heartbeat NAK.

### 4.6 Secondary Transport

>Added: Protocol version 5.1.0

After the RPC service has been established on an initial transport, it is possible to utilize a different transport beyond the initial transport for certain services.  This additional transport is called the "Secondary Transport". The initial transport used to start the RPC service is called the "Primary Transport".

#### 4.6.1 Secondary Transport Registration

The RPC `StartServiceACK` will include information on potential Secondary Transports in the parameter `secondaryTransports` if any are supported. Once received, it is possible to register the session on the Secondary Transport if connected; if the transport is not connected it will have to either wait until an update is received through the `TransportEventUpdated` frame or the physical connection is made.

Once the connection for Secondary Transport is established, if the application wishes to utilize that transport as a SecondaryTransport, the application is required to send a `RegisterSecondaryTransport` frame on that transport. The head unit will respond with either a`RegisterSecondaryTransportACK` or `RegisterSecondaryTransportNAK` frame. If the registration was successful and the application receives a `RegisterSecondaryTransportACK`, it may then utilize the Secondary Transport to start services.

##### 4.6.1.1 RegisterSecondaryTransport

##### Application -> Head Unit

<table width="100%">
  <tr>
    <th>Version</th>
    <th>E</th>
    <th>Frame Type</th>
    <th>Service Type</th>
    <th>Frame Info</th>
    <th>Session Id</th>
    <th>Data Size</th>
    <th>Message ID</th>
  </tr>
  <tr>
    <td>Max major version supported by module and application</td>
    <td>no</td>
    <td>Control</td>
    <td>Control</td>
    <td>Register Secondary Transport</td>
    <td>Session Id assigned on Primary Transport</td>
    <td>0</td>
    <td>1</td>
  </tr>
  <tr>
    <td>0bNNNN</td>
    <td>0b0</td>
    <td>0b000</td>
    <td>0x00</td>
    <td>0x07</td>
    <td>0xNN</td>
    <td>0x00000000</td>
    <td>0x00000001</td>
  </tr>
</table>

##### 4.6.1.2 RegisterSecondaryTransportACK

##### Head Unit -> Application

<table width="100%">
  <tr>
    <th>Version</th>
    <th>E</th>
    <th>Frame Type</th>
    <th>Service Type</th>
    <th>Frame Info</th>
    <th>Session Id</th>
    <th>Data Size</th>
    <th>Message ID</th>
  </tr>
  <tr>
    <td>Max major version supported by module and application</td>
    <td>no</td>
    <td>Control</td>
    <td>Control</td>
    <td>Register Secondary Transport ACK</td>
    <td>Session Id assigned on Primary Transport</td>
    <td>0</td>
    <td>2</td>
  </tr>
  <tr>
    <td>0bNNNN</td>
    <td>0b0</td>
    <td>0b000</td>
    <td>0x00</td>
    <td>0x08</td>
    <td>0xNN</td>
    <td>0x00000000</td>
    <td>0x00000002</td>
  </tr>
</table>

##### 4.6.1.3 RegisterSecondaryTransportNAK

##### Head Unit -> Application

<table width="100%">
  <tr>
    <th>Version</th>
    <th>E</th>
    <th>Frame Type</th>
    <th>Service Type</th>
    <th>Frame Info</th>
    <th>Session Id</th>
    <th>Data Size</th>
    <th>Message ID</th>
  </tr>
  <tr>
    <td>Max major version supported by module and application if known, otherwise 5</td>
    <td>no</td>
    <td>Control</td>
    <td>Control</td>
    <td>Register Secondary Transport NAK</td>
    <td>Session Id assigned on Primary Transport</td>
    <td>0</td>
    <td>2</td>
  </tr>
  <tr>
    <td>0bNNNN</td>
    <td>0b0</td>
    <td>0b000</td>
    <td>0x00</td>
    <td>0x09</td>
    <td>0xNN</td>
    <td>0x00000000</td>
    <td>0x00000002</td>
  </tr>
</table>


#### 4.6.2 Transport Event Update

Some Secondary Transports might require additional details on how they can be established. For example, in order to establish a TCP connection between the application and head unit the IP address and port number are required. The `TransportEventUpdate` control frame is used for this purpose. 

The head unit will send out a `TransportEventUpdate` frame whenever its transport configuration is changed, for example when its IP address is updated or Wi-Fi network goes down. The head unit will also send out a `TransportEventUpdate` frame right after the `StartServiceACK` frame that establishes the RPC service so the application can initiate a connection immediately.

The `TransportEventUpdate` frame must always be sent through the Primary Transport. The head unit should not send the frame to applications that don't support protocol versions 5.1.0 or newer.

##### 4.6.2.1 TransportEventUpdate

##### Head Unit -> Application

<table width="100%">
  <tr>
    <th>Version</th>
    <th>E</th>
    <th>Frame Type</th>
    <th>Service Type</th>
    <th>Frame Info</th>
    <th>Session Id</th>
    <th>Data Size</th>
    <th>Message ID</th>
  </tr>
  <tr>
    <td>Max major version supported by module and application</td>
    <td>no</td>
    <td>Control</td>
    <td>Control</td>
    <td>Transport Event Update</td>
    <td>Session Id assigned on Primary Transport</td>
    <td>Size of payload</td>
    <td>Variable</td>
  </tr>
  <tr>
    <td>0bNNNN</td>
    <td>0b0</td>
    <td>0b000</td>
    <td>0x00</td>
    <td>0xFD</td>
    <td>0xNN</td>
    <td>0xNNNNNNNN</td>
    <td>0xNNNNNNNN</td>
  </tr>
</table>

<table width=100% >
	<tr>
  		<th colspan="8">Payload</th>
  	</tr>
  	<tr align="center">
  		<td colspan="8" > [tcpIpAddress:"x.x.x.x", tcpPort:NNNN]</td
  	</tr>
</table>

#### 4.6.3 Starting Services on Secondary Transports

A Secondary Transport is capable of carrying  the video and audio services. Other services, including RPC and Hybrid service, must always run on the Primary Transport. (Note: Control service is an inherently started service on a transport and does not need to be established, but frames will be handled on a frame by frame basis of Primary vs Secondary Transport support)

The RPC `StartServiceACK` might include the parameters `audioServiceTransports` and `videoServiceTransports` describing which service is allowed to run on which transport priority type (Primary, Secondary or both). An application honors this information and starts the service(s) only on an allowed transport. For example, if video service is allowed only on a Secondary Transport, the application will not start video streaming until Secondary Transport is established and registered.

The transport priority types included in these parameters are listed in preferred order, for example, `[2,1]` (Secondary , Primary). In this case the priority of the Secondary Transport is higher than that of Primary Transport, the application may stop and restart service(s) when the Secondary Transport is added or removed. However,  each service type must only be started and carried on a single transport at a time. 

When starting a service over a Secondary Transport the application, it must follow the previous sections to establish the transport connection and register its session over that transport. At that point it runs the normal sequence described in section 4.4. When starting a service over a Secondary Transport, the session ID that was provided during the establishment of the RPC service should be used.

#### 4.6.4 Terminating Secondary Transport

There is no procedure to terminate a Secondary Transport. However, if the Primary Transport is disconnected or the RPC service is stopped, any Secondary Transport for that session should be unregistered and if no other sessions are registered over that Secondary Transport it should be disconnected.

## 5. Services
Every active session has the ability to start any of the services defined in this protocol spec as long as they have permission on the module in which they are connected. Every session can only have one of each type of service open at a time. 
 
Messages sent have a priority based on their Service Type. Lower values for service type have higher delivery priority. A message's payload's format is based on the different service types defined below.

### 5.1 Control Service
>Required: All Protocol Versions

The control service is the lowest level service available. While Control Frame packets are used frequently, the control service itself is rarely used.   

### 5.2 RPC Service
>Required: All Protocol Versions

The RPC service is used to send requests, responses, and notifications between an application and a head unit. Valid messages are defined in the [RPC Specification](https://github.com/smartdevicelink/sdl_core/blob/master/src/components/interfaces/MOBILE_API.xml).

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

#### 5.2.1 Binary Header
>Required: Protocol Version 2 and greater

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

##### 5.2.1.1 Binary Header Fields

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
    <td>32 bits (signed)</td>
    <td>The Correlation ID is used to map a request to its response. Requests sent in the same session with the same Correlation ID as a pending request will be rejected with an `INVALID_ID` response. Requests that use a Correlation ID less than 0 will be rejected with an `INVALID_ID` response. In Protocol Version 1, when the Binary Header did not exist, the Correlation ID was included as part of the JSON and has a max value of 65536.</td>
  </tr>
  <tr>
    <td>JSON Size</td>
    <td>32 bits</td>
    <td>The size of the JSON Data following the Binary Header in the RPC Payload</td>
  </tr>
</table>

### 5.3 Hybrid (Bulk Data) Service
>Required: Protocol Version 2 and greater

The Hybrid Service does not need to be explicitly started; all applications that have successfully started the RPC Service have access to the Hybrid Service.

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

### 5.4 Audio Service (PCM)
>Available: Protocol Version 3 and greater

The application can start the audio service to send PCM audio data to the head unit. After the `StartService` packet is sent and the ACK received, the payload for the Audio Service is only PCM audio data.

### 5.5 Video Service (H.264)
>Available: Protocol Version 3 and greater

The application can start the video service to send H.264 video data to the head unit. After the `StartService` packet is sent and the ACK received, the payload for the Video Service is only H.264 video data.


## 6. Ending Communication
The application may request it's session to be ended outside of a transport disconnect, module power cycle, etc. 
### 6.1 Completely Closing  a Session and Ending All Services
To close out a communication session with the head unit, an application sends an `EndService` packet with service type 7 (RPC) to the module. The `EndService` packet payload should include the correct hash ID supplied with the `StartServiceACK`. 

### 6.2 Closing Specific Services
If the application doesn't want to completely stop its session, but only wishes to close a specific session it can do so using an `EndService` packet that's service type matches the service that the application is trying to close. The `EndService` packet should include the hash ID in its payload that was contained in the `StartServiceACK` for  that specific service.
