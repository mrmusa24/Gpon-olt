# Gpon-olt
Manage IP port 192.168.50.100 enabled ssh and telnet.
ONT configuration examples all ports in the same VLAN
Vlan 26 smart

port vlan 26 0/2 0

#We create interface if we need it
Interface vlanif10
IP address 192.168.x.x 255.255.255.0

#configure the management port
Interface meth 0
IP address 192.168.1.1. 255.255.255.0

#We enable ssh /telnet
Sysman service ssh enable
Sysman service telnet enable

#create a user to use ssh/telnet
Terminal user name “we type and it guides us to create a user”

 
Service-profile:
----------------

ont-srvprofile gpon profile-id 50 profile-name HG8342
ont-port pots 2 eth 4

port vlan eth 1-4 26

commit
quit



Line-profile:
-------------

ont-lineprofile gpon profile-id 50 profile-name HG8342
tcont 4 dba-profile-id 10
gem add 50 eth tcont 4
gem mapping 50 0 vlan 26
commit
quit




Info need for ONT add (F/S/P=Frame/Slot/PON_port):
--------------------------------------------------
ont-lineprofile-id: 50
ont-srvprofile-id: 50
GEM ID: 50
-----------------------------


gpon F/S interface
ont add p sn-auth 485754437FD4E55A omci ont-lineprofile-id 50 ont-srvprofile-id 50 desc "Customer_Name/ID"


Native VLAN (p=PON_port, Y=ONT ID):
-----------------------------------


gpon F/S interface
ont port native-vlan p and eth 1 vlan 26
ont port native-vlan p and eth 2 vlan 26
ont port native-vlan p and eth 3 vlan 26
ont port native-vlan p and eth 4 vlan 26



Service port (x=service-port Value it is unique for each ONT/service vlan, F/S/P=Frame/Slot/PON_port):
-------------------------------------------------- -------------------------------------------------- --

service-port x vlan 26 gpon F/S/P ont y gemport 50 multi-service user-vlan 26 tag-transform translate


#ONT configuration examples all ports in different VLAN
vlan 20-23 smart

port vlan 20-23 0/2 0

Service-profile:
---------------
ont-srvprofile gpon profile-id 50 profile-name HG8342
ont-port pots 2 eth 4

port vlan eth 1 20
port vlan eth 2 21
port vlan eth 3 22
port vlan eth 4 23
commit
quit

Line-profile:
-------------
ont-lineprofile gpon profile-id 50 profile-name HG8342
tcont 4 dba-profile-id 9
gem add 50 eth tcont 4
gem mapping 50 0 vlan 20
gem mapping 50 1 vlan 21
gem mapping 50 2 vlan 22
gem mapping 50 3 vlan 23
commit
quit
Info need for ONT add (F/S/P=Frame/Slot/PON_port):
--------------------------------------------------
ont-lineprofile-id: 50
ont-srvprofile-id: 50
GEM ID: 50
-----------------------------


gpon F/S interface
ont add p sn-auth 485754437FD4E55A omci ont-lineprofile-id 50 ont-srvprofile-id 50 desc "Customer_Name/ID"


Native VLAN (p=PON_port, Y=ONT ID):
-----------------------------------

ont port native-vlan p and eth 1 vlan 20
ont port native-vlan p and eth 2 vlan 21
ont port native-vlan p and eth 3 vlan 22
ont port native-vlan p and eth 4 vlan 23



Service port (x=service-port Value it is unique for each ONT/service vlan, F/S/P=Frame/Slot/PON_port):
-------------------------------------------------- -------------------------------------------------- --

service-port x vlan 20 gpon F/S/P ont y gemport 50 multi-service user-vlan 20 tag-transform translate
service-port x vlan 21 gpon F/S/P ont y gemport 50 multi-service user-vlan 21 tag-transform translate
service-port x vlan 22 gpon F/S/P ont y gemport 50 multi-service user-vlan 22 tag-transform translate
service-port x vlan 23 gpon F/S/P ont y gemport 50 multi-service user-vlan 23 tag-transform translate








#Example configured in the Marjal Tarragona client We create Vlans in the olt
  vlan 13 to 13 smart
vlan 19 to 19 smart
#We create and assign vlan to the port through which we pass the vlans
port vlan 13,19,80 0/3 0
#We create a profile, only 1 is necessary, it is independent of the vlan
ont-srvprofile gpon profile-id 19 profile-name Internet_Vlan_19
#We configure the ports according to the ONTS model
ont-port pots adaptive 32 eth adaptive 13 catv adaptive 8
#we define the vlans that we are going to use, for example passing them to a port for each vlan
   port vlan eth 1 translation 19 user-vlan 11
   port vlan eth 3 translation 19 user-vlan 13
   port vlan eth 3 translation 19 user-vlan 19
   port vlan eth 4 translation 19 user-vlan 80
#We finish configuring the ont-lineprofile with the service profile that we created previously.
  ont-lineprofile gpon profile-id 19 profile-name "Internet_VLAN_19"
   tcont 4 dba-profile-id 13
#we map the gems with the previous Tcont and the vlans
  gem add 19 eth tcont 4
gem add 19 eth tcont 4
gem mapping 19 0 vlan 11
gem mapping 19 1 vlan 13
gem mapping 19 3 vlan 19
gem mapping 19 3 vlan 80
commit
quit

#EXAMPLE OF CONFIGURATION OF A NEW ONT
interface gpon 0/0
#
ont add 5 sn-auth 485754430407C39E omci ont-lineprofile-id 19 ont-srvprofile-id 19 desc "10.30.19.225"
#
ont port native-vlan 5 3 eth 1 vlan 11
ont port native-vlan 5 3 eth 3 vlan 13
ont port native-vlan 5 3 eth 3 vlan 19
ont port native-vlan 5 3 eth 4 vlan 80
quit
#
service-port vlan 11 gpon 0/0/5 ont 3 gemport 19 multi-service user-vlan 1
