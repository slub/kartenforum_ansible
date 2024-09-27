# Service place 

## main.yml

This ansible playbooks installs the [mediagis/nominatim-docker](https://github.com/mediagis/nominatim-docker) with an apache http2 server as reverse proxy. 

The services offers the full nomiatim API functionalities. The database volume was produced in advanced and is expected to be already available on the system. Make sure to have the correct file permissions set, because this 
can lead to problems, starting the service.

The playbook can be run via the following command:

> ansible-playbook service_place/main.yml --inventory place-search.kartenforum.slub-dresden.de, --key-file "./keyfile" --user user_name

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
                <li><b>name</b>: Name of the host.</li>
                <li><b>domain</b>: Domain of the host (e.g. place-search.kartenforum.slub-dresden.de, ...)</li>
            </ul>
        </td>
        <td align="left">Basically it serves an array of `hosts`, for which apache virtualhosts should be configured and which are describing a mapserver or a simple file server.</td>    
    </tr>      
  </tbody>
</table>

For the description of the configuration of the `certs` variables have a look at [_roles/README.md](../_roles/README.md)

### Backup

The geocoding database, was processed for the hole planet on a different system and is placed in the directory `/opt/nominatim/data/pg`. This database can not be stored by the ansible script and should be backup separately. To save storage, the database was created with `FREEZE=true`, which prevents incremental updates on the database.

It is recommend to store backup the database `/opt/nominatim/data/pg` on update. 

### GDPR

The service collects ip addresses through the apache access log file. This information is only used for debugging reasons and can be deleted via clearing the Apache log files.

## About [mediagis/nominatim](https://hub.docker.com/r/mediagis/nominatim)

The [mediagis/nominatim](https://hub.docker.com/r/mediagis/nominatim) is a docker image for the nominatim geocoder based on [OpenStreetMap](https://www.openstreetmap.de/). 

It offers a couple of APIs:

[Nominatim](https://nominatim.org/) supports a couple of different usage scenarios:

#### Search

It searches different OSM (with location) objects by name or type. 

Example request:

```
curl https://place-search.kartenforum.slub-dresden.de/search?format=json&q=Be
```

Response:

```json
[
   {
      "place_id":2758437,
      "licence":"Data © OpenStreetMap contributors, ODbL 1.0. https://osm.org/copyright",
      "osm_type":"relation",
      "osm_id":1686344,
      "boundingbox":[
         "46.3265189",
         "47.3453097",
         "6.8614832",
         "8.4551574"
      ],
      "lat":"46.8382351",
      "lon":"7.6004502",
      "display_name":"Bern, Switzerland",
      "class":"boundary",
      "type":"administrative",
      "importance":0.714039589531124
   },
   {
      "place_id":2070963,
      "licence":"Data © OpenStreetMap contributors, ODbL 1.0. https://osm.org/copyright",
      "osm_type":"way",
      "osm_id":332946077,
      "boundingbox":[
         "46.672345",
         "46.6724168",
         "7.4637816",
         "7.4638486"
      ],
      "lat":"46.672345",
      "lon":"7.4637816",
      "display_name":"Suspension bridge 18 m Weissenburg Bad, Oberwil im Simmental, Frutigen-Niedersimmental administrative district, Oberland administrative region, Bern, 3765, Switzerland",
      "class":"highway",
      "type":"path",
      "importance":0.17499999999999996
   }
]
```

#### Reverse

It searches different OSM objects by their location. 

Example request:

```
curl https://place-search.kartenforum.slub-dresden.de/reverse?format=json&lat=46.838&lon=7.463
```

Response:

```json
{
   "place_id":1545032,
   "licence":"Data © OpenStreetMap contributors, ODbL 1.0. https://osm.org/copyright",
   "osm_type":"way",
   "osm_id":190292632,
   "lat":"46.83790105",
   "lon":"7.463208872020891",
   "display_name":"5, Bütschelstrasse, Oberbütschel, Rüeggisberg, Bern-Mittelland administrative district, Bernese Mittelland administrative region, Bern, 3088, Switzerland",
   "address":{
      "house_number":"5",
      "road":"Bütschelstrasse",
      "hamlet":"Oberbütschel",
      "village":"Rüeggisberg",
      "county":"Bern-Mittelland administrative district",
      "state_district":"Bernese Mittelland administrative region",
      "state":"Bern",
      "postcode":"3088",
      "country":"Switzerland",
      "country_code":"ch"
   },
   "boundingbox":[
      "46.8377601",
      "46.8380688",
      "7.4629664",
      "7.4633447"
   ]
}
```

#### Status

```
curl https://place-search.kartenforum.slub-dresden.de/status
```