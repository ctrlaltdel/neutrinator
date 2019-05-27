# Neutrinator

Did you ever had to manually trace traffic to or from an OpenStack instance?

This tool is hopefully useful for operators which already had to deep
dive into neutron complexity to figure out why an instance network
connectivity was broken.

Given the uuid of an instance running on OpenStack, it will show you all the
details on how the network plumbing is being setup by Neutron.

# Pre-requisites

1. Admin access to OpenStack APIs (environment variables or CLI arguments)
2. SSH configuration for password-less login on controllers and compute nodes
   using the same hostname than returned by OpenStack APIs

# Example usage

```console
root@deploy:~/neutrinator# ./neutrinator 465f35fb-f244-4944-b6ac-11a15fcb2661
Server 465f35fb-f244-4944-b6ac-11a15fcb2661
  Name:           k8s-ci-136-master
  Livirt domain:  instance-00000eb2
  Compute node:   os-compute1.openstack.local
    
  Port a3d85e15-c7e4-44a3-bf51-03fc02501a4f
    Status:       ACTIVE
    IPs:          10.8.10.12
    MAC:          fa:16:3e:af:5c:fe
    Type:         compute:nova
    Host:         os-compute1
    Binding Host: os-compute1
    Network:      6a83dc8f-202c-465d-9606-2496fd45bbdb
    Floating IP   172.0.0.10
    Path:
      Tap        tapa3d85e15-c7 
      Bridge     brq6a83dc8f-20 
      Vxlan      vxlan-94 (vxlan id 94 group 239.1.1.1 dev br-vxlan srcport 0 0 dstport 8472 ageing 300)
  
    DHCP Agent ba084440-2bb4-4557-9a48-53ffeb23f833

      Port ee017006-367a-4f9e-bb97-7a54330853fe
        Status:       ACTIVE
        IPs:          10.8.10.2
        MAC:          fa:16:3e:7e:17:f8
        Type:         network:dhcp
        Host:         os-infra1
        Binding Host: os-infra1
        Network:      6a83dc8f-202c-465d-9606-2496fd45bbdb
        Path:
          Veth       ns-ee017006-36 (qdhcp-6a83dc8f-202c-465d-9606-2496fd45bbdb)
          Tap        tapee017006-36 
          Bridge     brq6a83dc8f-20 
          Vxlan      vxlan-94 (vxlan id 94 group 239.1.1.1 dev br-vxlan srcport 0 0 dstport 8472 ageing 300)
      
    Router a7844dbb-afb4-44ec-9c47-601df4572261

      L3 Agent c8236017-21d7-428c-b413-69968aab82a3
        Role:     master
        Alive:    True
        State:    None
        Host:     os-infra1
        
        Port 382e1f94-952b-4e61-9c04-86c05537bdfb
        Status:       ACTIVE
        IPs:          172.0.0.5
        MAC:          fa:16:3e:a6:1d:29
        Type:         network:router_gateway
        Host:         os-infra1
        Binding Host: os-infra1
        Network:      9ca40e75-3298-44c2-922d-356e12ebb6aa
        Path:
          Veth       qg-382e1f94-95 (qrouter-a7844dbb-afb4-44ec-9c47-601df4572261)
          Veth       tap382e1f94-95 
          Bridge     brq9ca40e75-32 
          Interface  br-vlan.501 
      
          
        Port 3b7de152-fe94-482d-8ccb-2b830151d668
        Status:       ACTIVE
        IPs:          10.8.10.1
        MAC:          fa:16:3e:a6:89:a4
        Type:         network:router_interface
        Host:         os-infra1
        Binding Host: os-infra1
        Network:      6a83dc8f-202c-465d-9606-2496fd45bbdb
        Path:
          Veth       qr-3b7de152-fe (qrouter-a7844dbb-afb4-44ec-9c47-601df4572261)
          Veth       tap3b7de152-fe 
          Bridge     brq6a83dc8f-20 
          Vxlan      vxlan-94 (vxlan id 94 group 239.1.1.1 dev br-vxlan srcport 0 0 dstport 8472 ageing 300)
```

# Author

François Deppierraz <francois@ctrlaltdel.ch>

# License

GPLv3
