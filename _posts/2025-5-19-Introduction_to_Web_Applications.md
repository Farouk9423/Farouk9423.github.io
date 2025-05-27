---

title: "Introduction to Web Applications — Arabic HackThe Box ACADEMY Walkthrough"
date: 2025-05-19
author: <author_id>
layout: post
categories: Hack_The_Box_Academy
tags: [Bug_Bounty_Hunter, PenetrationTesting, Security, Hacker]
description: بهالدرس رح نحكي عن تطبيقات الويب وكيف بتشتغل ورح نشوفها من وجهة نظر امن المعلومات، لهيك جهز قهوتك ويلا نبلش
image:
  path: /images/HTB/IntroToWebApplications/0.webp
  alt: 
---

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
<a href="https://academy.hackthebox.com/module/details/75" target="_blank" rel="noopener noreferrer" style="font-size:20px">HTB Academy الدرس على</a>

---


<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
 مرحبا يا هكرجية👨🏻‍💻، انا فاروق واليوم رح نحكي مقدمة عن تطبيقات الويب ورح نشرح كلشي بال Module من HTB.<br>
 الفكرة انو هالشي عم ياخد وقت وجهد كبير بس انا ماشي على مبدئ زكاة العلم نشره وعسى يكون صدقة جارية لأهلي<br>
 لهيك بس يلي رح اطلبه منك تتذكرنا بدعوة من قلبك وكمان تنشر العلم مع اي حدا مهتم بالمجال وبالتوفيق يارب❤️.<br>
 <strong>إن اخطأت فمن نفسي وإن اصبت فمن الله✨</strong>
</p>

---
<h2 id="Introduction to Web Applications" dir=""><span class="me-2"><strong>Introduction to Web Applications</strong></span><a href="#Introduction to Web Applications" class="anchor text-muted"></a></h2>

<h2 id="📌 Introduction:" dir=""><span class="me-2"><strong>📌 Introduction:</strong></span><a href="#📌 Introduction:" class="anchor text-muted"></a></h2>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
  ال Web Application هي عبارة عن برامج بتشتغل داخل المتصفحات متل Firefox,Chrome وبتعتمد على شي اسمو  client-server architecture.<br><br>
  <span style=" color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">العميل (Client):</span> هو المتصفح يلي المستخدم بشوف فيه واجهة التطبيق (Front-end) متل التصميم ولون الأزرار وشكل الصفحة...<br><br>
  <span style=" color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">الخادم (Server):</span> هو جزء ال Back-end، موجود فيه الكود تبع التطبيق وقواعد البيانات.<br>
  الترتيب هاد بخلي الشركات بتقدر تستضيف تطبيقات قوية تتحكم فيها بشكل مباشر وتكون متاحة للناس بكل العالم، متل خدمات البريد من Google، المتاجر الالكترونية متل Amazon، برامج تحرير النصوص متل Google docs<br>
  وهالشي مو مقتصر عالشركات الكبيرة فقط لانو اي Web Developer بيقدر يعمل Web Application ويستضيفه عبر خدمات الاستضافة العادية، وهاد السبب انو في ملايين الWeb Applications ومليارات المستخدمين.
</p>

<div class="htblogo">
  <img src="{{ '/images/HTB/IntroToWebApplications/1.jpg' | relative_url }}" alt="HTB">
</div>


---
<h3 id="🔵 Web Applications vs. Websites" dir=""><span class="me-2" style="color:white"><strong>🔵 Web Applications vs. Websites</strong></span><a href="#🔵 Web Applications vs. Websites" class="anchor text-muted"></a></h3>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
سابقاً، كانت المواقع مملة وجامدة (static)، يعني محتواها ثابت وحتى يتغير لازم المبرمج يدخل عالكود ويضيف عليه يدويا.<br>
يعني مثلا عنا موقع بيعرض معلومات شركة، فهي المعلومات رح تضل ثابتة ومابتتغير اذا حاول المستخدم يتفاعل معها، هي المواقع منسميها Web 1.0، وهي متل ماحكينا بتعرض المعلومات كأنك عم تقرأ جريدة ومابتقدر تغير شي حتى تغيره المطبعة.<br>
</p>

<div class="htblogo">
  <img src="{{ '/images/HTB/IntroToWebApplications/2.png' | relative_url }}" alt="HTB">
</div>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
اما اليوم معظم تطبيقات الويب بتستعمل شي اسمو Web 2.0 وهي التطبيقات بتعرض محتوى متغير (Dynamic) وبيتغير حسب تفاعل المستخدمين معه.<br>
يعني مثلا مثلا لما نفتح اليوتيوب ونضغط على اي زر فالمحتوى قدامي بيتغير حسب الزر يلي كبسته.<br>
وكمان من مميزاتها انها بتشتغل على كل انواع الشاشات يعني مابهم اذا بفتح التطبيق من موبايل او لابتوب او PC كلو شغال وكمان بتشتغل على اي نظام تشغيل OS.
</p>
---
<h3 id="🔵 Web Applications vs. Native Operating System Applications" dir=""><span class="me-2" style="color:white"><strong>🔵 Web Applications vs. Native Operating System Applications</strong></span><a href="#🔵 Web Applications vs. Native Operating System Applications" class="anchor text-muted"></a></h3>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
<span style="color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">Native OS Applications</span>
هي البرامج يلي بتتثبت على جهازك، متل برامج مايكروسوفت او الألعاب يلي بتشتغل عالويندوز أو الماك. اما ال
<span style="color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">Web Applications</span>
فهي مختلفة شوي وعندها اكتر من ميزة.<br><br>
ال Web Application مستقلة عن نظام التشغيل، يعني بتشتغل على اي نظام تشغيل بغض النظر عن نوعه (ويندوز,ماك او حتى لينكس)<br>
وكمان مافي داعي ثبت ال Web Application على جهازي، لأنها بتشتغل على Server بعيد، لهيك مافي داعي خزن مساحة على الهارد تبع المستخدم.<br><br>
وبالإضافة لهالأمور بال Web Application الإصدار موحد للكل، يعني كل المستخدمين عندهم نفس نسخة الإصدار للتطبيق، وبتم تحديثه على ال Server بدون الحاجة للإرسال تحديثات للمستخدم. وهالشي بقلل تكاليف الصيانة والدعم.<br><br>
بس كمان ال Native OS Applications في الها إيجابيات متل انها اسرع لأنها بتستخدم ملفات ومكتبات موجودة عالنظام والأجهزة المحلية، وكمان عندها قدرات افضل لانها بتستخدم موارد الجهاز بشكل اعمق بكتير مقارنة بتطبيقات الويب يلي بتعتمد على امكانيات المتصفح.<br><br>
آخر كم سنة طلع عندي شي اسمه
<span style="color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">Hybrid and Progressive Web Apps</span>
وهاد عبارة عن دمج لمميزات التنين فهي بتستخدم أطر عمل حديثة وبتعمل بسرعة اكبر وبتستفيد من موارد الجهاز.
</p>


<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
في لل Web Applications نوعين رئيسيين:<br>
<span style="color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">مفتوحة المصدر (Open Source)</span>:
اي شخص ممكن يدخل عالكود تبعها ويعدل عليها لتلبي احتياجه، وهي يلي بتفضله الشركات لانها بتقدر تعدل عليها حسب شو بناسبها، امثلة عنها:
</p>
- WordPress: لإنشاء المواقع والمدونات
- OpenCart: لإنشاء المتاجر الإلكترونية
- Joomla: لإدارة المحتوى
<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
<span style="color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">مغلقة المصدر (Closed Source)</span>:
وهي يلي بتطورها الشركات وبتبيعها، او بتسمح باستخدامها بإشتراك، متل:
</p>
- Wix: لإنشاء المواقع
- Shopify: لإنشاء متاجر إلكترونية
- DotNetNuke: لإدارة المحتوى

---

<h3 id="🔵 Security Risks of Web Applications" dir=""><span class="me-2" style="color:white"><strong>🔵 Security Risks of Web Applications</strong></span><a href="#🔵 Security Risks of Web Applications" class="anchor text-muted"></a></h3>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
بما انو ال Web Applications ممكن الوصول اليها من اي مكان بالعالم بمجرد مايكون عندك اتصال بالانترنت فهالشي سمح لأي شخص لأي شخص انو يقدر يهجم عليها، وكل ما كان ال Web Application اكبر ومعقد اكتر كل ما زاد احتمال انو يكون عندي فيها مشاكل امنية، وخاصة انو عندي أدوات تلقائية لمسح وفحص ومهاجمة ال Web Applications، ولما الهاكر يستخدم هالأدوات رح يسببلي مشاكل ومخاطر كبيرة.<br><br>
اذا فرضا الهاكر قدر يخترق ال Web Application بسبب خسائر مالية وبتتوقف الأعمال وممكن يصير تسريب لبيانات حساسة متل معلومات العملاء والشركات، لأنه غالبا ال Web Applications بترتبط ب Servers وبقواعد بيانات بتحتوي على هدول المعلومات.<br><br>
لهيك ضروري عالشركات انو تختبر تطبيقاتها مشان تكتشف الثغرات وتحاول إصلاحها بسرعة قبل ما الهكر يكتشفها🌚، ولازم تعتمد على مبدئ ال Secure Coding، واكيد لازم تتأكد انو اصلاح الثغرات ما يفتح ثغرات جديدة(يعني بالله شو استفدنا لم نصلح ثغرة بس نفتح وحدة غيرها😂🤦‍♂️)<br><br>
بالنسبة لاخبار اختراق ال Web Applications، اسمها Web Application Penetration Testing، وهي مهارة لازم الشخص يلي بدو يشتغل بالمجال يكون بيعرف كيف بيشتغل ال Web Application بالأصل لأنو مافيك تخترق شي مابتعرف كيف بيشتغل🌚، ولازم نتعلم كيف بتم تطوير ال Web Applications وشو هي المخاطر الخاصة بكل طبقة وبكل مكون من مكونات التطبيق حسب شو التقنيات المستخدمة.<br><br>
وحدة من الطرق المستخدمة كتير هي استخدام دليل OWASP، وهو مرجع عالمي بال Web Penetration Testing.<br><br>
وحدة من الإجراءات المشهورة هي انو نبلش بالتأكد من امان ال Front end ونراجع كود ال HTML, CSS, Java Script وهنن اللغات المشهورة يلي بتم من خلالهم بناء ال Web Applications، فمنتأكد انو الكود مكتوب بطريقة Secure ومنحاول ندور على نقاط ضعف متل Sensitive Data Exposure وهي مهاجمة البيانات الحساسة لل Web Application وبسبب اختراق لكل البيانات يلي لازم تنحمى، وال  Cross-Site Scripting (XSS) بتسبب انو المهاجم بيقدر يوصل برامج خبيثة لل Web Application أو سرقة ال Sessions.<br><br>
ولايهمك اذا مافهمت كتير، رح نحكي عنهن بالتفصيل لقدام لساتنا بالمقدمة حاليا🫠<br><br>
بعد ما نراجع كل المكونات الاساسية بال Front end منرجع ومنتأكد من باقي الوظائف الخاصة بال Web Applicatoin واتصاله بالسيرفرات وبدور عالمشاكل او الثغرات بالسيرفر.<br>
وعادةً منقيم ال Web Application كشخص خارجي وكشخص داخلي، يعني مثلا عندي موقع بيتطلب تسجيل دخول، بشوف النتائج مرة بدون ما يكون عندي حساب ومرة وانا عندي حساب مشان اعمل مراجعة لكل السيناريوهات المحتملة عندي.
</p>
---
<h3 id="🔵 Attacking Web Applications" dir=""><span class="me-2" style="color:white"><strong>🔵 Attacking Web Applications</strong></span><a href="#🔵 Attacking Web Applications" class="anchor text-muted"></a></h3>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
معظم الشركات بغض النظر عن حجمها عندها Web Applicatoin خاص فيها، بدايةً من الشغلات البسيطة متل ال Static وصولا لمدونات كبيرة بتشتغل بأنظمة إدارة المحتوى متل WordPress, وصولا لانظمة كبيرة فيها تسجيل دخول وادوار للمستخدمين (متل مستخدم عادي وادمن و...)<br>
وبسبب طبيعتها ال Dynamic بتتغير هالتطبيقات باستمرار، واي تغيير بسيط ممكن يودي لظهور ثغرات خطيرة.<br><br>
موجود العديد من الثغرات الخطيرة يلي ممكن تطلع عندي، وحدة منهن هي ال
<span style=" color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">SQL Injection</span>
بتصير لما حدا يتعامل مع طلبات المستخدم بشكل غير آمن، وهالشي بيسمح للمهاجم بالوصول لقاعدة البيانات وقراءة الملفات او حتى تنفيذ اوامر على السيرفر.
</p>
<div class="note" dir="rtl" style=" font-size: 20px;">
    <span style=" color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">✅ ملاحظة: </span>
حكينا عن ال SQL Injection بدرس تاني بالتفصيل بتقدر ترجع تشوفها وهي اللينك تبعها 
<a href="https://farouk9423.github.io/posts/SQLfundemantle/" target="_blank" rel="noopener noreferrer" style="font-size:20px">SQL Injectoin</a>
</div>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
<br>
الثغرة يلي بعدها هي
<span style=" color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">File Inclusion</span>
وهي بتسمح للمهاجم بقراءة Source Code (الكود تبع التطبيق) حتى اوصل لصفحات او لملفات مخفية وهيك بقدر اكشف وظائف مخفية مو مصرح الي اني اوصلها وممكن تؤدي لتنفيذ أوامر عن بعد.<br><br>
بعدين بيجي عنا ال <span style=" color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">Unrestricted File Upload</span>
وهي لما بيسمح التطبيق برفع الملفات بدون تحقق (مثل لما يكون التطبيق بيسمح الي اني ارفع صورة فبرفع برنامج ضار بدلاً من صورة)، وهيك ممكن للمهاجم السيطرة على ال server.<br><br>
وبعدين بيجي عنا ال
<span style=" color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">Insecure Direct Object Referencing (IDOR)</span>
وهي لما بيقدر المستخدم انو يوصل لبيانات أو وظائف بتخص مستخدمين آخرين.<br> على سبيل المثال، إذا كان في رابط لتعديل ملفك الشخصي مثل<br> /user/701/edit-profile، يمكن للمهاجم تغيير الرقم إلى 702 لتعديل ملف مستخدم آخر.<br><br>
آخر الشي عنا ال
<span style=" color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">Broken Access Control</span>
النظام عندي هو المسؤول عن تحديد الصلاحيات للمستخدمين، بس لما يصير عندي خطأ بالتنفيذ المهاجم ممكن يستغل هالشي ويوصل لمكان مو مسمحله يوصله.<br><br>
مثلاً عندي موقع بيسمح لأي شخص يعمل حساب جديد وبيطلب منه اسم المستخدم وكلمة السر والإيميل بس المشكلة انو بيبعت كمان rolied وهاد رقم بمثل صلاحية الحساب يعني مثلا rolied=3 يعني الحساب لمستخدم عادي، اما اذا كانت rolied=1 فالمستخدم ادمن، واذا كانت rolied=0 بكون صاحب النظام وهاد اعلى من الأدمن.<br>
واي حدا بدو يعمل حساب جديد الموقع بيرسل عالسيرفر هاد الطلب
</p>
```
POST /register
username=bjones&password=Welcome1&email=bjones@domain.com&roleid=3
```
<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
فهون المهاجم صار يفكر ليش ما غير ال rolied=3 ل rolied=1?<br>
وصار عندي الطلب هيك:
</p>
```
POST /register
username=hacker&password=123456&email=hacker@evil.com&roleid=1
```

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
والمفاجأة؟ 😏<br>
إذا ما كان في تحقق منيح بالسيرفر، الموقع رح يسجل الحساب كـ ادمن مباشرة!!<br>
الموقع عطاك صلاحيات مو من حقك بس لأنك غشيت برقم صغير، وهالشي بسبب إنو مافي تحقق حقيقي من السيرفر على صلاحية المستخدم.
</P>
---

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
<span style=" color:rgb(221, 39, 78); font-size: 27px; line-height: 1.6;"><strong>مثال واقعي:</strong></span><br>
اذا كان عندي تطبيق ويب بيستخدم شي اسمه Active Directory لتسجيل الدخول للمستخدمين، ممكن لثغرة ال SQL Injection انو تسمح للهاكر باستخراج ايميلات المستخدمين وبقدر استخدم هالايميلات بهجوم Password Spraying (بحاول بكلمات السر المشهورة على كتير حسابات) ضد بوابات ال VPN او حتى الايميلات وهالشي بيسمحلي اوصل للبيانات الحساسة او حتى اني اخترق الشبكة الداخلية للشركة.
</p>
---
<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
لا تخاف اذا حسيت اي شي من الحكي يلي حكيناه صعب، لانو رح نحكي عليهن لقدام بالتفصيل إن كان بهاد الدرس او بالدروس القادمة، ورح تصير تلاقيهن شوربة (شوربة = بغاية السهولة😂)<br>
<strong>لهيك يلا يا بطل خلينا نكمل وما نوقف هون وتذكر انو لازم تحس حالك غبي انت وعم تدرس حتى تصير عبقري🔥...</strong>
</p>
---

<h2 id="📌 Web Application Layout:" dir=""><span class="me-2"><strong>📌 Web Application Layout:</strong></span><a href="#📌 Web Application Layout:" class="anchor text-muted"></a></h2>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
  كل Web Application موجود مختلف عن التاني، لأن كل شركة بتبني التطبيق الخاص فيها بناءً على احتياجاتها والجمهور يلي بتستهدفه، لهالسبب تطبيقات الويب بتختلف بطريقة برمجتها وتصميمها والبنية التحتية متل السيرفرات والأنظمة يلي بتدعمها.<br><br>
  ولنفهم تطبيق الويب لازم نعرف 3 اشياء رئيسية:<br>
  1- <span style=" color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">البنية التحتية (Infrastructure)</span>:
  يعني الأجهزة والسيرفرات يلي بتشغل التطبيق، متل قاعدة البيانات (Database) أو السيرفر المستضيف لل Web Application.<br><br>
  2- <span style=" color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">مكونات التطبيق (Components)</span>:
  وبتتألف من الأجزاء يلي بيتكون منها التطبيق، متل واجهة المستخدم(Frontend) والمكونات يلي بيتفاعل معها.<br><br>
  3- <span style=" color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">هندسة التطبيق (Architecture)</span>:
  عبارة عن العلاقة بين كل هالمكونات مع بعضها وكيف بتتفاعل مع بعض.
</p>
---
<h3 id="🟢 Web Application Infrastructure" dir=""><span class="me-2" style="color:white"><strong>🟢 Web Application Infrastructure</strong></span><a href="#🟢 Web Application Infrastructure" class="anchor text-muted"></a></h3>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
  البنية التحتية هي متل حجر الأساس يلي بقوم عليه ال Web Application، متل ماحكينا تطبيقات الويب بحاجة سيرفرات وقواعد بيانات عشان تشتغل، وفي أربع طرق رئيسية لبناء هالعملية:<br><br>
</p>

- Client-Server
- One Server
- Many Servers - One Database
- Many Servers - Many Databases

---

<p>
<strong style="color:white; font-size:20px">Client-Server</strong>
</p>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
  هاد اشهر Model، واغلب تطبيقات الويب بتستخدمه
</p>

<div class="htblogo">
  <img src="{{ '/images/HTB/IntroToWebApplications/3.jpg' | relative_url }}" alt="HTB">
</div>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
  الفكرة من هال Model انو بكون عندي Server (جهاز كمبيوتر قوي)، والمستخدمين بيدخلو لهالسيرفر من خلال متصفح الانترنت على اجهزتهم (كمبيوتر او موبايل...)<br><br>
  وهاد ال Model بقسم التطبيق لجزئين:<br>
  <span style=" color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">Front-End</span>:
  وهو عبارة عن الشي يلي بتشوفه بالمتصفح، متل الأزرار والنصوص وهاد الجزء بيتنفذ على المتصفح بجهازك.<br><br>

  <span style=" color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">Back-End</span>:
  هون السيرفر هو يلي بيتعامل مع الطلبات، متل تسجيل الدخول او إضافة منتج على عربة التسوق مثلاً، السيرفر بياخد طلبك وبعالجه وبعدين برد عليك بالنتيجة.<br><br>
  <span style=" color:rgb(221, 39, 78); font-size: 27px; line-height: 1.6;"><strong>مثال واقعي:</strong></span><br>
  لما العميل يزور عنوان URL ل Web Application ولنفرض مثلا انه <a href="https://www.facebook.com/">Facebook.com</a> بيستخدم السيرفر واجهة التطبيق الرئيسية UI.<br><br>
  ولما المستخدم يضغط على زر او يطلب وظيفة معينة، بيرسل الويب HTTP Request للسيرفر، والسيرفر بفسر هالطلب وبنفذ المهام اللازمة لإكمال الطلب.<br><br>
  يعني اذا فتت عالفيسبوك وكتبت ايميلك وكلمة السر وضغطت على Login، الويب بيرسل ال HTTP Request للسيرفر وبقوم السيرفر بمطابقة الايميل وكلمة السر مع قاعدة البيانات يلي عنده وبعدها بيرجع بأرسل HTTP Response للمتصفح الخاص فيك وبقوم بتحويلها للغة مقروءة وبيعرضلك صفحتك الشخصية.
</p>

---

<p>
<strong style="color:white; font-size:20px">One Server</strong>
</p>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
  هون كلشي (التطبيق وقاعدة البيانات  وكلشي) موجود على سيرفر واحد وهاد model بسيط وسهل تنفيذه بس خطير!<br>
  ليش؟ لأنك بتكون حاطت كل البيض تبعك بسلة وحدة وإذا تم اختراق اي تطبيق، فبتكون كل بياناتك معرضة للإختراق، ولو السيرفر وقف عن العمل (وقع) كل التطبيقات بتصير غير متاحة.
</p>

<div class="htblogo">
  <img src="{{ '/images/HTB/IntroToWebApplications/4.jpg' | relative_url }}" alt="HTB">
</div>

---

<p>
<strong style="color:white; font-size:20px">Many Servers - One Database</strong>
</p>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
  هون قاعدة البيانات على سيرفر منفصل، والتطبيقات على سيرفرات تانية.<br>
  يعني لو في أكتر من تطبيق، كلهم يتشاركون نفس قاعدة البيانات.
الميزة هون لو سيرفر واحد اتعرض للاختراق، الباقي ما يتأثر بشكل مباشر.<br>
ولو قاعدة البيانات اتعرضت لمشكلة، التطبيق نفسه ما يتأثر مباشرة،
بس لازم يكون في تحكم صارم بالأشخاص يلي تقدر تدخل على البيانات.
</p>

<div class="htblogo">
  <img src="{{ '/images/HTB/IntroToWebApplications/5.jpg' | relative_url }}" alt="HTB">
</div>

---

<p>
<strong style="color:white; font-size:20px">Many Servers - Many Databases</strong>
</p>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
  هاد أكتر نموذج متقدم. كل تطبيق ويب عنده قاعدة بيانات خاصة فيه، وكل قاعدة بيانات على سيرفر منفصل.<br>
  الميزة عندي أمان عالي جدًا، لأن البيانات مفصولة، ولو في مشكلة بسيرفر أو بقاعدة بيانات، الباقي بيبقى شغال.<br>
  بيستخدموه كتير عشان التكرار (Redundancy)، يعني لو سيرفر وقف، في سيرفر احتياطي بيشتغل بداله.<br>
  بس هاد ال model معقد وبيحتاج أدوات متل "Load Balancers" عشان يوزع الطلبات بين السيرفرات.
</p>

<div class="htblogo">
  <img src="{{ '/images/HTB/IntroToWebApplications/6.jpg' | relative_url }}" alt="HTB">
</div>



<script src="https://giscus.app/client.js"
        data-repo="Farouk9423/farouk9423.github.io"
        data-repo-id="R_kgDOONQTbg"
        data-category="General"
        data-category-id="DIC_kwDOONQTbs4CqhWL"
        data-mapping="pathname"
        data-strict="0"
        data-reactions-enabled="1"
        data-emit-metadata="0"
        data-input-position="bottom"
        data-theme="preferred_color_scheme"
        data-lang="ar"
        crossorigin="anonymous"
        async>
</script>