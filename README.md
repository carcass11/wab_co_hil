#Hil transceiver firmware

#**********************************************************************************************

#V 1.0.0:

#	- UART/USB as STDOUT (8bit frame with 9600 BAUD, 1 stop bit, no parity checks or flow controls)
#	- Video input processing only
#	- No ARP support (Static Ethernet MAC : 00-0A-35-01-02-03)
#	- Ethernet as video frames input (accepts UDP packets to 1010 port)
#	- HDMI0 (1920x1080x2) and HDMI1(1024x512x2)
#	- YUV422 (16bit) as image format in DDR3 frame buffers
#	- Flushing images to HDMI1 frame buffers
#	- CAM device frame is poiniting to HDMI1 frame
#	- Two frame buffers for each HDMI video devices (HDMI0 and HDMI1). 
#	  The one is constantly polling by the video device 
#	  and the second for frame buffer thats currently collecting over Ethernet input 
#	  and when collected it`s flushed to video device frame for the presentation

#Notes:

#	When system launched and ready being accepting input the STDOUT outputs the following:
		
#		Initialising system...
#		Initialising transceiver...
#		Ready to accept input
	
#	No ARP support hence ethernet packets needed to be routed staticly to the system:

#		- Configure PC NIC as follows: ip - 192.168.1.X, netmask - 255.255.255.0 
#		  where X any node number except routing hardware one if any and the one that`s specified by the transmitter as target IP (see "WabCo HIL transmitter (PC software)" below)

#		- Add ARP entry to ARP table on PC side via "arp -s {target_ip} 00-0A-35-01-02-03 {pc_nic_ip}"
#		  where {target_ip} is a target IP specified by transmitter and {pc_nic_ip} is an IP specified above for the PC NIC 

#		- When: 
#			- Connected via switch add L2 routing entry to switch`s L2 routing table i.e. an entry for MAC address (00-0A-35-01-02-03) to physical RJ45 port mapping
#		  	  Hence switch usage with possibility of manual L2 static routing is required  	
#			- Not connected via switch no further actions are required

#**********************************************************************************************

#HIL transmitter (PC software)

#**********************************************************************************************

#V 1.0.0:

#	- Transmits video only over UDP
#	- Transmission endpoint defined staticly (192.168.1.3:1010)
#	- Accepts (*.dvl) CAN data as input parameter (e.g. hil.exe test.dvl)
#	- When *.dvl is specified looks up *.avi by the same file name (e.g. test.avi)

#Notes:
#	The only one command line pattern accepted like as follows "hil.exe {dvl_file_name}.dvl" 	

#**********************************************************************************************