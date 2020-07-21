# LAN Gateway
> This configuration was in use at Lanarama and was able to handle 10Gbit WAN traffic without issues.

This repository contains scripts for building an Alpine-Image containing the Gateway configuration used at Lanarama.

## Gateway Services
[We](https://lanarama.com) are using `nftable` as firewall / forwarder. It firewalls IPv6 traffic (very basic, conntrack only) and acts as NAT gateway for IPv4 clients.
Nftables internal `jhash` maps a number of clients across multiple public IPv4 addresses.


## Connectivity
[FRRouting](https://frrouting.org) acts as BGP router exposing a default route (`0.0.0.0/0`, `::/0`) to the backbone multilayer switches. ECMP support is configured in order to establish an active/active, highly available networking stack.
It should (in theory) be possible to just use static routes. We just love tech and want to play around :blush:.

## Observability
### Monitoring
We are using specific counters to track network throughput for IPv4 and IPv6 separately.
It is exported via [nftables_counter_exporter](https://github.com/xvzf/nftables_counter_exporter). 
Alongside, we're using the `node_exporter` for assessing further metrics on system health.

### Flow logging
Elastics [packetbeat](https://www.elastic.co/en/beats/packetbeat) exports flows to an elasticsearch database running on an internal kubernetes cluster. It provides full flow logs in comparison to the SFLOW based approach on the core network.

# TODO

[This](This) is ported of initial ansbible roles in order to provide a standalone solution for other LAN-parties.

- [x] Create this repo
- [ ] Build a custom Alpine image (for SD or USB stick)
- [ ] Migrate
  - [ ] Port existing nftables configuration
  - [ ] Port existing FRRouting configuration
- [ ] Configuration
  - [ ] Option to IPv4/v6 (do not disable v6. It's the future)
  - [ ] Disable BGP (99% of the time it is cool overkill)
- [ ] Observability
  - [ ] Add nftables exporter
  - [ ] Add node_exporter
  - [ ] Add packetbeat
