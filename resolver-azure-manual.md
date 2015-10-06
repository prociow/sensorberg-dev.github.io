---
layout: page
title: Resolver Azure Manual
permalink: /resolver/azure-manual/
additionalNavigation : [
    { "title" : "Source Code",              "link" : "https://github.com/sensorberg-dev/resolver" },
    { "title" : "Edit this page",           "link" : "https://github.com/sensorberg-dev/sensorberg-dev.github.io/edit/master/resolver-azure-manual.md" }              
]
---
#The Resolver ist now available as an Azure Marketplace VM Image

After you successfully created your image, log in via SSH and log into the ```sb``` user.
 
```
    Hello and welcome to the interactive installation wizard for the Microsoft Azure
    Marketplace appliance covering the resolver service, brought to you by
    sensorberg. Usage of this appliance constitutes full agreement to the licensing
    terms of all sensorberg original and 3rd party software provided with this image.

[com.sensorberg.sh.resolver.setup:] 2015-08-17-09-02-36 - Press ENTER to acknowledge and proceed 

                                       ___                        
  ______ ____   ____   ________________\_ |__   ___________  ____  
 /  ___// __ \ /    \ /  ___/  _ \_  __ \ __ \_/ __ \_  __ \/ ___\ 
 \___ \\  ___/|   |  \\___ (  (_) )  | \/ \_\ \  ___/|  | \/ /_/ |
/______>\_____>___|__/______>____/|__|  |_____/\_____>__|  \___  / 
                                                          /_____/  
    configuration options:
    ======================
    (1)  -  SSH password
    (2)  -  SSH public key
    (3)  -  add/remove/edit a (virtual) server name
    (4)  -  add/remove/edit an SSL private key
    (5)  -  add/remove/edit an SSL certificate & trust chain
    (6)  -  verify/change API backend URL
    (7)  -  resolver service restart
    (8)  -  resolver service & wizard update(s)
    (9)  -  launch shell
    (0)  -  quit

[com.sensorberg.sh.resolver.setup:] 2015-08-17-09-02-37 - Please choose a configuration option using its associated key binding
```

 
If you want to setup the resolver to sync with the Sensorberg Cloud backend, follow the [instructions](/resolver) on the resolver documentation.
 
<div class="callout callout-info">
    <h1><i class='fa fa-info-circle'/></i>Some APIs are not exposed</h1>
    <p>Please note, not all APIs are exposed publicly on this image. Only /layout and /ping are available from outside of the machine. If you need to access any other endpoints, SSH in on the machine or create an SSH tunnel so you can work as if it is deployed locally</p>
    <p>If you want to map port remote:8080 to localhost:8080:
    <p>
        <pre>ssh -L 8080:localhost:8080 ${your-ssh-user}@${your-appliance-name}.cloudapp.net -p ${your-ssh-port}</pre>
    </p>
</div>
     
        
        
 

<br/>
<br/>
<br/>
<br/>
<br/>

