devstack-dvr-multinode
======================
I used the following  OpenStack setup for the blog post series about Openstack Juno DVR. 

East-West:          http://blog.gampel.net/2014_12_14_archive.html

DNAT- Floating IPs: http://blog.gampel.net/2014/12/openstack-dvr2-floating-ips.html

SNAT-               http://blog.gampel.net/2015/01/openstack-DVR-SNAT.html

In my setup all the controller services and the network services ran on one machine.

1) Clone the devstack into the  controller node and into all the compute nodes 
    
    cd /opt/stack
    git clone https://git.openstack.org/openstack-dev/devstack

2) Checkout the stable Juno relase  

    cd devstack 
    git checkout stable/juno 

3) Copy the appropriate  local.conf file controller/compute and modified it to match your host ip setup.

clone this repository 

    cd /opt/stack 
  
    git clone https://github.com/gampel/devstack-dvr-multinode.git
  
    cd devstack-dvr-multinode
  
On the controller node 

    cp controller-node/local.conf  /opt/stack/devstack 

On the computes nodes 

    cp controller-node/local.conf  /opt/stack/devstack 

4) Run stack.sh

5) Set up the external bridge 

Assuming  the external network is connected to eth3 

        ifconfig br-ex promisc up
        ifconfig eth3 0.0.0.0
        ifconfig eth3 promisc 
        ifconfig br-ex <address on ext>  netmask <mask>
        ovs-vsctl add-port br-ex eth3
        
6) Update the neurton configuration   to DVR mode 

I modified the following configuration On the controller Node  

On the controller Node 
* Neutron.conf 
  
    router_distributed = True

    dvr_base_mac = fa:16:3f:00:00:00
    
* l3_agent.ini 

    agent_mode = dvr_snat
    
    router_delete_namespaces = True

* ml2_conf.ini 
  
  [ml2]

    mechanism_drivers =openvswitch,l2population
  
  [agent]

    l2_population = True

    enable_distributed_routing = True
    
    arp_responder = True
    

I modified the following configuration  On all the Compute  Node 

 

* l3_agent.ini 
  agent_mode = dvr

  router_delete_namespaces = True


* ml2_conf.ini 

  mechanism_drivers =openvswitch,l2population
  
  [agent]
  
    l2_population = True
  
    enable_distributed_routing = True


7) For the floating ip association I used the neutron command line due to the horizon bug associating FIP 

            neutron  floatingip-associate           Create a mapping between a floating IP and a fixed IP.
