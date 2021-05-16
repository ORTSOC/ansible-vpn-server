ansible-vpn-server
=========

Deploys a Wireguard instance, running as a VPN server to accept inbound traffic.

Requirements
------------

- Debian 10
- sudo
- ssh

Role Variables
--------------

- `vpn_server_private_key`: base64 key, best generated with "wg genkey".
- `vpn_server_address`: The WireGuard internal address that peers/clients will use for communication after establishing initial connection. This cannot be the actual address on the network, this is an address that we choose.
- `vpn_server_port`: The port that clients will connect over to establish an initial connection.
- `net_interface`: (OPTIONAL) Remove this if "the server is behind a router and receives traffic via NAT". If not, run "ip link" to get this.
- `peers`: An object list, one for each client that will be connecting over wireguard, each with the following keys:
   - `public_key`: a base64 public key for the client/peer's WireGuard instance
   - `addr`: the WireGuard address for the client, not their actual address.
- `filebeat_modules`: A list of filebeat modules to enable, but really should only be set to `wireguard`. See [ansible-filebeat](https://github.com/ORTSOC/ansible-filebeat) for more information on filebeat modules.

Example Playbook
----------------

```yml

# Choose an address. This cannot be the actual network-facing address of the machine.
# instead, this will be the new address for the machine that systems can reach when
# connected through WireGuard.
vpn_server_address: 10.11.11.1

# port that the server will be accessible from. Make sure this port-forwarded if accepting external connections.
vpn_server_port: 51820

# Optional. Remove this if "the server is behind a router and receives traffic via NAT"
# If not, run "ip link" and find the network interface name. Will be something like enp0, ens33, etc.
net_interface: ens192

# the private key to decrypt traffic from that is encrypted with the public key
vpn_private_key: aaaaaaaaaaaaaaaaaaaaaaaa=

# list of peers that the VPN server will accept connections from.
#     public_key - the base64 public key provided by the peer for their own wireguard instance.
#     addr - the Wireguard address for the wireguard network, not their regular network interface address.
peers:
  - public_key: uRHD+jcUXnNYHiEBcTZrj/fC5woEC68668b7p0FcITg=
    addr: 10.11.11.2
    
filebeat_modules:
  - wireguard
```

License
-------

GPL3

Author Information
------------------

ORTSOC, Oregon State University
