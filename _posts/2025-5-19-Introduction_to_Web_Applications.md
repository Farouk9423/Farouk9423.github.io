---

title: "Introduction to Web Applications — Arabic HackThe Box ACADEMY Walkthrough"
date: 2025-05-19
authors: "Farouk"
layout: post
categories: Hack_The_Box_Academy
tags: [Bug_Bounty_Hunter, PenetrationTesting, Security, Hacker]
direction: rtl
---

<script>
document.addEventListener("DOMContentLoaded", function () {
    // البحث عن العنصر الذي يحتوي على "By"
    let authorElement = document.querySelector("span em");

    // التأكد أن العنصر موجود
    if (authorElement) {
        authorElement.textContent = "Farouk"; // وضع الاسم داخل الوسم <em>
    }
});
</script>

<style>
    h1, h2, h3, h4, h5, h6, p, li {
        font-family: "Tajawal", "Amiri", "Cairo", sans-serif;
    }

    .note {
        background-color: #dff9fb;
        border-right: 5px solid #22a6b3;
        padding: 10px;
        margin: 20px 0;
        border-radius: 5px;
    }

    .warning {
        background-color: #f9e79f;
        border-right: 5px solid #f39c12;
        padding: 10px;
        margin: 20px 0;
        border-radius: 5px;
    }

    .code-title {
        background-color: #2c3e50;
        color: #ecf0f1;
        font-weight: bold;
        padding: 5px 10px;
        border-top-left-radius: 5px;
        border-top-right-radius: 5px;
    }

    .image-box {
        text-align: center;
        margin: 20px 0;
    }

    .image-box img {
        max-width: 80%;
        border: 2px solid #ccc;
        border-radius: 10px;
    }
      .htblogo {
        text-align: center;
        margin: 20px 0;
    }

      .htblogo img {
        width: 100%;
        max-width: 100%;
        border: 2px solid #ccc;
        border-radius: 10px;
    }

  .note {
    background-color: #1e1e1e; /* لون خلفية غامق مريح */
    color: #c9d1d9; /* لون نص فاتح */
    border-left: 4px solid #2ea043; /* شريط أخضر مثل GitHub */
    padding: 12px 16px;
    margin: 20px 0;
    border-radius: 6px;
    font-size: 16px;
    line-height: 1.6;
    box-shadow: 0 2px 5px rgba(0,0,0,0.2);
  }
    .terminal-box {
  background-color:rgb(37, 37, 37); /* نفس الخلفية يلي عندك */
  color: #ccc;
  padding: 1rem;
  border-radius: 6px;
  font-family: monospace;
  font-size: 0.95rem;
  overflow-x: auto;
  border: 1px solid #222;
}

/* ألوان النصوص فقط */
.user   { color: #00afff; }   /* أزرق المستخدم */
.path   { color: #ffaa00; }   /* مسار */
.symbol { color: #00ff00; }   /* $ */
.cmd    { color: #7CFC00; }   /* أمر MySQL */
.input  { color: #ffff66; }   /* إدخال */
.dim    { color: #777; font-style: italic; }  /* رمادي باهت */
.prompt { color: #00ffff; }   /* mysql> */

</style>

<div class="htblogo">
  <img src="{{ '/images/HTB/Untitled.png' | relative_url }}" alt="HTB">
</div>

- [HTB الرابط على](https://academy.hackthebox.com/module/75/section/719)

<h2 id="📌 Introduction:" dir=""><span class="me-2"><strong>📌 Introduction:</strong></span><a href="#📌 Introduction:" class="anchor text-muted"></a></h2>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
  ال Web Application هي عبارة عن برامج بتشتغل جوا المتصفحات متل Firefox,Chrome وبتعتمد على شي اسمو  client-server architecture.<br>
  <span style="font-family: 'Amiri'; color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">العميل(Client):</span> هو المتصفح يلي المستخدم بشوف فيه واجهة التطبيق (ال Front-end) متل التصميم ولون كل زر وهي الأمور كلها...<br>
  <span style="font-family: 'Amiri'; color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">الخادم (Server):</span> هو جزء ال Back-end، موجود فيه الكود تبع التطبيق وقواعد البيانات.<br>
  الترتيب هاد بخلي الشركات بتقدر تستضيف تطبيقات قوية تتحكم فيها بشكل مباشر وتكون متاحة للناس بكل العالم، متل خدمات البريد من Google، المتاجر الالكترونية متل Amazon، برامج تحرير النصوص متل Google docs<br>
  وهالشي مو مقتصر عالشركات الكبيرة بس لانو اي Web Developer بيقدر يعمل Web Application ويستضيفه علة خدمات الاستضافة العادية، وهاد السبب انو في ملايين الWeb Applications ومليارات المستخدمين.
</p>

<div class="htblogo">
  <img src="{{ '/images/HTB/IntroToWebApplications/1.jpg' | relative_url }}" alt="HTB">
</div>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">




<!-- Google tag (gtag.js) -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-KKMCPL6FXY"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'G-KKMCPL6FXY');
</script>
