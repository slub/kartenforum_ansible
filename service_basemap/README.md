# Service basemap

## standalone_basemap.yml

This ansible playbook creates a tileserver serving basemap tiles. Basically it installs a standalone tile server with initial configuration and an Apache2 HTTP server as a reverse proxy and image cache. It can be run via the following command:

> ansible-playbook standalone_basemap.yml --key-file "~/path/to/key_file" -i 111.111.111.111,

If the service has been installed successfully, the URL `http://111.111.111.111` different maps (vector/raster) should be offered.

### Variables

<table>
  <tbody>
    <tr>
      <th align="left">Name</th>
      <th align="center">Default value</th>
      <th align="right">Declaration</th>
    </tr>
    <tr>
      <td align="left">apache2.admin_mail</td>
      <td align="left">info@pikobytes.de</td>
      <td align="left">Mail of the admin of the Apache HTTP server.</td>
    </tr>
    <tr>
        <td align="left">apache2.vhosts</td>
        <td align="left">111.111.111.111</td>
        <td align="left">
            IP address or domain of the tile service, for which virtual host configs should be created.
        </td>    
    </tr>
    <tr>
        <td align="left">tileserver_gl.root_dir</td>
        <td align="left">/opt/tileserver</td>
        <td align="left">Root directory of the tileserver, where configuration and mbtiles files should be placed.</td>    
    </tr>
    <tr>
        <td align="left">tileserver_gl.mbtile</td>
        <td align="left">zurich_switzerland.mbtiles</td>
        <td align="left">Name of the mbtile, which is used by the tileserver.</td>    
    </tr>
    <tr>
        <td align="left">tileserver_gl.mbtile_download_link</td>
        <td align="left">https://tile-1.pikobytes.de/zurich_switzerland.mbtiles</td>
        <td align="left">Download link for the mbtile to use.</td>    
    </tr>
    <tr>
        <td align="left">tileserver_gl.mbtile_bounds</td>
        <td align="left">[8.3613,47.2958,8.6744,47.4854]</td>
        <td align="left">Bounds of the mbtile used.</td>    
    </tr>
    <tr>
        <td align="left">tileserver_gl.tile_domains</td>
        <td align="left">111.111.111.111</td>
        <td align="left">Domain / IP address under which the tileserver is publised. Important setting for proper working of the tileservice behind a reverse proxy and load balancer.</td>    
    </tr>
    <tr>
        <td align="left">disk_cache_size</td>
        <td align="left">10G</td>
        <td align="left">Size of the disk cache used by the Apache HTTP server to cache image files.</td>    
    </tr>
    <tr>
        <td align="left">custom_remote_user</td>
        <td align="left">vk2adm</td>
        <td align="left">Remote user which should be used for running the tileserver</td>    
    </tr>
  </tbody>
</table>
