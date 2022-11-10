PS-Remoting was enabled by default (Win server 2022)

change hostname via `sconfig` to DC-01
change ip to 192.168.174.50
change DNS to ourself 192.168.174.50

Installed AD-Domain-Services

```
install-windowsfeature AD-Domain-Services -IncludemanagementTools
```

Create the Forest
```
import-Module ADDSDeployment
install-ADDSForest
```

I've chosen a random mexican last name: carballo.local

The DNS has changed after installing the forest. Let's change it again: to our IP.

```
Get-NetIPAddress -IPAddress $DC_IP
Set-DnsClientServerAddress -InterfaceIndex 5 -ServerAddresses $DC_IP
```


## Workstations and Servers setup
The workstations/servers will be joined automatically by the script.
Just provided their `hostname` value.

Note: If you clone VMs, you could need to [generate a new SID](https://mivilisnet.wordpress.com/2017/06/29/changing-sid-of-cloned-vms/).

Before running the script, be sure to:
* enable wmi 
* change DNS to DC IP
* disable fireall
* change Newtork type to Private

**Enable wmi**

```
winrm quickconfig
```

**Change DNS to DC** //TODO: automate this

```
Get-NetIPAddress -IPAddress $MACHINE_IP
Set-DnsClientServerAddress -InterfaceIndex 5 -ServerAddresses $DC_ADDRESS
```




