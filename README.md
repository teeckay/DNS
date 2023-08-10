<p align="center">
  
![image](https://github.com/teeckay/DNS/assets/64244011/d0cac925-32cd-46c6-8111-5275c25bada8)

</p>

<h1> Domain Naming System ( DNS ): How It Works. </h1>

This tutorial outlines the implementation DNS on Active Directory<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines)
- Remote Desktop
- Active Directory Domain Services
- Command Line and Command Line Tools
  
<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>Exercises Included</h2>

- A - Record Exercise
- Local DNS Cache Exercise
- CNAME Record Exercise

<p>
<h2>DNS Exercise Illustration Steps</h2>

<p>
  
To illustrate DNS for this lab, I will log into a Domain Controller ( Active Directory)  installed in a virtual machine named DC-1. I will also log in with a user in the domain from a separate client virtual machine called Client-1.

</p>

 <h3> A-Record Exercise </h3>

</p>

<p>
  
To illustrate how an A - record ( DNS record ) works, I begin by pinging a non-existent machine called “dnsexercise” from a client machine to a domain controller. I expect this to fail as “dnsexercise” is not in the local cache, host file or DNS records in the Domain Controller.

</p>
<br />

<p>

![image](https://github.com/teeckay/DNS/assets/64244011/c1d6ba84-cecf-4aea-9b71-7391593b0028)


</p>


<p>
  
Next I will create an A - record for “dnsexercise”. I do this by accessing DNS settings on the Domain Controller through the Server Manager. I then create a New Host (A or AAAA) called “dnsexercise” and set this new host’s IP address to the client computer’s address for a successful ping.

</p>
<br />

<p>
  
![image](https://github.com/teeckay/DNS/assets/64244011/dd20e312-f3dd-4009-8d60-c164edc2f352)

![image](https://github.com/teeckay/DNS/assets/64244011/9abf60c9-b167-43cb-b1eb-28eb0c4d20f4)

</p>


<p>
  
Next, I ping the newly created dns record “dnsexercise” and it resolves to the assigned domain ( tiudydomain.com ) in the A - record and the IP address 10.0.0.5.
</p>
<br />

<p>
  
![image](https://github.com/teeckay/DNS/assets/64244011/f60d2839-754a-4723-baa7-ea9ced9d7558)

</p>

<p>
  
Next, I use nslookup for dnsexercise and it shows that the Domain Controller (DC-1) server returned dnsexercise.tiudydomain.com
</p>
<br />

<p>
  
![image](https://github.com/teeckay/DNS/assets/64244011/a1c89c95-47a6-4d87-a7da-065de2998f44)

</p>

<h3> Local DNS Cache Exercise </h3>

<p>
  
Next, I will do a test on the local DNS cache. To display this I use ipconfig /displaydns and records such as the domain controller (DC-1) and its mapped IP address and dnsexercise.tiudydomain.com and its mapped IP address are showing. This allows the client machine to use these records in the local cache first instead of using the DNS server in the Domain Controller, making it faster
</p>
<br />

<p>
  
![image](https://github.com/teeckay/DNS/assets/64244011/54bb890a-ba86-4f28-b4dd-f259b736393f)

</p>

<p>
  
To illustrate why you should refresh dns records and cache, i will change the A - record IP address for dnsexercise to 8.8.8.8 from 10.0.0.5 and ping dnsexercise again.

</p>
<br />

<p>
  
![image](https://github.com/teeckay/DNS/assets/64244011/eba98b83-302e-4448-b542-1bb7651f608f)

</p>

<p>
  
After pinging dnsexercise, the reply comes from the old IP address of 10.0.0.5 instead of the new address of 8.8.8.8 as this record is still in the local DNS Cache as seen below.
</p>
<br />

<p>
  
![image](https://github.com/teeckay/DNS/assets/64244011/1ab77a2b-3084-4fe2-9a32-6e8347e8dd09)

![image](https://github.com/teeckay/DNS/assets/64244011/7429a9be-b68c-4061-8ce7-cf221d53e12b)

</p>

<p>
  
This shows the importance of flushing the DNS records in this instance. This is done with the command ipconfig /flushdns. This will clear out the dns cache and it will then be repopulated by querying the DNS Server again for new records. Next, I flush the DNS cache with administrator privileges and display it to show that it is cleared.

</p>
<br />

<p>
  
![image](https://github.com/teeckay/DNS/assets/64244011/08a8871c-0995-485b-bb68-fed3c2fa8324)

![image](https://github.com/teeckay/DNS/assets/64244011/64ac8971-7181-44b9-9b2d-03b095b1b2f2)


</p>

<p>
  
Next I will ping dnsexercise again and since the cache has been cleared, The client machine will ask the DNS Server (DC-1) for new records and the updated IP Address of 8.8.8.8 will show. 
</p>
<br />

<p>
  
![image](https://github.com/teeckay/DNS/assets/64244011/46e4ed8e-d14a-4b28-a5b0-d7056376f314)

</p>

<p>
  
This new record is also now stored in the cache.

</p>
<br />

<p>
  
![image](https://github.com/teeckay/DNS/assets/64244011/3ba1aba2-44ea-4702-b06e-e0fca3252dbb)

</p>

<h3> CNAME Record Exercise </h3>

<p>
  
 I will create a CNAME record that points the host “searchengine” to “www.google.com”. I will start by pinging “search engine” and it shows the host could not be found. 

</p>
<br />

<p>
  
![image](https://github.com/teeckay/DNS/assets/64244011/80ad0de1-071f-48da-b95f-298b45307227)

</p>

<p>
  
Next i will create a CNAME record on the Domain Controller that targets www.google.com 

</p>
<br />

<p>
  
![image](https://github.com/teeckay/DNS/assets/64244011/fb7c03f3-b13a-4674-903b-193930f64024)

![image](https://github.com/teeckay/DNS/assets/64244011/c94c5a77-d113-46e8-9161-1ea121c84fec)


</p>

<p>
  
I then ping the searchengine CNAME again and this time it pings www.google.com and gets replies.
</p>
<br />

<p>
  
![image](https://github.com/teeckay/DNS/assets/64244011/ea234e4b-8e75-40e5-b49e-4e8e5c9f1779)

</p>

<p>
  
This is also shown in the DNS records below.

</p>
<br />

<p>
  
![image](https://github.com/teeckay/DNS/assets/64244011/3590d59b-d4ee-4395-a1bc-eed56d5501a2)

</p>

<p>
  

</p>
<br />

