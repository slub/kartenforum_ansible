# Service explore

## main.yml

This ansible playbook installs an [elasticsearch](https://www.elastic.co/guide/index.html) on the remote system and published it behind an apache2 http server as a reverse proxy. The [elasticsearch](https://www.elastic.co/guide/index.html) is configured with a basic auth setup, which allows reading actions from `anonymous` users and writing actions only from the `elastic` user.

The playbook can be run via the following command:

> ansible-playbook service_explore/main.yml --inventory search.test.kartenforum.slub-dresden.de, --key-file "~/keyfile" --user user_name

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
        <td align="left">es.es_home</td>
        <td align="left">/usr/share/elasticsearch</td>
        <td align="left">Home directory of the elasticsearch installation</td>    
    </tr>
    <tr>
        <td align="left">es.es_api_basic_auth_username</td>
        <td align="left">elastic (<b>do not change</b>)</td>
        <td align="left">Username of the elastic writing user. Currently the ansible playbook is not tested with a user which is not elastic.</td>    
    </tr>
    <tr>
        <td align="left">es.es_api_basic_auth_password</td>
        <td align="left">set_your_own_password</td>
        <td align="left">Password of the elastic writing user.</td>    
    </tr>
    <tr>
        <td align="left">es.memory</td>
        <td align="left">1000M</td>
        <td align="left">RAM in MB which should be given to the elasticsearch node.</td>    
    </tr>
    <tr>
        <td align="left">es.always_reset</td>
        <td align="left">True</td>
        <td align="left">If set to `True`, the hole elasticsearch incl. basic auth settings is resetted, if the playbook is run.</td>    
    </tr>                
  </tbody>
</table>

For the description of the configuration of the `certs` variables have a look at [_roles/README.md](../_roles/README.md)

### Debug

Log-Files for the elasticsearch service can be find in the directory `/var/log/elasticsearch`.

For checking the proper working of the services use this test requests:

``` 
curl -X GET "https://search.kartenforum.slub-dresden.de/vk20/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": {
    "term": {
      "has_georeference": true 
    }
  }
}
'
```

### Backup

This system can be fully restored via ansible script.

### GDPR

The service collects ip addresses through the apache access log file. This information is only used for debugging reasons and can be deleted via clearing the Apache log files.

### Troubleshooting

* Normally the script does not reset a existing index. If the index is delete, it can be restored by the georeference-daemon. Therefore have a look at [service_geo/readme.md](../service_geo/README.md)