# Conti
<H4>Can you identify the location of the ransomware?</H4>
we can use "index=main | stats count by Image" as the search query to find all values in the Image field, searching all the list i found a suspicious file that should not be there.

![Image_1](Images%2FPasted%20image%2020230201211023.png)

<H4>What is the Sysmon event ID for the related file creation event?</H4>
using "C:\\\\Users\\\\Administrator\\\\Documents\\cmd.exe" as search query we can search for any hit on that command line. after that we need to find the oldest record of that hit. that record will contain this 

![Image_2](Images%2FPasted%20image%2020230201211509.png)
<H4>Can you find the MD5 hash of the ransomware?</H4>
clicking the record above the record from the previous question we can see the MD5 hash of the cmd.exe

![Image_3](Images%2FPasted%20image%2020230201211640.png)
<H4>What file was saved to multiple folder locations?</H4>
the answer is readme.txt, becasue from my experience usually malware that encrypt data will leave a readme.txt file in every folder that have been encrypted.
<H4>What was the command the attacker used to add a new user to the compromised system?</H4>
To complete this question we need to know how to add user using command in in windows. Searching in google give me this commandline 

```powershell
net user _username_ _password_ /add
```

from that code, we can search for wevery command line what have similarity to that code. Using "CommandLine=\*user\*", then click the "CommandLine" field we can find the Commandline that the attacker use to add a user.

![Image_4](Images%2FPasted%20image%2020230201210425.png)

From the this hint "Try Sysmon event code 8" we can search for Event ID. Therefore we can use this query "EventCode=8"

![Image_5](Images%2FPasted%20image%2020230201203700.png)

Then we need to compare the oldest timestamp with the newest timestamp. Then we just need to compare both "Source Image" field
```powershell
New SourceImage: C:\Windows\System32\wbem\unsecapp.exe
Old SourceImage: C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
```

<H4>The attacker also retrieved the system hashes. What is the process image used for getting the system hashes?</H4>
Next we just need to see the Target Image of the newsest process

```powershell
TargetImage: C:\Windows\System32\lsass.exe
```
<H4>What is the web shell the exploit deployed to the system?</H4>
Following the hint we can use this query "sourcetype=IIS POST" to find the answer
then click the "cs_uri_steam" field and look for suspecious uri

![Image_6](Images%2FPasted%20image%2020230201205807.png)
<H4>What is the command line that executed this web shell?</H4>
We can use  "CommandLine" field to find the answer. we can just type in the search box "CommandLine=\*i3gfPctK1c2x.aspx\*" . This command will search for any command line that contain string "i3gfPctK1c2x.aspx" inside.

![Image_7](Images%2FPasted%20image%2020230201200224.png)
after that we just need to show all lines and we will find this
```powershell
attrib.exe -r \\\\win-aoqkg2as2q7.bellybear.local\C$\Program Files\Microsoft\Exchange Server\V15\FrontEnd\HttpProxy\owa\auth\i3gfPctK1c2x.aspx
```
