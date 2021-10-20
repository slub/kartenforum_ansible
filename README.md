# Scripts for deploying the Virtual Map Forum 2.0 

This repository contains the configuration files and ansible scripts for deploying the different service of the Virtual Map Forum 2.0. 

<table>
  <tbody>
    <tr>
      <th align="left">Placebook</th>
      <th align="center">Purpose</th>
      <th align="center">Requirements</th>
    </tr>
    <tr>
      <td align="left">core_setup/main.yml</td>
      <td align="left">Installs a base system with the user "vk2adm", blocks the "root" user for ssh and configures IP tables with open ports for SSH and HTTP/S</td>
      <td align="left">
        <ul>
            <li>Debian 11</li>
            <li>SSH-Login for User <code>root</code> via key file</li>
        </ul>
      </td>
    </tr>
    <tr>
        <td align="left">service_basemap/main.yml</td>
        <td align="left">Installs a standalone tile server with initial configuration and an Apache2 HTTP server as reverse proxy and image cache.</td>
        <td align="left">
            <ul>
                <li>Debian 11</li>
                <li>SSH-Login for User <code>vk2adm</code> via key file</li>
                <li>core_setup/main.yml was executed before</li>
                <li>at least 2 cpu cores</li>
                <li>at least 2 gb ram</li>
                <li>at least 40 gb disk space for default configuration</li>
            </ul>
        </td>    
    </tr>
    <tr>
        <td align="left"></td>
        <td align="left"></td>
        <td align="left"></td>    
    </tr>
  </tbody>
</table>

## Recommendation for service dimensioning

The Virtual Map Forum consists of a number of different services, which can be dimensioned and configured in different ways. 

![Architecture of the Virtual Map Forum](./architecture.png "Architecture of the Virtual Map Forum")


The following dimensioning is recommended as a standard setup.

<table>
  <tbody>
    <tr>
      <th align="left">Domain / Name</th>
      <th align="center">Purpose</th>
      <th align="center">Requirements</th>
      <th align="right">Redundancy</th>
    </tr>
    <tr>
      <td align="left">
        single-tile-{x}.slub-dresden.de
      </td>
      <td align="left">Tileserver delivering raster and vector tiles based on an OpenStreetMap dump. In case the server are set behind a load balancer the domains should be used by the load balancer.</td>
      <td align="left">
        <ul>
            <li>Debian 11</li>
            <li>2 cpu cores</li>
            <li>2 gb ram</li>
            <li>120 gb disk space</li>
        </ul>
      </td>
      <td align="left">
        At least 2 server. In case of high parallel load of raster tiles 3 to 4 would be better.
      </td>
    </tr>
    <tr>
        <td align="left">
            tile-{1|2|3}.slub-dresden.de}
        </td>
        <td align="left">
            Load balancer based on Apache2 HTTP server. Distributes tile requests across multiple servers.
        </td>
        <td align="left">
        <ul>
            <li>Debian 11</li>
            <li>1 cpu cores</li>
            <li>2 gb ram</li>
            <li>20 gb disk space</li>
        </ul>
        </td>
        <td align="left">
            Not necessary.
        </td>     
    </tr>
  </tbody>
</table>