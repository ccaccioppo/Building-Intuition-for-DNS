# Building-Intuition-for-DNS

<p align="center">
<img src="https://i.imgur.com/CtGfsq8.png" alt="osTicket logo"/>
</p>

<h1>Building Intuition for DNS</h1>
In this lab, we'll explore and experiment with the Domain Name System (DNS) to better understand how it works and its role in enabling network communication. Through hands-on activities, we'll deepen our knowledge of how DNS functions and its crucial job of translating domain names into IP addresses.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- DNS

<h2>Operating Systems Used </h2>

- Windows 10</b> (21H2)

<h2>List of Prerequisites</h2>

- A Virtual Machine with Active Directory
- A Client VM joined to your Network 

<h2>Lab Steps</h2>
<p>
<img src="https://i.imgur.com/2Wl0nRt.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Start by powering on the virtual machines and connecting to Client1 using Remote Desktop Protocol (RDP). Once connected, open PowerShell as an administrator and try pinging "mainframe." The ping attempt will fail because the system can't resolve "mainframe"—it's not stored in the local cache, the hosts file, or the DNS server, which is responsible for translating domain names into IP addresses.
</p>
<br />

<p>
<img src="https://i.imgur.com/4IPXilR.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next, run the command ipconfig /displaydns. You'll notice there is no entry for "mainframe" in the DNS cache.
</p>
<br />

<p>
<img src="https://i.imgur.com/XbNuQE3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next, run the command nslookup mainframe. Just like before, you won’t get any results.
</p>
<br />

<p>
<img src="https://i.imgur.com/pwQioSl.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now, let's create a DNS A-record for "mainframe" on DC-1, pointing it to DC-1’s private IP address. On the DC-1 VM, open DNS Manager from Administrative Tools. Navigate to DC-1 > Forward Lookup Zones > mydomain.com, then right-click inside mydomain.com and select New Host (A or AAAA). In the Name field, enter mainframe, and in the IP Address field, type DC-1’s private IP address. Once done, save the record.
</p>
<br />

<p>
<img src="https://i.imgur.com/zMrBLqR.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Go back to Client1 and try pinging "mainframe" in PowerShell again. This time, it should work because we added a DNS A-record on DC-1 that points to its private IP address.
</p>
<br />

<p>
<img src="https://i.imgur.com/hRa9Zos.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/xWA0TfZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Go back to DC-1 and update the DNS A-record for "mainframe" to 8.8.8.8. Once you've made the change, switch over to Client1 and try pinging "mainframe" again. You’ll see that it’s still using the old IP address.
</p>
<br />

<p>
<img src="https://i.imgur.com/8nSHNtq.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
On Client1, run the command ipconfig /displaydns and look for the entry labeled "mainframe." You'll notice that the A-record is still set to 10.0.0.4, showing a detailed view of the cached DNS information.
</p>
<br />

<p>
<img src="https://i.imgur.com/K7BQDEu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To make sure the updated A-record reflects the new IP address, run ipconfig /flushdns to clear the DNS cache. Once that’s done, you can check if the change took effect by using ipconfig /displaydns.
</p>
<br />

<p>
<img src="https://i.imgur.com/BGKH5xK.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now, try pinging "mainframe" again from Client1. This time, you’ll see that it successfully pings the new IP address, 8.8.8.8, which confirms that the updated A-record is in effect.
</p>
<br />

<p>
<img src="https://i.imgur.com/1Idq19o.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next, go back to the DC-1 VM and set up a CNAME record to point the host "explore" to "www.google.com." To do this, open the DNS Manager, right-click on mydomain.com under Forward Lookup Zones, and select New Alias (CNAME). In the Alias name field, type explore, and in the Fully Qualified Domain Name (FQDN) field, enter www.google.com. Once you’ve filled in the details, save the record to complete the process.
</p>
<br />

<p>
<img src="https://i.imgur.com/SI2VH6B.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Go back to the Client1 VM and try pinging "explore." You’ll notice that the ping successfully resolves to www.google.com, thanks to the CNAME record you set up earlier, which directs "explore" to that specific address.
</p>
<br />

<p>
<img src="https://i.imgur.com/ys286eQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Finally, run the nslookup explore command on Client1. The output should confirm that "explore" resolves to www.google.com, which means the CNAME record is functioning correctly.
</p>
<br />
