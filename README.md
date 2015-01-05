devstack-dvr-multinode
======================
The folwing is the setup i used for the blog post series about Openstack Juno DVR. 

http://blog.gampel.net/2014_12_14_archive.html

In my setup all the controller services and the network services ran on one machine.

I modified the following configuration  On the controller Node after running  ./stack 

On the controller Node 
* Neutron.conf 
  
    router_distributed = True

* l3_agent.ini 

    agent_mode = dvr_snat
    router_delete_namespaces = True

* ml2_conf.ini 
  
    mechanism_drivers =openvswitch,l2population
  [agent]

    l2_population = True
    enable_distributed_routing = True

I modified the following configuration  On the Compute  Node after running  ./stack 

On the compute nodes 

* l3_agent.ini 
  agent_mode = dvr
  router_delete_namespaces = True


* ml2_conf.ini 

  mechanism_drivers =openvswitch,l2population
  
  [agent]
    l2_population = True
    enable_distributed_routing = True


