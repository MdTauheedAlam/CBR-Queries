### Process Search

###

`process_name:explorer.exe AND netconn_count:[500 TO *]`

`process_name:explorer.exe (modload:"c:\windows\syswow64\taskschd.dll")`


### Behavior

`-digsig_result_filemod:Signed process_name:rundll32.exe`

`process_name:cacls.exe cmdline:\startup\`

###

`-digsig_result_parent:Signed process_name:svchost.exe`

### DLL Hijack

c:\windows\system32\wbem\

`filemod:"wbem\loadperf.dll" OR filemod:"wbem\bcrypt.dll"`

###

`process_name:svchost.exe cmdline:RemoteRegistry`

###

`process_name:explorer.exe filemod:temp1_*.zip filemod:request*.doc`

`process_name:winword.exe cmdline:request*.doc\"`

`process_name:explorer.exe filemod:temp1_*.zip filemod:request*.doc`

###

`process_name:mode.com`

`digsig_result_parent:Unsigned process_name:raserver.exe`


### scrobj load and behavior

`process_name:regsvr32.exe (modload:scrobj.dll) AND childproc_name:powershell.exe`

`parent_name:powershell.exe AND process_name:nslookup.exe AND netconn_count:[1 TO *]`

### Java Embedded MSI files
`process_name:java.exe cmdline:-classpath parent_name:javaw.exe (childproc_name:java.exe or childproc_name:conhost.exe)`

`process_name:java.exe cmdline:-classpath parent_name:javaw.exe (childproc_name:java.exe or childproc_name:conhost.exe) filemod:appdata\local\temp\*.class`

[API](https://github.com/cparmn/CarbonBlackResponse/blob/master/msijar.py)


### uacbypass

[Reference](https://enigma0x3.net/2016/08/15/fileless-uac-bypass-using-eventvwr-exe-and-registry-hijacking/)

    regmod:"mscfile\shell\open\command"

<br>

    parent_name:powershell.exe process_name:eventvwr.exe

### PPID Spoofing - Explorer CLSID

    process_name:rundll32.exe cmdline:Shell32.dll*  cmdline:SHCreateLocalServerRunDll cmdline:{c08afd90-f2a1-11d1-8455-00a0c91f3880}

### Browser Broker

    process_name:browser_broker.exe digsig_result_child:Unsigned

### Emotet stuff

    parent_name:userinit.exe digsig_result_process:Unsigned

### Something new

    parent_name:powershell.exe process_name:csc.exe


### Some stuff here


[reference](https://ti.360.net/blog/articles/latest-target-attack-of-darkhydruns-group-against-middle-east-en/)

    parent_name:powershell.exe process_name:nslookup.exe

### csc spawns

    (process_name:excel.exe OR process_name:winword.exe OR process_name:outlook.exe) childproc_name:csc.exe

<br>

    (process_name:excel.exe OR process_name:winword.exe OR process_name:outlook.exe) filemod:.cs

### Domain Enumeration

https://github.com/clr2of8/DPAT

    process_name:ntdsutil.exe

<br>

    process_name:dcdiag.exe

<br>

    process_name:repadmin.exe

<br>


    process_name:netdom.exe

<br>


    company_name:"http://www.joeware.net"


### EternalBlue - Miner

[Reference](https://labsblog.f-secure.com/2019/01/03/nrsminer-updates-to-newer-version/)

    parent_name:wininit.exe process_name:spoolsv.exe

<br>

    process_name:sc.exe cmdline:Snmpstorsrv

<br>

    process_name:svchost.exe digsig_result_modload:unsigned


### SQLi Dumper + spam

    filemod:"url exploitables.xml"

<br>

    filemod:"url list.txt"

<br>

    process_name:"sqli dumper.exe"

<br>

    process_name:"advanced mass sender.exe"

<br>

    process_name:"turbomailer.exe"

<br>

    modload:"appvirtdll64_advanced mass sender.dll"

<br>

    process_name:storm.exe



### CobaltStrike - Argue

    (modload:"c:\windows\syswow64\ntmarta.dll") process_name:svchost.exe

<br>

    (modload:"c:\windows\system32\wshtcpip.dll") digsig_result:Unsigned (modload:"c:\windows\system32\wship6.dll")

<br>

    (modload:"c:\windows\syswow64\iertutil.dll" modload:"c:\windows\syswow64\ntmarta.dll") process_name:rundll32.exe

<br>

    (modload:"c:\windows\syswow64\iertutil.dll" modload:"c:\windows\syswow64\ntmarta.dll") process_name:rundll32.exe AND netconn_count:[1 TO * ]

<br>

    digsig_result_parent:Unsigned (process_name:svchost.exe -username:SYSTEM -username:"NETWORK SERVICE" -username:"LOCAL SERVICE" -cmdline:"UnistackSvcGroup")

<br>

    digsig_result_parent:Unsigned process_name:svchost.exe

<br>

    parent_name:rundll32.exe process_name:svchost.exe


### Process

    (regmod:"\registry\user\.default\software\microsoft\windows\currentversion\internet settings\proxyenable") digsig_result:Unsigned AND path:c:\windows\syswow64\*

<br>

    process_name:procdump.exe cmdline:-accepteula

<br>

    process_name:procdump.exe cmdline:lsass.exe

<br>

    digsig_result_parent:Unsigned process_name:explorer.exe

<br>

    process_name:schtasks.exe cmdline:/c

<br>

    process_name:schtasks.exe cmdline:"cscript.exe"

<br>

    process_name:schtasks.exe cmdline:"wscript.exe"

<br>

    process_name:schtasks.exe cmdline:"powershell.exe"


<br>

    crossproc_type:"remotethread" AND -process_name:wmiprvse.exe -process_name:svchost.exe -process_name:csrss.exe



### klist

[Reference](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/klist)

    process_name:klist.exe

### dfsvc/browser broker queries

    parent_name:browser_broker.exe process_name:mshta.exe

<br>

    parent_name:browser_broker.exe process_name:rundll32.exe

<br>

    process_name:dfsvc.exe digsig_result_child:"Unsigned" OR digsig_result_child:"Untrusted Root"

<br>

    process_name:rundll32.exe childproc_name:dfsvc.exe

<br>

    is_executable_image_filewrite:True -path:google\chrome\* and -path:google\update\* -digsig_result_filewrite:Signed filemod:local\settings\* filemod:appdata\local\temp\*

##

<br>

    process_name:lsass.exe digsig_result_filewrite:"Unsigned"

<br>

    process_name:lsass.exe AND digsig_result_modload:"Unsigned"

<br>

    filemod:Content.Outlook\*  and is_executable_image_filewrite:True

<br>

    filemod:Content.Outlook\*  and -digsig_result_filewrite:Signed

<br>

    process_name:winlogon.exe AND netconn_count:[1 TO *]

<br>

    filemod: ???Start Menu\Programs\Startup???

<br>

    regmod:CurrentVersion\Run*

<br>

    filemod:windows\system32\* digsig_result:unsigned digsig_result_filewrite:"Unsigned"

<br>

    regmod:services\national* digsig_result:unsigned

<br>

    regmod:services\svchostc* digsig_result:unsigned

<br>

    path:windows\system32\* digsig_result:unsigned parent_name:services.exe childproc_count:1

<br>

    (regmod:"\registry\user\s-1-5-21-348440682-330175067-1304115618-242891\software\microsoft\office\14.0\excel\security\accessvbom")

<br>

    (process_name:cmd.exe OR process_name:powershell.exe OR process_name:wmic.exe OR process_name:msbuild.exe OR process_name:mshta.exe OR process_name:wscript.exe OR process_name:cscript.exe OR process_name:installutil.exe OR process_name:rundll32.exe OR process_name:regsvr32.exe OR process_name:msxsl.exe OR process_name:regasm.exe OR process_name:regsvcs.exe) (domain:pastebin.com OR domain:dl.dropboxusercontent.com OR domain:githubusercontent.com)

<br>

    digsig_result_child:"Unsigned" ((parent_name:chrome.exe OR parent_name:firefox.exe OR parent_name:iexplore.exe OR parent_name:microsoftedge.exe OR parent_name:outlook.exe) is_executable_image_filewrite:"true")

<br>

    digsig_result_process:"Unsigned" ((parent_name:chrome.exe OR parent_name:firefox.exe OR parent_name:iexplore.exe OR parent_name:microsoftedge.exe OR parent_name:outlook.exe) is_executable_image_filewrite:"true")

<br>

    regmod:"keyboard layout\2"

<br>

    (regmod:"\registry\machine\software\microsoft\windows nt\currentversion\image file execution options\cmd.exe\verifierdlls")

<br>

    process_name:mshta.exe modload:mscoree.dll

<br>

    (modload:mscoree.dll AND modload:system.management.automation.dll) -process_name:powershell_ise.exe -process_name:sdiagnhost.exe -process_name:mscorsvw.exe -process_name:powershell.exe -process_name:searchfilterhost.exe

<br>

    process_name:netsh.exe cmdline:appdata/

<br>

    (modload:mscoree.dll AND modload:system.management.automation.dll AND modload:mscorlib*) -process_name:powershell_ise.exe -process_name:sdiagnhost.exe -process_name:mscorsvw.exe -process_name:powershell.exe -process_name:searchfilterhost.exe

<br>

    (cmdline:/user: OR cmdline:/pwd: OR cmdline:/username: OR cmdline:/password:)

<br>

    process_name:notepad.exe (modload:vaultcli.dll AND modload:samlib.dll)

<br>

    parent_name:explorer.exe process_name:lsass.exe

<br>

    process_name:netsh.exe cmdline:ProgramData/

<br>

    process_name:csc.exe netconn_count:[1 TO *]

<br>

    path:programdata\* -path:programdata\*\* -process_name:chgservice.exe -process_name:userprofilemigrationservice.exe -process_name:mm.exe -process_name:mmimage.exe

<br>

    process_name:rundll32.exe domain:.ru AND netconn_count:[1 TO *]

<br>

    (process_name:powershell.exe OR internal_name:powershell) (modload:samlib.dll OR modload:vaultcli.dll)

<br>

    parent_name:spoolsv.exe (process_name:cmd.exe OR process_name:powershell.exe)

<br>

    digsig_result:Unsigned ipport:443 modload:winsta.dll path:appdata/local/temp/*

### BloodHound detection - network

    ipport:445 AND netconn_count:[150 TO *] AND -process_name:ntoskrnl.exe AND process_name:*

### BloodHound detection - command line

    cmdline:--ExcludeDC OR cmdline:LoggedOn OR cmdline:ObjectProps OR cmdline:GPOLocalGroup OR product_name:"SharpHound"

### BloodHound detection - file modifications

    filemod:sessions.csv OR filemod:acls.csv OR filemod:group_membership.csv OR filemod:local_admins.csv OR filemod:computer_props.csv OR filemod:user_props.csv

### BloodHound detection - network pipe

    filemod:\pipe\samr AND filemod:\pipe\lsarpc AND filemod:pipe\srvsvc

<br>

    netconn_count:[100 TO *] AND ipport:445 AND (filemod:lsarpc OR filemod:samr OR filemod:srvsvc)

### Squiblytwo

    (process_name:wmic.exe OR internal_name:wmic.exe) (cmdline:format:\ AND cmdline:os)

<br>

    (process_name:wmic.exe OR internal_name:wmic.exe) (cmdline:format:\ AND cmdline:os) AND netconn_count:[1 TO *]

<br>

    (process_name:wmic.exe OR internal_name:wmic.exe) netconn_count:[1 TO *]

<br>

    process_name:wmic.exe (modload:jscript.dll OR modload:vbscript.dll)

## More

    process_name:powershell.exe (filemod:c:\windows\temp\*)

<br>

    process_name:powershell.exe ipport:445

<br>

    process_name:powershell.exe AND netconn_count:[2 TO *] ipport:445

<br>

    process_name:powershell.exe AND netconn_count:[2 TO *] (ipport:445 OR ipport:80 OR ipport:443 OR ipport:137 OR ipport:138 OR ipport:135 OR ipport:22)

<br>

    (process_name:powershell.exe or process_name:powershell_ise.exe) AND netconn_count:[2 TO *] (ipport:445 OR ipport:80 OR ipport:443 OR ipport:137 OR ipport:138 OR ipport:135 OR ipport:22)

<br>

    parent_name:explorer.exe process_name:mshta.exe (modload:jscript.dll OR modload:vbscript.dll) netconn_count:[1 TO *]

<br>

    process_name:mshta.exe (modload:jscript.dll OR modload:vbscript.dll) netconn_count:[1 TO *]

<br>

    process_name:php.exe childproc_name:cmd.exe

<br>

    parent_name:php.exe process_name:cmd.exe

<br>

    parent_name:php.exe process_name:cmd.exe digsig_result_child:"Unsigned"

<br>

    (filemod:wwwroot\* or filemod:htdocs\*) and (filemod:.aspx or filemod:.jsp or filemod:.cfm or filemod:.asp or filemod:.php) AND host_type:"server"

<br>

    parent_name:outlook.exe (process_name:iexplore.exe OR process_name:chrome.exe OR process_name:microsoftedge.exe OR process_name:firefox.exe)

<br>

    domain:.ru -process_name:iexplore.exe OR -process_name:chrome.exe OR -process_name:microsoftedge.exe OR -process_name:microsoftedgecp.exe OR -process_name:firefox.exe OR -process_name:opera.exe digsig_result:Unsigned


### Rogue DC

    process_name:svchost.exe AND cmdline:"-k netsvcs -p -s gpsvc" AND domain:* AND -(ipaddr:172.20.1.200 OR ipaddr:10.100.12.4 OR ipaddr:172.20.0.117 OR ipaddr:10.100.86.75 OR ipaddr:10.254.1.120 OR ipaddr:10.254.1.69 OR ipaddr:10.254.1.121)

<br>

    process_name:svchost.exe AND cmdline:"-k netsvcs -p -s gpsvc" AND domain:* AND -host_type:"domain_controller"

### Coin Miner

    digsig_result:"Unsigned" company_name:"Zhuhai Kingsoft Office Software Co.,Ltd"

<br>

    parent_name:lsass.exe process_name:cmd.exe childproc_name:reg.exe

<br>

    parent_name:lsass.exe process_name:cmd.exe childproc_name:schtasks.exe

<br>

    process_name:net1.exe cmdline:"net1 user IISUSER_ACCOUNTXX /del"

<br>

    process_name:lsass.exe digsig_result_filewrite:"Unsigned"

<br>

    company_name:"TODO: <?????????>"

<br>

    parent_name:conhost.exe digsig_result_parent:"Unsigned"


https://github.com/fireice-uk/xmr-stak

<br>

    filemod:xmrstak_opencl_backend.dll

<br>

    filemod:xmrstak_cuda_backend.dll

<br>

    observed_filename:c:\windows\debug\

<br>

    observed_filename:c:\windows\inf\

<br>

    observed_filename:c:\windows\web\
