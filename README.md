# Ansible Role: maradns

This role installs and manages [MaraDNS](http://www.maradns.org/). It was developed and tested on Debian Stretch.

## Requirements

- Ansible 2.1+ (might ork with prior versions too)
- Debian-based linux-distribution

## Dependencies

None.

## Configuration example

```
    maradns_zones:
      - name: example.com
        email: support@example.com
        spf:
          - { val: 'v=spf1 ip4:212.41.224.0/24 -all' }
        txt:
          - { val: 'v=spf1 ip4:212.41.224.0/24 -all' }
          - { name: 'xmas', val: 'Merry Christmas' }
        ns:
          - { val: ns1.example.com. }
          - { val: ns2.example.com. }
          - { name: 'subdom.%', val: 'ns1.%' }
        mx:
          - { prio: 5, rec: mx.example.com. }
          - { prio: 10, rec: mx2.% }
        srv:
          - { name: "_sip._udp", val: "0 0 5060 sip.%" }
        fqdn4:
          - { domain: "mx", ip: "7.7.7.7" }
        ptr:
          - { domain: "www", ip: "8.8.8.8" }
        a:
          - { ip: 8.8.8.8 }
          - { domain: 'www', ip: 8.8.8.8 }
          - { domain: 'sip', ip: 6.6.6.6 }
```
This example produces the following csv2-file:


```
/origin example.com.
% SOA % support@example.com. /serial 3600 1800 604800 600 

%	NS	ns1.example.com. 
%	NS	ns2.example.com. 
subdom.%	NS	ns1.% 

%	MX	5 mx.example.com. 
%	MX	10 mx2.% 

%	TXT	'v=spf1 ip4:212.41.224.0/24 -all' 
xmas.%	TXT	'Merry Christmas' 

%	SPF	'v=spf1 ip4:212.41.224.0/24 -all' 


%	8.8.8.8 
www.%	8.8.8.8 
sip.%	6.6.6.6 

mx.%	FQDN4	7.7.7.7 

www.%	PTR	8.8.8.8 


_sip._udp.%	SRV	0 0 5060 sip.% 

```

# Licence

GPL

# Author information

This role was created in 2017 by [Wolfgang Hotwagner](https://tech.feedyourhead.at)
