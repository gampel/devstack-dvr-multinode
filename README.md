devstack-dvr-multinode
======================
I used the following  OpenStack setup for the blog post series about Openstack Juno DVR. 

http://blog.gampel.net/2014_12_14_archive.html

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

    cp controller-node/local.confntroller-node/local.conf /opt/stack/devstack 

On the computes nodes 

    cp controller-node/local.conf  /opt/stack/devstack 

4) Run stack.sh

5) Update the neurton configuration   to DVR mode 

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


