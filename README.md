# Scripts for deploying the Virtual Map Forum 2.0 

This repository contains the configuration files and ansible scripts for deploying the different service of the Virtual Map Forum 2.0. 

<table>
  <tbody>
    <tr>
      <th align="left">Placebook</th>
      <th align="center">Purpose</th>
      <th align="right">Requirements</th>
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
        <td align="left">service_basemap/standalone_basemap.yml</td>
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