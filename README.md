# ansible-vpn-server
ORTSOC Infra Role: VPN Server

## Variables

* `vpn_server_private_key`: base64 key, best generated with "wg genkey".
* `vpn_server_address`: The WireGuard internal address that peers/clients will use for communication after establishing initial connection. This cannot be the actual address on the network, this is an address that we choose.
* `vpn_server_port`: The port that clients will connect over to establish an initial connection.
* `peers`:
    * An object list, one for each client that will be connecting over wireguard, each with the following keys:
        *`public_key`: a base64 public key for the client/peer's WireGuard instance
        *`addr`: the WireGuard address for the client, not their actual address.