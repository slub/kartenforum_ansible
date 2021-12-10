# Core setup

## main.yml

This ansible playbook creates a basic core setup. It can be run via the following command:

> ansible-playbook core_setup/main.yml --inventory 111.111.111.111, --user remote_user --ask-pass --ask-become-pass

### Variables

<table>
  <tbody>
    <tr>
      <th align="left">Name</th>
      <th align="center">Default value</th>
      <th align="right">Declaration</th>
    </tr>
    <tr>
      <td align="left">custom_remote_user</td>
      <td align="left">user_name</td>
      <td align="left">
        The <code>custom_remote_user</code> is the default user, which should be used for accessing (ssh) the 
        server and installing dependencies. If changed in the Playbook it should also be changed in the <code> ansible.cfg > remote_user</code>
      </td>
    </tr>
  </tbody>
</table>
