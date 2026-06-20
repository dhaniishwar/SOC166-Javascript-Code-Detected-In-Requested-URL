**Platform:** LatsDefend

**Lab Name:** SOC166-Javascript code Detected in Requested URL

**Difficulty:** Easy

**Date Completed:** 2026-06-19

**Type:** Web Attack

**Target:** WebServer1002(172.16.17.17)

---
<h3 align =center> Goal </h3>

&nbsp;&nbsp;&nbsp;&nbsp; Investigate a suspected XSS attack against an internal web server, determine whether the attack was successful, contain the host if needed, and close the case.

---
<h3 align =center> Walkthrough </h3>

***Alert Review:***

<br>
&nbsp;&nbsp;&nbsp;&nbsp; Opening the alert in the Monitoring tab under the Investigation Channel reveals all the critical details. 
<br>
<br>

<img width="1037" height="326" alt="1" src="https://github.com/user-attachments/assets/6200ae65-6cfd-4aa3-b061-a9cfa3e6ae59" />

<br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp; There is two important things to note from alert. First, the Source IP 112.85.42.13 is an external address — this traffic is coming from outside the company network. Second, Device Action is Allowed, meaning the request went through. That mean whatever was in that URL actually reached the server. Now, let's open the playbook.
<br>

---
***Starting the Playbook:***

<img width="596" height="344" alt="2" src="https://github.com/user-attachments/assets/b2d9ea52-3a1f-46b9-9b75-d8419ad4265f" />

<br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp; The rule triggered because JavaScript code was detected inside the requested URL. Specifically, look at the URL:
<br>
<br>

```bash
https://172.16.17.17/search/?q=<$script>javascript:$alert(1)<$/script>
```

<br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp; What we're seeing here is a classic Cross-Site Scripting (XSS) payload. The attacker use "alert(1)" comment is use to test whether a site is vulnerable or not.
<br>
<br>

<img width="592" height="355" alt="3" src="https://github.com/user-attachments/assets/0fe846bd-51fe-4641-994d-13b9b83bf2ec" />

<br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp; The paybook now asks us to gather context anbout the IP addresses. Let's check the reputation with VirusTotal.
<br>
<br>

<img width="1260" height="696" alt="4" src="https://github.com/user-attachments/assets/95aeefcc-2075-4633-ba5b-f8757b418ada" />

<br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp; This IP has a bad reputation in the security community even if only one vendor flagged it in their detection engine. this IP has a bad reputation in the security community even if only one vendor flagged it in their detection engine.
<br>
<br>

---
***Log Investigation:***

<img width="583" height="412" alt="5" src="https://github.com/user-attachments/assets/189c86a8-1865-4413-9a82-04e8fc9c6ea2" />

<br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp; Now, let's check all traffic from 112.85.42.13 to 172.16.17.17 from Log Management.
<br>
<br>

<img width="835" height="285" alt="6" src="https://github.com/user-attachments/assets/6187c41e-635e-41b0-88c7-bd8d4bd57078" />

<img width="842" height="283" alt="7" src="https://github.com/user-attachments/assets/2546a757-4ca2-4e49-8950-62acec86e402" />

<img width="831" height="285" alt="8" src="https://github.com/user-attachments/assets/907f82fa-db3e-4e1d-a97d-11fea8889116" />

<img width="851" height="303" alt="9" src="https://github.com/user-attachments/assets/8a27146e-cfd8-42c3-ae13-ff4f56ef0d68" />

<img width="847" height="284" alt="10" src="https://github.com/user-attachments/assets/03c3aa63-c338-4720-ad6a-6dc7988fe8c1" />

<img width="836" height="279" alt="11" src="https://github.com/user-attachments/assets/b05a3243-a6c9-40bb-9907-4c03b52cfa6b" />

<img width="850" height="287" alt="12" src="https://github.com/user-attachments/assets/b1a465b2-8406-4c59-a124-480b727d4774" />

<br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp; That 302 with zero response body is the key detail. The server redirected instead of rendering, which means the payloads were never reflected back. Compare that to the benign ?q=test which returned a 200 with actual content — the contrast confirms the application handled the malicious input differently and didn't execute any of it.
<br>
<br>

---
***Back to PlayBook:***

<img width="603" height="289" alt="13" src="https://github.com/user-attachments/assets/9c9f73f9-40a4-444b-8e1f-a5d261d5906a" />

<br>
&nbsp;&nbsp;&nbsp;&nbsp; Yes, the URL is malicious.
<br> 
<br>

<img width="592" height="239" alt="14" src="https://github.com/user-attachments/assets/d48a99e9-9c3d-4477-8266-25e83d017cbe" />

<br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp; Everything in these logs, The URL contains script tag at the end. This attack is called Cross-Site Scripting (XSS).
<br>
<br>

<img width="588" height="363" alt="15" src="https://github.com/user-attachments/assets/6d9aaafb-f6d9-431b-8de9-0be9e835e249" />

<br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp; No emaiils, no maintenance windows, no recode of a scheduled pentest involving WebServer1002 or the source IP. This was not a planned test.
<br>
<br>

---
***Endpoint Investigation:***

<img width="604" height="268" alt="16" src="https://github.com/user-attachments/assets/c8509807-a7d4-4c9f-8796-ec369135a546" />

<br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp; let's check with the Endpoint security.
<br>
<br>

<img width="1029" height="554" alt="17" src="https://github.com/user-attachments/assets/4943766b-ad45-410a-8fe3-4d1396be8828" />

<br>
<br>
&nbsp;&nbsp;&nbsp;&nbsp; The soure IP has no matches. That confirms this is a purely external attacker. So, the answer is Internet -> Company Network.
<br>
<br>

<img width="599" height="431" alt="18" src="https://github.com/user-attachments/assets/66ae3660-8694-478b-9c13-b7270f810791" />






