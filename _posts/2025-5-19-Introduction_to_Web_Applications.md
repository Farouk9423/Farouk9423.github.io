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
  ال Web Application هي عبارة عن برامج بتشتغل جوا المتصفحات متل Firefox,Chrome وبتعتمد على شي اسمو  client-server architecture.<br>
  <span style="font-family: 'Amiri'; color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">العميل(Client):</span> هو المتصفح يلي المستخدم بشوف فيه واجهة التطبيق (ال Front-end) متل التصميم ولون كل زر وهي الأمور كلها...<br>
  <span style="font-family: 'Amiri'; color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">الخادم (Server):</span> هو جزء ال Back-end، موجود فيه الكود تبع التطبيق وقواعد البيانات.<br>
  الترتيب هاد بخلي الشركات بتقدر تستضيف تطبيقات قوية تتحكم فيها بشكل مباشر وتكون متاحة للناس بكل العالم، متل خدمات البريد من Google، المتاجر الالكترونية متل Amazon، برامج تحرير النصوص متل Google docs<br>
  وهالشي مو مقتصر عالشركات الكبيرة بس لانو اي Web Developer بيقدر يعمل Web Application ويستضيفه علة خدمات الاستضافة العادية، وهاد السبب انو في ملايين الWeb Applications ومليارات المستخدمين.
</p>

<div class="htblogo">
  <img src="{{ '/images/HTB/IntroToWebApplications/1.jpg' | relative_url }}" alt="HTB">
</div>


---
<h3 id="🔵 Web Applications vs. Websites" dir=""><span class="me-2" style="color:white"><strong>🔵 Web Applications vs. Websites</strong></span><a href="#🔵 Web Applications vs. Websites" class="anchor text-muted"></a></h3>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
من زمان، كانت المواقع مملة وجامدة (static)، يعني محتواها مابيتغير الا اذا المبرمج فات عالكود وغيره يدويا.<br>
يعني مثلا عنا موقع بيعرض معلومات شركة، فهي المعلومات رح تضل ثابتة ومابتتغير اذا المستخدم اتفاعل معها، هي المواقع منسميها Web 1.0، وهي متل ماحكينا بتعرض المعلومات كأنك عم تقرأ جريدة ومابتقدر تغير شي حتى تغيره المطبعة.<br>
</p>

<div class="htblogo">
  <img src="{{ '/images/HTB/IntroToWebApplications/2.png' | relative_url }}" alt="HTB">
</div>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
اما اليوم معظم تطبيقات الويب بتستعمل شي اسمو Web 2.0 وهي التطبيقات بتعرض محتوى متغير (Dynamic) وبيتغير حسب تفاعل المستخدمين معو.<br>
يعني مثلا مثلا لما نفتح هاليوتيوب ونكبس شي زر فالمحتوى يلي طالع قدامي بيتغير حسب الزر يلي كبسته.<br>
وكمان من مميزاتها انها بتشتغل على كل انواع الشاشات يعني مابهم اذا بستخدمها من موبايل او لابتوب او PC كلو شغال وكمان بتشتغل على اي نظام تشغيل OS.
</p>
---
<h3 id="🔵 Web Applications vs. Native Operating System Applications" dir=""><span class="me-2" style="color:white"><strong>🔵 Web Applications vs. Native Operating System Applications</strong></span><a href="#🔵 Web Applications vs. Native Operating System Applications" class="anchor text-muted"></a></h3>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
<span style="font-family: 'Amiri'; color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">Native OS Applications</span>
هي البرامج يلي بتتثبت على جهازك، متل برامج مايكروسوفت، وورد او الألعاب يلي بتشتغل عالويندوز أو الماك. اما ال
<span style="font-family: 'Amiri'; color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">Web Applications</span>
فهي مختلفة شوي لأنو عندها اكتر من ميزة.<br><br>
ال Web Application مستقلة عن نظام التشغيل، يعني بتشتغل على اي نظام تشغيل بغض النظر عن نوعه (ويندوز,ماك او حتى لينكس)<br>
وكمان مافي داعي ثبت ال Web Application على جهازي، لأنها بتشتغل على Server بعيد، لهيك مافي داعي خزن مساحة على الهارد تبع المستخدم.<br>
وبالإضافة لهالأمور بكون بال Web Application اللإصدار موحد للكل، يعني كل المستخدمين بكون عندهن نفس نسخة الإصدار للتطبيق، وبتم تحديثه على ال Server بدون الحاجة للإرسال تحديثات للمستخدم. وهالشي بقلل تكاليف الصيانة والدعم.<br><br>
بس كمان ال Native OS Applications في الها إيجابيات متل انها اسرع لأنها بتستخدم ملفات ومكتبات موجودة عالنظام والأجهزة المحلية، وكمان عندها قدرات افضل لاني بقدر استفيد موارد الجهاز بشكل اعمق بكتير مقارنة بتطبيقات الويب يلي بتعتمد على امكانيات المتصفح.<br><br>
آخر كم سنة طلع عندي شي اسمه
<span style="font-family: 'Amiri'; color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">Hybrid and Progressive Web Apps</span>
وهاد عبارة عن دمج لمميزات التنين فهي بتستخدم أطر عمل حديثة وبتشتغل بسرعة اكبر وبتستفيد من موارد الجهاز.
</p>


<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
في لل Web Applications نوعين رئيسيين:<br>
<span style="font-family: 'Amiri'; color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">مفتوحة المصدر (Open Source)</span>:
اي شخص ممكن يدخل عالكود تبعها ويعدل عليها لتلبي احتياجه، وهي يلي بتفضله الشركات لانها بتقدر تعدل عليها حسب شو بناسبها، امثلة عنها:
</p>
- WordPress: لإنشاء المواقع والمدونات
- OpenCart: لإنشاء المتاجر الإلكترونية
- Joomla: لإدارة المحتوى
<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
<span style="font-family: 'Amiri'; color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">مغلقة المصدر (Closed Source)</span>:
وهي يلي بتطورها الشركات وبتبيعها، او بتسمح باستخدامها بإشتراك، متل:
</p>
- Wix: لإنشاء المواقع
- Shopify: لإنشاء متاجر إلكترونية
- DotNetNuke: لإدارة المحتوى

---

<h3 id="🔵 Security Risks of Web Applications" dir=""><span class="me-2" style="color:white"><strong>🔵 Security Risks of Web Applications</strong></span><a href="#🔵 Security Risks of Web Applications" class="anchor text-muted"></a></h3>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
بما انو ال Web Applications ممكن الوصول الها من اي مكان بالعالم بمجرد مايكون عندك اتصال بالانترنت فهاد خلاها هدف سهل لأي شخص انو يقدر يهجم عليها، وكل ما كان ال Web Application اكبر ومعقدة اكتر كل ما زاد احتمال انو يكون عندي فيها مشاكل امنية، وخاصة انو عندي أدوات تلقائية لمسح وفحص ومهاجمة ال Web Applications، ولما الهاكر يستخدم هالأدوات رح يسببلي مشاكل ومخاطر كبيرة.<br><br>
اذا فرضا الهاكر قدر يخترق ال Web Application فرح تسبب خسائر مالية ورح توقف الأعمال وممكن يصير تسريب لبيانات حساسة متل معلومات العملاء والشركات، لأنه غالبا ال Web Applications بترتبط ب Servers وقواعد بيانات بتحتوي على هدول المعلومات.<br><br>
لهيك ضروري عالشركات انو تختبر تطبيقاتها مشان تكتشف الثغرات وتحاول إصلاحها بسرعة قبل ما الهكر يكتشفها🌚، ولازم تعتمد على مبدئ ال Secure Coding، واكيد لازم تتأكد انو اصلاح الثغرات ما يفتح ثغرات جديدة(يعني بالله شو استفدنا😂🤦‍♂️)<br><br>
بالنسبة لاخبار اختراق ال Web Applications، اسمها Web Application Penetration Testing، وهي مهارة لازم الشخص يلي بدو يشتغل فيها يتعلمها ولازم يكون بيعرف كيف بيشتغل ال Web Application بالأصل لأنو مافيك تخترق شي مابتعرف كيف بيشتغل🌚، ولازم نتعلم كيف بتم تطوير ال Web Applications وشو هي المخاطر الخاصة بكل طبقة وبكل مكون من مكونات التطبيق حسب شو التقنيات المستخدمة.<br><br>
وحدة من الطرق المستخدمة كتير هي استخدام دليل OWASP، وهو مرجع عالمي بال Web Penetration Testing.<br><br>
وحدة من الإجراءات المشهورة هي انو نبلش بالتأكد من امان ال Front end ونراجع كود ال HTML, CSS, Java Script وهنن اللغات المشهورة يلي بتم من خلالهم بناء ال Web Applications، فمنتأكد انو الكود مكتوب بطريقة Secure ومنحاول ندور على نقاط ضعف متل Sensitive Data Exposure وهي مهاجمة البيانات الحساسة لل Web Application وبسبب اختراق لكل البيانات يلي لازم تنحمى، وال  Cross-Site Scripting (XSS) بتسبب انو المهاجم بيقدر يوصل برامج خبيثة لل Web Application أو سرقة ال Sessions.<br>
ولايهمك اذا مافهمت كتير رح نحكي عنهن بالتفصيل لقدام لساتنا بالمقدمة🫠<br>
بعد ما نراجع كل المكونات الاساسية بال Front end منرجع ومنتأكد من باقي الوظائف الخاصة بال Web Applicatoin واتصاله بالسيرفرات وبدور عالمشاكل او الثغرات بالسيرفر.<br>
وبالعادة منقيم ال Web Application كشخص خارجي وكشخص داخلي، يعني مثلا عندي موقع بيتطلب تسجيل دخول، بطالع النتائج مرة بدون ما يكون عندي حساب ومرة وانا عندي حساب مشان اعمل مراجعة لكل السيناريوهات المحتملة عندي.
</p>
---
<h3 id="🔵 Attacking Web Applications" dir=""><span class="me-2" style="color:white"><strong>🔵 Attacking Web Applications</strong></span><a href="#🔵 Attacking Web Applications" class="anchor text-muted"></a></h3>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
معظم الشركات بغض النظر عن حجمها عندها Web Applicatoin خاص فيها من الشغلات البسيطة متل ال Static وصولا لمدونات كبيرة بتشتغل بأنظمة إدارة المحتوى متل WordPress, وصولا لانظمة كبيرة فيها تسجيل دخول وادوار للمستخدمين (متل مستخدم عادي وادمن و...)<br>
وبسبب طبيعتها ال Dynamic بتتغير هالتطبيقات باستمرار، واي تغيير بسيط ممكن يودي لظهور ثغرات خطيرة.<br>
عنا اكتر من ثغرة خطيرة ممكن تطلع عندي وحدة منهن ال
<span style="font-family: 'Amiri'; color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">SQL Injection</span>
بتصير لما حدا يتعامل مع بيانات المستخدم بشكل غير آمن، وهالشي بيسمح للمهاجم بالوصول لقاعدة البيانات وقراءة الملفات او حتى تنفيذ اوامر على السيرفر.
</p>
<div class="note" dir="rtl" style=" font-size: 20px;">
    <span style="font-family: 'Amiri'; color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">✅ ملاحظة: </span>
حكينا عن ال SQL Injection بدرس لحال بالتفصيل بتقدر ترجع تشوفها وهي اللينك تبعها 
<a href="https://farouk9423.github.io/posts/SQLfundemantle/" target="_blank" rel="noopener noreferrer" style="font-size:20px">SQL Injectoin</a>
</div>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
<br>
الثغرة يلي بعدها هي
<span style="font-family: 'Amiri'; color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">File Inclusion</span>
وهي بتسمح للمهاجم بقراء Source Code (الكود تبع التطبيق) حتى لاقي الصفحات او الملفات المخفية وهيك بقدر اكشف وظائف مخفية مو مصرح الي اني وصلها وممكن تؤدي لتنفيذ أوامر عن بعد.<br><br>
بعدين بيجي عنا ال <span style="font-family: 'Amiri'; color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">Unrestricted File Upload</span>
وهي لما بيسمح التطبيق برفع أي نوع من الملفات (مثل رفع برنامج ضار بدلاً من صورة)، يمكن للمهاجم السيطرة على الخادم.<br><br>
وبعدين بيجي عنا ال
<span style="font-family: 'Amiri'; color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">Insecure Direct Object Referencing (IDOR)</span>
وهي لما بيقدر المستخدم انو يوصل لبيانات أو وظائف بتخص مستخدمين آخرين. على سبيل المثال، إذا كان في رابط لتعديل ملفك الشخصي مثل<br> /user/701/edit-profile، يمكن للمهاجم تغيير الرقم إلى 702 لتعديل ملف مستخدم آخر.<br><br>
آخر الشي عنا ال
<span style="font-family: 'Amiri'; color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">Broken Access Control</span>
النظام عندي هو المسؤول عن تحديد الصلاحيات للمستخدمين، بس لما يصير عندي خطأ بالتنفيذ المهاجم ممكن يستغل هالشي ويوصل لمكان مو مسمحله يوصله.<br>
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

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
<span style="font-family: 'Amiri'; color:rgb(221, 39, 78); font-size: 27px; line-height: 1.6;"><strong>مثال واقعي:</strong></span><br>
اذا كان عندي تطبيق ويب بيستخدم شي اسمه Active Directory لتسجيل الدخول للمستخدمين، ممكن لثغرة ال SQL Injection انو تسمح للهاكر باستخراج ايميلات للمستخدمين وفيني استخدم هدول الايميلات بهجوم Password Spraying (بحاول بكلمات السر المشهورة على كتير حسابات) ضد بوابات ال VPN او حتى الايميلات وهالشي بوصلني للبيانات الحساسة او حتى اني اخترق الشبكة الداخلية للشركة.
</p>
---
<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
لا تخاف اذا حسيت اي شي من الحكي يلي حكيناه صعب، لانو رح نحكي عليهن لقدام بالتفصيل إن كان بهاد الدرس او بالدروس القادمة، ورح تصير تلاقيهن شوربة (شوربة = بغاية السهولة😂)<br>
لهيك يلا يا بطل خلينا نكمل وما نوقف هون وتذكر انو لازم تحس حالك غبي انت وعم تدرس حتى تصير عبقري🔥...
</p>