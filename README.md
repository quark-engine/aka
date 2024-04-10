# akapack

This is a tool that can very easy to customize and build your own VPN or proxy protocal.


## Example

config:
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
