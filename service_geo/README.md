# Service explore

## main.yml

This ansible playbook installs the [postgresql]() / [postgis]() database, imports a database dump, install the georeference service and daemon and installs an apache2 http server as a reverse proxy.

The playbook can be run via the following command:

> ansible-playbook service_geo/main.yml --inventory geo.test.kartenforum.slub-dresden.de, --key-file "~/keyfile" --user user_name

The services writes temporary and permanent georeference data, which it shares with the different services from [service_maps](../service_maps/README.md). It therefor needs write access to the network storage which is mounted by the [service_maps](../service_maps/README.md) machine.

In production the service is only requested through a TYPO3 proxy layer. This means the service can also be run within an private network. __Caution__: In the public the service should be only runned with `apache2.basic_auth=True`.

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
      <td align="left">apache2.basic_auth</td>
      <td align="left">True</td>
      <td align="left">Enables basic auth. If set to `False` and the system is accessible in the public, every user can write data to the georeference service, because the latter is currently missing an authentification layer.</td>
    </tr> 
    <tr>
      <td align="left">apache2.basic_auth_user</td>
      <td align="left">set_your_own_user</td>
      <td align="left">Basic auth user for the georeference service.</td>
    </tr>
    <tr>
      <td align="left">apache2.basic_auth_password</td>
      <td align="left">set_your_own_password</td>
      <td align="left">Basic auth password for the georeference service.</td>
    </tr>    
    <tr>
      <td align="left">db.name</td>
      <td align="left">vkdb</td>
      <td align="left">Name of the database of the service.</td>
    </tr>
    <tr>
      <td align="left">db.user</td>
      <td align="left">vk20</td>
      <td align="left">Owner of the database of the service. Main user of the georeference service.</td>
    </tr>
    <tr>
      <td align="left">db.password</td>
      <td align="left">set_your_own_password</td>
      <td align="left">Password of the user configured via `db.user`.</td>
    </tr>
    <tr>
      <td align="left">db.dump</td>
      <td align="left">vkdb-new.dump</td>
      <td align="left">Dump of the vkdb, which should be imported into the database. Make sure that the dump uses the same user as configured via `db.user`. The dump file should be placed in the directory `roles/import-database/files/`</td>
    </tr>      
        <tr>
      <td align="left">db.restore_database</td>
      <td align="left">False</td>
      <td align="left">If set to `True` it also creates and imports the database. Should be `True` on first import.</td>
    </tr>
    <tr>
      <td align="left">service.root_path</td>
      <td align="left">/opt/kartenforum_georeference</td>
      <td align="left">Path to the root directory of the georeferencer service and daemon code.</td>
    </tr>
    <tr>
      <td align="left">service.git_repo.url</td>
      <td align="left">https://github.com/slub/kartenforum_georeference</td>
      <td align="left">Url to the git repository. Make sure that the git repository is public or that your ssh agent supports `-o ForwardAgent=yes`.</td>
    </tr> 
    <tr>
      <td align="left">service.git_repo.branch</td>
      <td align="left">main</td>
      <td align="left">Branch of the git repository to checkout.</td>
    </tr>
    <tr>
      <td align="left">service.use_git_repo</td>
      <td align="left">True</td>
      <td align="left">If `True` it uses the git repository for code checkout. As an alternative for developing setups, set it to `False` and place the code as an `tar.gz` archive under the path `roles/geoerference/files/georeference.tar.gz`.</td>
    </tr>
    <tr>
      <td align="left">service.settings.global_permalink_resolver</td>
      <td align="left">http://digital.slub-dresden.de</td>
      <td align="left">Url of the global permalink resolver.</td>
    </tr>
    <tr>
      <td align="left">service.settings.template_links_tms</td>
      <ul>
        <li>https://tms-1.kartenforum.slub-dresden.de/%s</li>
        <li>https://tms-1.kartenforum.slub-dresden.de/%s</li>
      </ul>
      <td align="left">Template string which is used from the Python Code to generate the TMS-Links. `%s` is an expression, which can be used from Python to resolve template strings.</td>
    </tr>   
    <tr>
      <td align="left">service.settings.template_link_wms</td>
      <td align="left">https://wms.kartenforum.slub-dresden.de/map/%s</td>
      <td align="left">Template string which is used from the Python Code to generate the WMS-Links. `%s` is an expression, which can be used from Python to resolve template strings.</td>
    </tr>   
    <tr>
      <td align="left">service.settings.template_link_wms_transform</td>
      <td align="left">https://wms-transform.kartenforum.slub-dresden.de/map/%s</td>
      <td align="left">Template string which is used from the Python Code to generate the temporary WMS-Links. `%s` is an expression, which can be used from Python to resolve template strings.</td>
    </tr> 
    <tr>
      <td align="left">service.settings.template_link_wcs</td>
      <td align="left">https://wcs.kartenforum.slub-dresden.de/map/%s</td>
      <td align="left">Template string which is used from the Python Code to generate the WCS-Links. `%s` is an expression, which can be used from Python to resolve template strings.</td>
    </tr>   
   <tr>
      <td align="left">service.settings.template_link_thumbnail</td>
      <td align="left">https://thumbnail.test.kartenforum.slub-dresden.de/{}</td>
      <td align="left">Template string which is used from the Python Code to generate the Thumbnail-Links. `{}` is an expression, which can be used from Python to resolve template strings.</td>
    </tr>   
   <tr>
      <td align="left">service.settings.template_link_zoomify</td>
      <td align="left">https://zoomify.test.kartenforum.slub-dresden.de/{}/ImageProperties.xml</td>
      <td align="left">Template string which is used from the Python Code to generate the Zoomify-Links. `{}` is an expression, which can be used from Python to resolve template strings.</td>
    </tr>   
    <tr>
      <td align="left">search.domain</td>
      <td align="left">search.kartenforum.slub-dresden.de</td>
      <td align="left">Domain of the search endpoint.</td>
    </tr> 
    <tr>
      <td align="left">search.port</td>
      <td align="left">443</td>
      <td align="left">Port to acccess the search endpoint.</td>
    </tr>
    <tr>
      <td align="left">search.ssl</td>
      <td align="left">True</td>
      <td align="left">If `True` it uses https and if `False` http.</td>
    </tr>
    <tr>
      <td align="left">search.username</td>
      <td align="left">elastic</td>
      <td align="left">Username of a user, which is allowed to write to the search endpoint.</td>
    </tr>
    <tr>
      <td align="left">search.password</td>
      <td align="left">set_your_own_password</td>
      <td align="left">Password for the `search.username`.</td>
    </tr>
    <tr>
      <td align="left">storage.root_path</td>
      <td align="left">/srv/vk</td>
      <td align="left">Root directory of data directory.</td>
    </tr>
    <tr>
      <td align="left">storage.directories.image_root</td>
      <td align="left">/srv/vk/original</td>
      <td align="left">Directory in which the original images should be placed.</td>
    </tr>
    <tr>
      <td align="left">storage.directories.geo_root</td>
      <td align="left">/srv/vk/georef</td>
      <td align="left">Directory in which the georeferenced images should be placed.</td>
    </tr>
    <tr>
      <td align="left">storage.directories.tms_root</td>
      <td align="left">/srv/vk/tms</td>
      <td align="left">Directory in which the tms image directories should be placed.</td>
    </tr>
    <tr>
      <td align="left">storage.directories.tmp_root</td>
      <td align="left">/srv/vk/tmp</td>
      <td align="left">Directory in which temporary files should be placed.</td>
    </tr>
    <tr>
      <td align="left">storage.directories.mapfile_root</td>
      <td align="left">/srv/vk/map_services</td>
      <td align="left">Directory in which the mapfiles for configuration of the map services should be placed.</td>
    </tr>   
    <tr>
      <td align="left">storage.directories.mapfile_tmp_root</td>
      <td align="left">/srv/vk/map_services_tmp</td>
      <td align="left">Directory in which temporary mapfiles and geotiffs should be placed for deliverying georeference results within the georeference process.</td>
    </tr>          
    <tr>
      <td align="left">storage.directories.daemon_tmp</td>
      <td align="left">/srv/vk/daemon_tmp</td>
      <td align="left">Directory in which configuration files of the daemon should be placed.</td>
    </tr>         
<tr>
      <td align="left">storage.directories.thumbnail_root</td>
      <td align="left">/srv/vk/thumbnails</td>
      <td align="left">Directory in which thumbanils are placed.</td>
    </tr> 
<tr>
      <td align="left">storage.directories.zoomify_root</td>
      <td align="left">/srv/vk/zoomify</td>
      <td align="left">Directory in which zoomify tiles are placed.</td>
    </tr> 
<tr>
      <td align="left">storage.directories.upload_tmp</td>
      <td align="left">/srv/vk/upload_tmp</td>
      <td align="left">Directory in which the temporary upload files are stored.</td>
    </tr> 
    <tr>
      <td align="left">storage.check_permission</td>
      <td align="left">False</td>
      <td align="left">Checks if the permissions on the different directories are set correctly. <b>Caution:</b> this can lead to long running playbooks.</td>
    </tr>     
    <tr>
      <td align="left">backup.data_path</td>
      <td align="left">/srv/vk/backup/data</td>
      <td align="left">Directory where backup files should be placed.</td>
    </tr> 
    <tr>
      <td align="left">backup.script_path</td>
      <td align="left">/srv/vk/backup/scripts</td>
      <td align="left">Directory where backup scripts should be placed.</td>
    </tr>                                    
  </tbody>
</table>

For the description of the configuration of the `certs` variables have a look at [_roles/README.md](../_roles/README.md)

### Debug

Log-Files for the georeference service and daemon can be find in the directory `/var/log/georeference`.

For checking the proper working of the services use this test requests:

``` 
curl -XGET https://geo.kartenforum.slub-dresden.de/statistics -u vk20_user:vk20_user_1234
```

### Backup

The system backs up the database daily and stores it in the directory xy. Backups older than 14 days are deleted. 

Furthermore, it is recommended to back up the following directories regularly, as a failure would cause very long recovery times:

* /srv/vk/original
* /srv/vk/georef
* /srv/vk/tms

### GDPR

Within the database there are userids stored. If these are appropriately speaking, they can allow a conclusion to be drawn about individual users. They are also published on the portal as part of the ranking.

The service collects ip addresses through the apache access log file. This information is only used for debugging reasons and can be deleted via clearing the Apache log files.

### Troubleshooting

#### Restart the georeference-daemon

In case the mapfiles or the index is broken, they can be restored by using the georeference-daemon. The daemon can be restarted via the folling commands:

``` 
# Kills the daemon
top -c -p $(pgrep -d',' -f python_env)
kill -9

# Starts the daemon
/opt/kartenforum_georeference/python_env/bin/python /opt/kartenforum_georeference/georeference/daemon/runner.py
```

#### Idempotence of the ansible scripts

The script can be executed several times in a row. However, care should be taken that `db.restore_database==True` resets the database each time and thus existing information in the database can be lost.

#### Resetting the database

If the database is resetted, make also sure that the roles `georeference` is run, which starts the georeference daemon and service again.