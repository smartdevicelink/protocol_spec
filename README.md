# SmartDeviceLink Protocol

## 2. Frame Headers
### 2.1 Version 1 Frame Header
<table width="100%">
  <tr>
    <tr>
      <th width="25%" colspan="3">Byte 1</th>
      <th width="25%">Byte 2</th>
      <th width="25%">Byte 3</th>
      <th width="25%">Byte 4</th>
    </tr>
  </tr>
    <tr>
      <td>Version</td>
      <td>C</td>
      <td>Frame Type</td>
      <td>Service Type</td>
      <td>Control Frame Info</td>
      <td>Session ID</td>
    </tr>
    <tr>
    <tr>
        <tr>
          <th colspan=3>Byte 5</th>
          <th >Byte 6</th>
          <th >Byte 7</th>
          <th >Byte 8</th>
        <tr>
    </tr>
      <tbody>
        <tr>
          <td colspan="6" align="center">Data Size</td>
        </tr>
      </tbody>
    </tr>
  </tbody>
</table>

### 2.2 Version 2 Frame Header

<table width="100%">
  <tr>
    <tr>
      <th width="25%" colspan="3">Byte 1</th>
      <th width="25%">Byte 2</th>
      <th width="25%">Byte 3</th>
      <th width="25%">Byte 4</th>
    </tr>
  </tr>
    <tr>
      <td>Version</td>
      <td>E</td>
      <td>Frame Type</td>
      <td>Service Type</td>
      <td>Control Frame Info</td>
      <td>Session ID</td>
    </tr>
    <tr>
    <tr>
        <tr>
          <th colspan=3>Byte 5</th>
          <th >Byte 6</th>
          <th >Byte 7</th>
          <th >Byte 8</th>
        <tr>
    </tr>
      <tbody>
        <tr>
          <td colspan="6" align="center">Data Size</td>
        </tr>
      </tbody>
    </tr>
    <tr>
        <tr>
          <th colspan=3>Byte 9</th>
          <th >Byte 10</th>
          <th >Byte 11</th>
          <th >Byte 12</th>
        </tr>
          <tbody>
            <tr>
              <td colspan="6" align="center">Message ID</td>
            </tr>
          </tbody>
    </tr>
  </tbody>
</table>

## 3. Establishing Communication

### 3.1 Transport Layer

### 3.2 Version Negotiation

#### 3.2.1 Success

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
