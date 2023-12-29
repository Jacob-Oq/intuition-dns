<p align="center">
<img src="https://i.imgur.com/MwrkwEQ.png" alt="DNS Photo"/>
</p>

<h1>Understanding DNS</h1>
This lab focuses on DNS and how it is used. DNS is a fundamental concept in IT and many sources go over the theory behind it. I will be configuring DNS records and seeing how it works in practice. This is building up from a previous lab where I have a client joined to a domain. I am logged in as an admin account on both the client and domain controller VMs. In order for the lab to work, you need to be logged in as an administrator. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- Command Prompt

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 Pro (21H2)

<h2>DNS Configuration Steps</h2>
<p>While logged in to the client as an admin, open the command prompt. We wil ping "mainframe" and it will fail.</p>

![DNS 1](https://github.com/Jacob-Oq/intuition-dns/assets/150084528/35d44a3c-8ec8-4acd-9b0a-893978bad6a0)

![DNS 2](https://github.com/Jacob-Oq/intuition-dns/assets/150084528/81f4d3bc-af0d-438e-87ff-648a19b431f6)

<p>
Nslookup "mainframe" provides similar results and shows that there is no DNS record for it. A DNS A-record will have to be created for mainframe on the domain controller. On the domain controller, open the DNS Manager and go to the domain you created within the Forward Lookup Zones tab. 
</p>

![DNS 3](https://github.com/Jacob-Oq/intuition-dns/assets/150084528/d22953b3-af5a-4f5e-b76f-8a98af47c666)

![DNS 4](https://github.com/Jacob-Oq/intuition-dns/assets/150084528/a9a10342-3df0-4863-96f8-fbea02c6cdf6)

![DNS 5](https://github.com/Jacob-Oq/intuition-dns/assets/150084528/ce855be5-3ead-49c5-b451-4baf72361d8a)

<p>  
Right click on the page and create a New Host. For name, input mainframe. The IP address should be the same as the domain controller so that ping can resolve. Refresh the DNS server so the new record can be updated. 
</p>

![DNS 6](https://github.com/Jacob-Oq/intuition-dns/assets/150084528/df032e51-4026-4b46-8b5a-7efd7b7badf7)

![DNS 7](https://github.com/Jacob-Oq/intuition-dns/assets/150084528/732be9ca-20ec-4f1a-9ebc-5d3b9bce3ab7)

<p>Return to the client VM. Ping "mainframe" again to see it resolve.</p>

![DNS 8](https://github.com/Jacob-Oq/intuition-dns/assets/150084528/8109635c-0fd3-4ccd-b7b3-2ee9d1340b47)

![DNS 9](https://github.com/Jacob-Oq/intuition-dns/assets/150084528/67426372-f661-4a5a-8dab-b7bcb8918ff7)

<br />
<p>On the domain controller change the mainframe's record address to 8.8.8.8 and refresh the DNS server.</p> 

![DNS 7](https://github.com/Jacob-Oq/intuition-dns/assets/150084528/031a673d-8e4e-426e-881c-512c470898cc)

![DNS 12](https://github.com/Jacob-Oq/intuition-dns/assets/150084528/35e38907-2b5f-41a9-874a-e74079af982b)

![DNS 13](https://github.com/Jacob-Oq/intuition-dns/assets/150084528/7eacbb5e-40c1-4fa8-b95f-209f7e17b977)

![DNS 14-1](https://github.com/Jacob-Oq/intuition-dns/assets/150084528/0d555000-b820-43ba-9045-7ebfcf0d47f7)

<p>Then ping "mainframe" on the client, it will still ping the old IP address.</p>

![DNS 15](https://github.com/Jacob-Oq/intuition-dns/assets/150084528/8d3bbebc-a70b-4d19-98fb-1a38ee33719b)

<p>When the command ipconfig /displaydns is run, it will reveal that the DNS cache still has the old IP.</p>

![DNS 16](https://github.com/Jacob-Oq/intuition-dns/assets/150084528/b3f0c9d6-3450-4779-92c0-87bf2daaee76)

In order to update the cache, we need to clear it. The command ipconfig /flushdns will empty the cache so that when we ping mainframe again, the IP address will be updated to the new one on the client side. When pinging mainframe, the new IP address of the record should show.
</p>

![DNS 18](https://github.com/Jacob-Oq/intuition-dns/assets/150084528/552816b7-a8e6-44c9-96c9-6d27dce29ec5)

![DNS 19](https://github.com/Jacob-Oq/intuition-dns/assets/150084528/e5286f97-5348-4786-85fc-8f261fedda4d)

![DNS 20](https://github.com/Jacob-Oq/intuition-dns/assets/150084528/4cdc647e-d36b-40aa-98b0-6b5eff962840)

![DNS 21](https://github.com/Jacob-Oq/intuition-dns/assets/150084528/6ee65708-c297-457c-99bd-04e90d7220d1)

<br />
<p>
A CNAME record will now be made on the DNS server that will point "search" to Google. On the Forward Lookup Zones tab in the DNS Manager, open the tab that has the domain. Create a new CNAME record called search and point it to Google. Refresh the server to save the changes. On the client, pinging search and using nslookup will return the results of the CNAME record.
</p>
<br />

<h2>Lessons Learned </h2>

This lab made me understand how DNS directly works on a computer and the network. DNS records can change and sometimes this can cause connectivity issues. The DNS cache may have the old records and needs to be cleared out to update to the new records. The ipconfig /flushdns command is a common troubleshooting tool that has been referenced a lot in IT programs and I did not get a complete understanding how and why this works until I have done this lab. In the context of pinging "mainframe" at the start of the lab, the DNS cache gets checked first. If there is no result, the host file will be checked. The DNS server will be checked if there are no results in the host file. I made a record on the DNS server so a ping to mainframe can resolve. A CNAME record maps an alias name to another domain name. In this case, "search" was another name for Google.
