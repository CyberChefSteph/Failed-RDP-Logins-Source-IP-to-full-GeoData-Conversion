<h1>Failed RDP Logins : Source IP to full GeoData Conversion</h1>

<h2>Description</h2>
<b>The Powershell script ( not my own ) in this repository is responsible for parsing out Windows Event Log information for failed RDP attacks and using a third party API to collect geographic information about the attackers location.
</b>
<br />
<br />
I used this script in this project where I setup Azure Sentinel (SIEM) and connected it to a live virtual machine acting as a honey pot.
I was able to observe and collect data from all around the world from the attempted attacks. I used the PowerShell script to
look up the attackers Geolocation information and plot it on an Azure Sentinel Map.
<br />
<br />


<h2>Languages and Utilities Used</h2>

- <b>Remote Desktop Connection</b>
- <b>PowerShell : Extract RDP failed logon logs from Windows Event Viewer</b>
- <b>Microsoft Azure</b>
- <b>ipgeolocation.io:</b> IP Address to Geolocation API

<h2>Environments Used </h2>

- <b>Windows 10</b> (21H2)(Sentinel)
- <b>VirtualBox Windows 10</b>
<h2>Project walk-through:</h2>

<p align="center">
Creating high inbound vulnerability for the purpose of testing VM. This step essentially makes the VM very enticing and visible for potential malicious actors: <br/>
<img src="https://imgur.com/ZFthEQA.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Sentinel virtual machine successfully deployed:  <br/>
<img src="https://imgur.com/DvOZl19.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Creating a log ananlytics workspace to injest the logs from my newly created VM: <br/>
<img src="https://imgur.com/qr8djpt.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Log analytic workspace successfully deployed:  <br/>
<img src="https://imgur.com/uqZilGW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Selecting the defender plan settings for my VM:  <br/>
<img src="https://imgur.com/KKX7O3t.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Setting up my honeypot log data collection settings to collect ALL EVENTS:  <br/>
<img src="https://imgur.com/9lWBDKZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Connecting my log analytics Workspace to my VM:  <br/>
<img src="https://imgur.com/PXn2tv9.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
Adding Sentinel ( Microsoft's Cloud Native SIEM ) to Log analytics workspace: <br/>
<img src="https://imgur.com/gvBJ1MO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Using my VirtualBox Windows VM to Remote Desktop into my Azure/Sentinel VM:  <br/>
<img src="https://imgur.com/5qc2wqm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Turning off the firewalls on my Azure VM so that my Virtualbox windows VM can ping it and essentially any inbound source can ping it, creating the vulvnerability for the sake of gathering details from the logs:  <br/>
<img src="https://imgur.com/dQYSjCE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
After turning off firewalls, I used ping 23.102.117.185 -t to get confirmation it was now in a vulnerable state as before the ping returned a " Request timed out " message:  <br/>
<img src="https://imgur.com/eEtxYhw.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Used Powershell ISE to run a Log Exporter script ( not my own ). The script looks through the event security log on the VM, grabs all the events of people who failed to login to the vulnerable VM through RDP and gathers the geodata for the login ( including IP address, latitude, longitude, username, country, state, and time/date ):  <br/>
<img src="https://imgur.com/C86ZyBg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
Confirmation that the script was able to run successfully and geolocate failed login attempts to the VM:  <br/>
<img src="https://imgur.com/81VksDV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Uploading the Log results gathered from the Powershell script to train the Log Ananlytics of the Azure VM on what to look for: <br/>
<img src="https://imgur.com/m5Grajb.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Setting the collection path/ identifying where the log lives on the VM:  <br/>
<img src="https://imgur.com/chHDPiZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Did a search for SecurityEvent where the EventID = 4625 which essentially showed me all the failed RDP login attempts I performed as a test:  <br/>
<img src="https://imgur.com/2uFMGR9.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Setting up my honeypot log data collection settings to collect ALL EVENTS:  <br/>
<img src="https://imgur.com/9lWBDKZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Extracting Raw Data from the logs to create custom fields for the geodata of the failed RDP login source addresses:  <br/>
<img src="https://imgur.com/Iabyvk2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
Naming and data classification of the fields: <br/>
<img src="https://imgur.com/Zvu2x0U.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Ensuring Log Analytics is idenitfying the correct values; Custom field for longitude being created:  <br/>
<img src="https://imgur.com/lKwABWN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Verified that my custom created fields are now all logging data as milicious actors try to access the VM:  <br/>
<img src="https://imgur.com/mDtucFL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Selecting the parameters for how I want the data to display on the map: <br/>
<img src="https://imgur.com/UwGWNer.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Map displaying the geodata of the failed login attempts on my VM during a 3 hour timespan:  <br/>
<img src="https://imgur.com/HGuotQE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
