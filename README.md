# Akapack
Akapack is a highly customizable VPN and proxy server/client toolkit designed to address the challenges of establishing secure, reliable connections in environments with restrictive internet access controls, such as corporate networks or countries with internet censorship. Akapack allows users to fully customize their VPN protocols, making it possible to bypass protocol-specific filtering and detection systems with ease.

## Features
- Modular Design: Leverage a variety of modules such as WebSockets, WireGuard, IPv4, SOCKS, and VMess for tailored connectivity solutions.
- Flexible Configuration: Define complex encapsulation and decapsulation rules to navigate through restrictive networks.
- Easy Deployment: Quickly set up VPN or proxy servers with minimal configuration.


## Example
Below is a sample configuration in config.yml that demonstrates how to set up a WebSockets module encapsulating WireGuard traffic.
``` yaml
web:
  module: websocket
  rule:
    encap:
      input: 
        - type: module
          name: wg
          action: encap
      output: 
        - type: endpoint
          protocal: tcp
          local: 0.0.0.0:<random>
          remote: test.com:80
    decap:
      input: 
        - type: endpoint
          protocal: tcp
          local: 0.0.0.0:<random>
          remote: test.com:80
      output: 
        - type: module
          name: wg
          action: decap
wg:
  module: wireguard
  configfile: /etc/wireguard/vpn01.conf
  rule:
    encap:
      input: 
        - type: tun
          name: vpn01
      output:
        - type: module
          name: web
          action: encap
    decap:
      input: 
        - type: module
          name: web
          action: decap
      output:
        - type: tun
          name: vpn01
```
