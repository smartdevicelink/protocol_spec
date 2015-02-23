# protocol_spec

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
