## 1. SmartDeviceLink Protocol
**Note: this is a work in progress. I will remove this note when I believe this document is accurate**

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
      0x0 - 0xFFFFFFFF reserved. In a gen1.1 head unit a control frame with frame info StartServiceAck(0x02) sends 0x04 as the data size, and the payload contains the hash of the service which was ACK'd.<br> // TODO: need confirmation on other possible behaviors<br>
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

## 3. Establishing Communication

### 3.1 Transport Layer
A physical transport must be established between a head unit and an application.

### 3.2 Version Negotiation
Once a transport is established, an application must negotiate the maximum supported protocol version with the head unit. To establish basic communication and register with the head unit, the application must start an RPC service (Service Type: 0x07), using a *Version 1 Protocol Header*.

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
  <td>0x1</td>
  <td>0b0</td>
  <td>0b000</td>
  <td>0x07</td>
  <td>0x01</td>
  <td>0x00</td>
  <td>0x00000000</td>
</tr>
</table>

#### 3.2.1 Success

If the head unit can start the RPC service it will respond with a Start Service ACK containing its maximum supported protocol version. If its maximum supported version is 1, the packet will contain an 8 byte header, otherwise it will contain a 12 byte header. From this point forward, the application is expected to use the minimum of the head unit's maximum supported version, and its maximum supported version.

#### 3.2.2 Failure

## 4. Services

### 4.1 Starting a Service

#### 4.1.1 Success

#### 4.1.2 Failure

### 4.2 Ending a Service

### 4.3 RPC Service

### 4.4 Hybrid Service

### 4.5 Control Service

### 4.6 Audio Service

### 4.7 H.264 Service

### 4.8 Reserved Services

## 5 Ending Communication
