# Network_Programming-

from jnpr.junos.op.phyport import *
from jnpr.junos import Device

PhyPortTable:
  rpc: get-interface-information
  args:
    interface_name: '[fgx]e*'
  args_key: interface_name
  item: physical-interface
  view: PhyPortView
  
  PhyPortView:
  fields:
    oper : oper-status
    admin : admin-status
    mtu: { mtu : int }
    link_mode: link-mode
    speed: speed
    macaddr: current-physical-address
    flapped: interface-flapped
    

 
dev = Device( user='netconf-test', host='lab-switch', password='lab123' )
dev.open()
 
ports = PhyPortTable(dev).get()
 
print "Port,Status,Flapped" #Print Header for CSV
 
 
for port in ports:
        print("%s,%s,%s" % (port.key, port.oper, port.flapped))
