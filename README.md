openvpn
=======

Installs and configures OpenVPN

Requirements
------------

This role requires Ansible 1.4 or higher.

**NOTE:** This role currently only supports static key VPN tunnel.

1. Enable one of **openvpn_client** or **openvpn_server**
2. Generate a secret key file and set **openvpn_secret** to the contents of the file
   *openvpn --genkey --secret static.key*
3. If this is a client, you must set **openvpn_remote_host** to the name or IP of the remote OpenVPN server

Role Variables
--------------

| Name                    | Default                                                       | Description                                                                                            |
|-------------------------|---------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| openvpn_version         | 2.3.8                                                         | Version of OpenVPN to install                                                                          |
| openvpn_client          | false                                                         | Configure OpenVPN client                                                                               |
| openvpn_server          | false                                                         | Configure OpenVPN server                                                                               |
| openvpn_client_endpoint | 10.8.0.2                                                      | Client side endpoint address                                                                           |
| openvpn_server_endpoint | 10.8.0.1                                                      | Server side endpoint address                                                                           |
| openvpn_client_ifconfig | "{{ openvpn_client_endpoint }} {{ openvpn_server_endpoint }}" | Client ifconfig setting                                                                                |
| openvpn_server_ifconfig | "{{ openvpn_server_endpoint }} {{ openvpn_client_endpoint }}" | Server ifconfig setting                                                                                |
| openvpn_dev             | tun                                                           | VPN device                                                                                             |
| openvpn_comp_lzo        | true                                                          | Enable or disable compression                                                                          |
| openvpn_daemon          | true                                                          | Run OpenVPN as a daemon                                                                                |
| openvpn_keepalive       | '10 60'                                                       | Set timeouts                                                                                           |
| openvpn_nobind          | true                                                          | Do not bind to local address and port                                                                  |
| openvpn_persist_key     | true                                                          | Don't re-read key files across SIGUSR1 or --ping-restart                                               |
| openvpn_persist_tun     | true                                                          | Don't close and reopen TUN/TAP device or run up/down scripts across SIGUSR1 or --ping-restart restarts |
| openvpn_ping_timer_rem  | true                                                          | Run the --ping-exit / --ping-restart timer only if we have a remote address                            |
| openvpn_proto           | udp                                                           | Protocol to use for communicating with remote host                                                     |
| openvpn_remote_host     | ''                                                            | Remote host name or IP address                                                                         |
| openvpn_remote_port     | 1194                                                          | Remote host port                                                                                       |
| openvpn_remote_proto    | "{{ openvpn_proto }}"                                         | Remote host protocol                                                                                   |
| openvpn_secret          | ''                                                            | Secret static key                                                                                      |

Dependencies
------------

None

Example Playbook
----------------

Install OpenVPN server
```
- hosts: all
  vars:
    openvpn_server: true
    openvpn_secret: 'secret'
  roles:
    - kbrebanov.openvpn
```

Install older version of OpenVPN server
```
- hosts: all
  vars:
    openvpn_version: 2.3.6
    openvpn_server: true
    openvpn_secret: 'secret'
  roles:
    - kbrebanov.openvpn
```

Install OpenVPN client
```
- hosts: all
  vars:
    openvpn_client: true
    openvpn_secret: ''
    openvpn_remote_host: 'remote.vpn.com'
  roles:
    - kbrebanov.openvpn
```

License
-------

BSD

Author Information
------------------

Kevin Brebanov
