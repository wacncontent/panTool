Although the use of system routes facilitates traffic automatically for your deployment, there are cases in which you want to control the routing of packets through a virtual appliance. You can do so by creating user defined routes that specify the next hop for packets flowing to a specific subnet to go to your virtual appliance instead, and enabling IP forwarding for the VM running as the virtual appliance.

Some of the cases where virtual appliances can be used include:

* Monitoring traffic with an intrusion detection system (IDS)
* Controlling traffic with a firewall

For more information about UDR and IP forwarding, visit [User Defined Routes and IP Forwarding](/documentation/articles/virtual-networks-udr-overview/).
