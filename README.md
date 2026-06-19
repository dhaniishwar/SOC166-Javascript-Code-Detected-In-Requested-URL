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





