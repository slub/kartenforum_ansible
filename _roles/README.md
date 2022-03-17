# _roles

The directory contains different generic roles. For proper working of this role it is expected that a `certificate`, `chain` and a `key` file are stored within a directory consisting of the dns name, for which the certificates work, e.g.:

```
> apache-certs
  > files 
    > kartenforum.slub-dresden.de
      > cert.pem
      > cert-key.key
      > chain.pem
```

## apache-certs

Installs certificates on the remote machine for supporting https.

### Variables

<table>
  <tbody>
    <tr>
      <th align="left">Name</th>
      <th align="center">Default value</th>
      <th align="center">Declaration</th>
    </tr>
    <tr>
      <td align="left">certs.root</td>
      <td align="left">/etc/certs</td>
      <td align="left">Directory where the certificate files should be stored on the remote machine.</td>
    </tr>
    <tr>
      <td align="left">certs.certificates</td>
      <td align="left">
        <ul>
            <li><b>name</b>: Domain name for which the certificate files are registered (e.g. kartenforum.slub-dresden.de).</li>
            <li><b>cert</b>: Name of the certificate file (e.g. cert.pem).</li>
            <li><b>chain</b>: Name of the certificate chain file (e.g. chain.pem).</li>
            <li><b>key</b>: Key file for the certificate (e.g. cert-key.key).</li>
        </ul>
      </td>
      <td align="left">Array structure which allows the installation of multiple certificates</td>
    </tr>    
  </tbody>
</table>

## apache-clean-install

The role install an apache2 http server and remove any default vhosts as well as the index.html from `/var/www/html`

## apache-clean-vhosts

This role removes any vhost configurations from a apache2 http server installation configured via `apache2.vhosts`.

## iptables

This role sets up a basic iptables configuration allowing access via ssh, http and https.