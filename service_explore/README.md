# Service explore

## main.yml

This ansible playbook creates a elasticsearch single node instance with an Apache2 HTTP server as Reverse Proxy. It can be run via the following command:

> ansible-playbook standalone_basemap.yml --key-file "~/path/to/key_file" -i 111.111.111.111,

If the service has been installed successfully, the URL `http://111.111.111.111` should return an json error object for anonymous users.

The elasticsearch instance is created with an  anonymous user, which is allowed to read all indices and documents as well as an admin "elastic" user. 

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
            IP address or domain of the tile service, for which virtual host configs should be created. A server can
            listen to multiple vhosts.
        </td>    
    </tr>
    <tr>
        <td align="left">es.es_home</td>
        <td align="left">/usr/share/elasticsearch</td>
        <td align="left">Root directory of the elasticsearch. Should be changed carefully.</td>    
    </tr>
    <tr>
        <td align="left">es.es_api_basic_auth_password</td>
        <td align="left">test1234</td>
        <td align="left">Password of the elastic search user. Should be changed.</td>    
    </tr>
    <tr>
        <td align="left">es.es_api_basic_auth_username</td>
        <td align="left">elastic</td>
        <td align="left">Admin default user. Should be changed carefully.</td>    
    </tr>
    <tr>
        <td align="left">es.memory</td>
        <td align="left">100m</td>
        <td align="left">Speicherplatz der durch ES allokiert werden darf.</td>    
    </tr>
  </tbody>
</table>

### GDPR

The service collects ip adresses through the Apache access log file. This information is only used for debugging reasons and can be deleted via clearing the Apache log files.