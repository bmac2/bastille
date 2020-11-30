Bastille
========
[Bastille](https://bastillebsd.org/) is an open-source system for automating
deployment and management of containerized applications on FreeBSD. This
document outlines the testing procedures used in the CI/CD pipeline for
Bastille.

-----------

Testing Matrix
====================

Types of Containers
====================
1. Thin jails
2. Thick jails
3. Empty jails

Network Testing Requirements
====================
1. Virtual Networking   **VNET**
2. shared IP networking **IP Alias**
3. loopback networking  **bastille0**

Test Cases
====================
1. Thin jail, loopback networking
2. Thick jail, loopback networking
3. Empty jail, loopback networking
4. Thin jail, IP alias
5. Thick jail, IP alias
6. Empty jail, IP alias
7. Thin jail, VNET
8. Thick jail, VNET
9. Empty jail, VNET



-----------

Commands to be tested:
====================

* bootstrap   
    bastille bootstrap $test_template
    bastille list template | grep $test_template

* clone      
    bastille clone $testjail newtestjail $clone_ip
    bastille list | grep newtestjail

* cmd        
    bastille cmd $testjail freebsd-version

* config     
    bastille get $propertyName
    bastille set $newPropertyValue
    bastille get $propertyName

* console    
    bastille console $testjail
    bastille freebsd-version

* convert    
    bastille convert $testjail


* cp         
    bastille cp $testjail $hostfile $jailFile


* create     
    bastille create $testjail 12.2-RELEASE 

* destroy    


* edit       


* export     


* help       


* htop       


* import     


* limits     


* list       


* mount      


* pkg        
    bastille create pkgTest 12.2-RELEASE $testIP $testInterface
    bastille start pkgTest
    bastille pkg pkgTest install vim-console -y
    bastille cmd pkgTest which vim
    bastille destroy -f pkgTest

* rdr        


* restart    
    bastille restart 

* service    


* start      


* stop       


* sysrc      


* template   


* top       


* umount    
    See mount test, done in conjunction with mount

* update    


* upgrade   
    bastille create upgrade1 11.4-RELEASE $testIp
    bastille cmd upgrade1 freebsd-version > origVersion
    bastille upgrade upgrade1 12.2-RELEASE
    bastille cmd upgrade1 freebsd-version > newVersion
    if origVersion!=newVersion, success
    bastille destroy -f upgrade1

* verify    


* zfs       



```shell

The convert script is using this logic:
if [ ! -d "${bastille_jailsdir}/${TARGET}/root/.bastille" ]; then
    error_exit "${TARGET} is not a thin container."
elif ! grep -qw ".bastille" "${bastille_jailsdir}/${TARGET}/fstab"; then
    error_exit "${TARGET} is not a thin container."
fi


```

------------------

Community Support
=================
If you've found a bug in Bastille, please submit it to the [Bastille Issue
Tracker](https://github.com/bastillebsd/bastille/issues/new).
