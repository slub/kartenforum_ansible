# Service maps

## main.yml

This ansible playbook installs an apache2 for delivering files according to the tms, zoomify and terrain tiles protocol as well as simple thumbnail images. 

The playbook can be run via the following command:

> ansible-playbook service_tiles/main.yml --inventory tms-1.test.kartenforum.slub-dresden.de, --key-file "./keyfile" --user user_name

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
        <td align="left">hosts</td>
        <td align="left">
            <ul>
                <li><b>name</b>: Name of the host. (e.g. tms, wcs, wms, wms-transform, ...)</li>
                <li><b>domain</b>: Domain of the host (e.g. tms-1.kartenforum.slub-dresden.de, ...)</li>
                <li><b>dir</b>: Data directory to which the server reference. Should be an absolute path.</li>
                <li><b>vhost_template</b>: Base name of the vhost template file within the template file directory of the role apache-http-file-serving</li>
            </ul>
        </td>
        <td align="left">Basically it serves an array of `hosts`, for which apache virtualhosts should be configured and which are describing a mapserver or a simple file server.</td>    
    </tr>      
  </tbody>
</table>

For the description of the configuration of the `certs` variables have a look at [_roles/README.md](../_roles/README.md)

### Backup

This system can be fully restored via ansible script.

### GDPR

The service collects ip addresses through the apache access log file. This information is only used for debugging reasons and can be deleted via clearing the Apache log files.