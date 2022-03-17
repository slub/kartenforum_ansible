# Service basemap

## main.yml

This ansible playbook creates a tileserver serving basemap tiles. Basically it installs a standalone [tileserver-gl](https://github.com/maptiler/tileserver-gl), together with downloading the data and configure the map services / styles and serve the tiles via and apache2 http server as reverse proxy:

> ansible-playbook main.yml --inventory single-tile-1.kartenforum.slub-dresden.de, --key-file "~/key_file" --user user_name


If the service has been installed successfully, the URL `https://single-tile-1.kartenforum.slub-dresden.de` different maps (vector/raster) should be offered.

### Variables

<table>
  <tbody>
    <tr>
      <th align="left">Name</th>
      <th align="center">Default value</th>
      <th align="center">Declaration</th>
    </tr>
    <tr>
      <td align="left">apache2.admin_mail</td>
      <td align="left">info@pikobytes.de</td>
      <td align="left">Mail of the admin of the Apache HTTP server.</td>
    </tr>
    <tr>
      <td align="left">apache2.cert.cert</td>
      <td align="left">/etc/certs/kartenforum.slub-dresden.de/cert.pem</td>
      <td align="left">Path to the certificate file on the remote machine.</td>
    </tr>
    <tr>
      <td align="left">apache2.cert.chain</td>
      <td align="left">/etc/certs/kartenforum.slub-dresden.de/chain.pem</td>
      <td align="left">Path to the certificate chain file on the remote machine.</td>
    </tr>
    <tr>
      <td align="left">apache2.cert.key</td>
      <td align="left">/etc/certs/kartenforum.slub-dresden.de/cert-key.key</td>
      <td align="left">Path to the key file for the certificate on the remote machine.</td>
    </tr>    
    <tr>
        <td align="left">apache2.vhosts</td>
        <td align="left">
            <ul>
                <li>tile-1.kartenforum.slub-dresden.de</li>
                <li>tile-2.kartenforum.slub-dresden.de</li>
            </ul>
        </td>
        <td align="left">
            Domain names under which the service should react on http and https.
        </td>    
    </tr>
    <tr>
        <td align="left">tileserver_gl.root_dir</td>
        <td align="left">/opt/tileserver</td>
        <td align="left">Root directory of the tileserver.</td>    
    </tr>
    <tr>
        <td align="left">tileserver_gl.mbtile</td>
        <td align="left">planet_20211030.mbtiles</td>
        <td align="left">Name of the mbtile file.</td>    
    </tr>   
    <tr>
        <td align="left">tileserver_gl.mbtile_download_link.url</td>
        <td align="left">https://u279620-sub1.your-storagebox.de/planet_20211030.mbtiles</td>
        <td align="left">Download link of the mbtile.</td>    
    </tr>   
    <tr>
        <td align="left">tileserver_gl.mbtile_download_link.auth_user</td>
        <td align="left">u279620-sub1</td>
        <td align="left">Name of the auth user, according to the download link.</td>    
    </tr>
    <tr>
        <td align="left">tileserver_gl.mbtile_download_link.auth_user</td>
        <td align="left">IVC3R0QpcYTfKmwc</td>
        <td align="left">Name of the auth password, according to the download link.</td>    
    </tr>    
    <tr>
        <td align="left">tileserver_gl.bounds</td>
        <td align="left">[-180.0,-85.06,180.0,85.06]</td>
        <td align="left">Bounds in geographic lat / lon coordinates of the mbtiles.</td>    
    </tr>     
    <tr>
        <td align="left">tileserver_gl.tile_domains</td>
        <td align="left">
            <ul>
                <li>tile-1.kartenforum.slub-dresden.de</li>
                <li>tile-2.kartenforum.slub-dresden.de</li>
            </ul>
        </td>
        <td align="left">Domains under which the tile server should be public accessible. <b>Important:</b> This configuration should match with the load balancer setting. If not, it will lead to problems while serving vector tiles.</td>    
    </tr>         
  </tbody>
</table>

For the description of the configuration of the `certs` variables have a look at [_roles/README.md](../_roles/README.md)

### Backup

This system can be fully restored via the ansible script. Nevertheless it is recommended to backup the mbtile (`planet_20211030.mbtiles`), in case the link does not work in the future.


### GDPR

The service collects ip adresses through the apache access log file. This information is only used for debugging reasons and can be deleted via clearing the Apache log files.

### License

##### Fonts 

For information about the license of the contained Fonts have a look at [openmaptiles/fonts](https://github.com/openmaptiles/fonts).

Referenced this source, all fonts are either licensed under OFL or Apache. 

##### Map styles

* [MapTiler Basic](https://github.com/openmaptiles/maptiler-basic-gl-style/blob/master/LICENSE.md): CC-BY 4.0 (Design) and BSD (Code)
* [OSM Bright](https://github.com/openmaptiles/osm-bright-gl-style/blob/master/LICENSE.md): CC-BY 4.0 (Design) and BSD (Code)
* [Positron](https://github.com/openmaptiles/positron-gl-style/blob/master/LICENSE.md): CC-BY 4.0 (Design) and BSD (Code)