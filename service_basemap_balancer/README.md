# Service basemap_balancer

## main.yml

This ansible playbook creates a software (apache2) based load balancer for balancing load for an arbitrary number of map tile server.

Basically it uses the `reverse proxy` and `caching` functionalities of the apache2 http server. The services only caches image resources.

The playbook can be run via the following command:

> ansible-playbook main.yml --inventory tile-1.kartenforum.slub-dresden.de, --key-file "~/keyfile" --user user_name

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
        <td align="left">apache2.clear_cache_interval</td>
        <td align="left">15</td>
        <td align="left">Interval of cache clean cycles in minutes.</td>    
    </tr>
    <tr>
        <td align="left">apache2.disk_cache_size</td>
        <td align="left">12G</td>
        <td align="left">Size of the disk cache used by the Apache HTTP server to cache image files.</td>    
    </tr>
       <tr>
        <td align="left">apache2.proxy_urls</td>
        <td align="left">
            <ul>
                <li>https://single-tile-1.kartenforum.slub-dresden.de</li>
                <li>https://single-tile-2.kartenforum.slub-dresden.de</li>
            </ul>
        </td>
        <td align="left">URLs which the load balancer should be proxy.</td>    
    </tr>
  </tbody>
</table>

For the description of the configuration of the `certs` variables have a look at [_roles/README.md](../_roles/README.md)

### Backup

This system can be fully restored via ansible script.

### GDPR

The service collects ip addresses through the apache access log file. This information is only used for debugging reasons and can be deleted via clearing the Apache log files.