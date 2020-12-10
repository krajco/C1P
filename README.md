### Initializing
```
enable
configure terminal
no ip domain-lookup
ipv6 unicast-routing
```
### Keychain
```
key chain c1p
 key 1
  key-string FIT
  cryptographic-algorithm md5
end
```

### SSH
```
conf t
username C1P password C1P
ip ssh version 2
crypto key generate rsa label ssh modulus 1024
  line vty 0 4
  login local
  transport input ssh
  end
```

```
ssh -l C1P 10.1.35.1
```

### IS-IS
Convertuje sa IP addresa loopbacku, a taktiež musí byť pridaný interface loopbacku do IS IS.
```
conf t
router isis
  net 49.0001.<convertovana ip>.00
```
```
int e0/0
  no sh
  ip addr 192.168.1.1 255.255.255.0
  ipv6 enable
  ipv6 address fc:0:1::1/64
  clns router isis
  ip router isis
```
## RIPv2
Rip configure
```
router rip
  version 2
  no auto-summary
  network 192.168.1.1
```

Propagacia default route
```
router rip
  default-information originate
```

```
inte e0/0
  ip rip authentication mode md5
  ip rip authentication key-chain C1P
```

## EIGRP
```
router eigrp C1P
  address-family ipv4 unicast autonomous-system 1
  af-interface default
    shutdown
    exit
  address-family ipv6 unicast autonomous-system 1
  af-interface default
    shutdown
    exit
```


Vytvorenie routrovania v name mode
Aplikácia autentizácie
```
en
conf t
router eigrp C1P
  address-family ipv6 unicast autonomous-system 1
    af-interface e0/0
    authentication mode md5
    authentication key-chain C1P
    exit
  exit
exit
```

## OSPF
Key chain authorizácia
```
router ospf 1
  area 0 authentication
```
```
int e0/0
ip ospf authentication key-chain C1P
```

## BGP
```
router bgp 65100
  neighbor 192.168.10.10 remote-as 65200
  neighbor 192.168.10.10 password CiscoJeNaj
  address-family ipv4 unicast
    neighbor 192.168.10.10 activate
    exit
 ```
 
 Agregácia 

 ```
router bgp 65100
  address-family ipv4 unicast
  aggregate-address 192.168.0.0 255.255.255.0
  exit
 ```
 
 Propagácia suseda ako default routu
 ```
router bgp 65100
  address-family ipv4 unicast
  neighbor 192.168.1.1 default-originate
```
iBGP peer group, bez predchadzajuceho peeringu.
 ```
 router bgp 65100
    neighbor iBGP-PeerGrp peer-group
    neighbor iBGP-PeerGrp remote-as 65100
    neighbor iBGP-PeerGrp update-source Loopback1
    neighbor 192.168.1.1 peer-group iBGP-PeerGrp
  ```


