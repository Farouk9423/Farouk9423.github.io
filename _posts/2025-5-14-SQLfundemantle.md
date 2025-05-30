---

title: "SQL Injection Fundamentals — Arabic HackThe Box ACADEMY Walkthrough"
date: 2025-05-14
author: <author_id>
layout: post
categories: Hack_The_Box_Academy
tags: [SQL, PenetrationTesting, Security, Hacker]
direction: rtl
image:
  path: /images/HTB/0.webp
  alt: 
description: قواعد البيانات جزء مهم من بنية تطبيقات الويب،  وبدرس اليوم رح نتعلم عن قواعد البيانات وكيف نستغل الحماية الضعيفة فيها.
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

<a href="https://academy.hackthebox.com/module/details/33" target="_blank" rel="noopener noreferrer" style="font-size:20px">HTB Academy الدرس على</a>

---

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
 مرحبا يا هكرجية👨🏻‍💻، انا فاروق واليوم رح نحكي ثغرة ال SQL Injection ورح نشرح كلشي بال Module من HTB.<br>
 الفكرة انو هالشي عم ياخد وقت وجهد كبير بس انا ماشي على مبدئ زكاة العلم نشره وعسى يكون صدقة جارية لأهلي<br>
 لهيك بس يلي رح اطلبه منك تتذكرنا بدعوة من قلبك وكمان تنشر العلم مع اي حدا مهتم بالمجال وبالتوفيق يارب❤️.<br>
 <strong>إن اخطأت فمن الشيطان ومن نفسي وإن اصبت فمن الله✨</strong>
</p>

---

<h2 id="Table of Contents" dir=""><span class="me-2"><strong>Table of Contents</strong></span><a href="#Table of Contents" class="anchor text-muted"></a></h2>
<h2 id="📌 Introduction:" dir=""><span class="me-2"><strong>📌 Introduction:</strong></span><a href="#📌 Introduction:" class="anchor text-muted"></a></h2>

<p dir="rtl" style=" font-size: 20px; line-height: 1.6;">
    اغلب تطبيقات الويب الحديثة بتعتمد على قاعدة بيانات (Database) بال Backend، بتستخدم هال Databases لتخزين واسترجاع البيانات المتعلقة بال Web Application، بداية من محتوى ال Website وبياناته الاساسية وصولا للبيانات الخاصة بالمستخدمين ومحتواهم على الموقع.<br>
    يعني مثلاً الانستغرام، كل المنشورات والصور والحسابات الشخصية والريلز مخزنة ضمن Databases ضخمة جدا.<br><br>
    وحتى اقدر خلي ال Web Application يكون dynamic لازم خليه يتواصل مع قاعدة البيانات.<br>
    يعني لما بيوصلني طلب HTTP/s Request من اليوزر، لازم ال Backend تبع الموقع تبعت طلب لل Database حتى يصير في استجابة.<br>
    هاد الطلب يلي بيبعته ال Web Application بسميه query وهال query بتحتوي معلومات من الطلب يلي وصلها وبعض المعلومات التانية يلي رح نشرحها بالتفصيل.<br><br>
    يعني مثلاً اذا عندي قاعدة بيانات لشركة وبدي اسماء كل الموظفين فبتكون ال query بالشكل التالي:
</p>
```sql
Select * FROM employees;
```
<div class="note" dir="rtl" style=" font-size: 20px;">
    <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">✅ ملاحظة: </span>
  طبيعي اذا مافهمت شي لسا رح نشرح التعليمات كلها بالتفصيل😅
</div>



<div class="htblogo">
  <img src="{{ '/images/HTB/1.webp' | relative_url }}" alt="HTB">
</div>

<p dir="rtl" style=" font-size: 20px; line-height: 1.6;">
الفكرة هون انو اذا بستخدم المعلومات يلي بيعطيني ياها المستخدم حتى ابني query لقاعدة البيانات تبعي رح يصير عندي كتير مشاكل، لانو ممكن الهكر بكل بساطة يستخدم هي ال query ويعدل شوي عليها ليستخدمها بشي تاني تماما او ليحاول يوصل لقواعد بيانات ما لازم يكون عنده وصول عليها وهاد الشي اسمه هجوم ال SQL Injection.<br><br>
يعني اتخيل معي اي شي بيعطيني ياه المستخدم بروح بحطه بال query تبعي بدون ما اتأكد منه، بقوم الهاكر بضيف اوامر جديدة عال query متل انه يعرض اسماء وكلمات السر تبع كل المستخدمين 🫠<br> ولقدام شوي رح نشوف كيف ممكن نعمل هالشي بأيدنا😈<br><br>
المقصود بمصطلح الSQL Injection هو الهجوم على relashional database، بينما في هجمات على ال<br> non-relational dataabase متل MongoDB.<br>
وبهي الوحدة رح نركز على ال MySQL حتى نقدر نفهم مفاهيم ال SQL Injection، وإن شاء الله لاحقاً منشرح عن ال non-relational database.
</p>
---
<h3 id="🔵 SQL Injection (SQLi):" dir=""><span class="me-2"><strong>🔵 SQL Injection (SQLi):</strong></span><a href="#🔵 SQL Injection (SQLi):" class="anchor text-muted"></a></h3>

<p dir="rtl" style=" font-size: 20px; line-height: 1.6;">
      في عندي كتير انواع من ثغرات ال Injection بتطبيقات الويب مثل:<br>
       HTTP Injection, Code Injection, Command Injection بس اشهر الشي عندي هو ال SQL Injection.<br>
      بصير عندي ال SQL Injection لما المستخدم بحط مدخلات بتغير بال query يلي بدو يبعتها الويب لقاعدة البيانات وهالشي بخلي المستخدم ينفذ quries غير يلي لازم تتنفذ متل ماشرحنا سابقاً اذا شخص غير بال query وعرض يوزرات وكلمات سر كل المستخدمين.<br><br>
      حتى نفذ ال SQL Injection، لازم اول شي حاول ادخل مدخلات SQL حتى غير بالمنطق تبع ال query وما يتم تنفيذ مجرد ال query يلي بدو ال Web Application ينفذها.<br>
      وحدة من الطرق البسيطة هي ادخال single quote (‘) أو double quote (“)، وهالشي عشان اتأكد اذا عم يتم تنفيذ اي شي بدخله بال query مباشرة ولا عم يتم تنظيفه.<br>
      <span style="  color:rgb(221, 39, 78); font-size: 27px; line-height: 1.6;"><strong>مثال:</strong></span><br>
      خلينا نتخيل انو عندي صفحة تسجيل دخول عادية، لما الشخص يدخل الاسم وكلمة السر بصير هيك عندي بال Backend:
      </p>

```sql
    SELECT * FROM users WHERE username = 'username' AND password = 'password';
```
<p dir="rtl" style=" font-size: 20px; line-height: 1.6;">
      ولما الشخص يدخل ( ' ) بدال الاسم فبصير عندي هيك اذا بحط المدخلات مباشرة بال query
      </p>

```sql
    SELECT * FROM users WHERE username = ''' AND password = 'password';
```

<p dir="rtl" style=" font-size: 20px; line-height: 1.6;">
      وهاد الشي برجع Error وهيك بعرف انه الموقع ما براقب المدخلات قبل ما يضيفها عال query
      <br><br>
      وبمجرد ما شوف انو طلع عندي Error ساعتها بحاول وسع الهجوم تبعي وادخل اوامر تانية لنفذ الامر الاساسي يلي كان موجود بالإضافة للأوامر الجديدة يلي بدي ياها.<br>
      وهالشي ممكن باستخدام شي اسمو stacked queries او using Union queries رح نحكي عنهن بشكل اوسع لاحقاً ، وبالنهاية حتى اقدر جيب المعلومات من هالاستعلام وشوفها لازم استدعيها بال Frontend مثلا.
</p>
<div class="note" dir="rtl" style=" font-size: 20px;">
    <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">✅ ملاحظة: </span>
لحد الآن عم نحكي بالمقدمة ونشرح ال Methodology بشكل عام ولقدام رح نشوف كيف منطبق كل خطوة مع امثلة. 
</div>
---
<h3 id="🔵 Use Cases and Impact:" dir=""><span class="me-2"><strong>🔵 Use Cases and Impact:</strong></span><a href="#🔵 Use Cases and Impact:" class="anchor text-muted"></a></h3>

<p dir="rtl" style=" font-size: 20px; line-height: 1.6;">
      ممكن يكون تأثير ال SQL Injection كارثي وخاصة اذا كانت الصلاحيات على ال Backend وعلى ال Database ضعيفة.<br>
      ممكن الهكر يوصل لمعلومات سرية ما لازم حدا يقدر يوصلها، متل بيانات تسجيل الدخول تبع المستخدمين وكلمات السر الخاصة فيهن أو حتى معلومات البطاقات البنكية.<br><br>
      في عندي استخدامات تانية لل SQL Injection، متل اني اقدر سجل دخول لحساب محدد بدون ما استخدم كلمة السر🌚، وممكن كمان اقدر وصل لحساب الادمن تبع الموقع وعدل بالبيانات الموجودة على كيفي وضيف اكواد على ال Backend وأعمل ثغرات جديدة بال Backend بتسمحلي اني اتحكم بالموقع بالكامل، بس طبعا ما رح نعمل هيك لانو نحنا اكابر😈<br>
</p>
---
<h3 id="🔵 Prevention:" dir=""><span class="me-2"><strong>🔵 Prevention:</strong></span><a href="#🔵 Prevention:" class="anchor text-muted"></a></h3>

<p dir="rtl" style=" font-size: 20px; line-height: 1.6;">
      غالبا بكون سبب ال SQL Injection هو اغلاط برمجية او مشاكل بالصلاحيات على ال Database، ورح نحكي لقدام شو هي اهم الطرق يلي بتقلل من احتمالية التعرض ل SQL Injection باستخدام طرق التشفير والتحقق من المدخلات وصلاحيات المستخدم بال Backend.
</p>
---
<p dir="rtl" style=" font-size: 23px; line-height: 1.6;">
<strong>
وهيك منكون خلصنا المقدمة واتعلمنا شوي عن قواعد البيانات بشكل عام واخدنا لمحة بسيطة عن ال SQL Injection، وإن شاء الله رح نتوسع اكتر ورح نبلش ندخل ونفهم قواعد البيانات بشكل اكبر.<br>
كملو يا وحوش🔥✨
</strong>
</p>
---
<h2 id="Databases" dir=""><span class="me-2"><strong>Databases</strong></span><a href="#Databases" class="anchor text-muted"></a></h2>

<h2 id="📌 Intro to Databases" dir=""><span class="me-2"><strong>📌 Intro to Databases</strong></span><a href="#📌 Intro to Databases" class="anchor text-muted"></a></h2>

<p dir="rtl" style="font-size: 20px; line-height: 1.6;">
    قبل ما نتعلم ال Sql Injection بدنا نتعلم شوي عن ال SQL، يعني اكيد حتى نقدر نعدل على الquery لازم نعرف عالقليلة نقرأها ونفهمها صح🌚<br><br>
    في انواع كتيرة ومتنوعة من قواعد البيانات ولكل نوع استخدام غير التاني، كانت التطبيقات سابقاً بتستخدم قواعد بيانات بتخزن ال Data بملفات وهاد الشي كان بطيء كتير وخاصة لما يصير عندي كمية كبيرة من الData وهاد الشي يلي جعل الكل يعتمد على ال Database Managment System(DBMS).<br>
</p>
---
<h3 id="🟢 Database Management Systems:" dir=""><span class="me-2"><strong>🟢 Database Management Systems:</strong></span><a href="#🟢 Database Management Systems:" class="anchor text-muted"></a></h3>

<p dir="rtl" style="font-size: 20px; line-height: 1.6;">
    ال Database Managment System(DBMS) هو عبارة عن نظام بساعدني على إنشاء قواعد البيانات وتعريفها وحتى إستضافتها كمان.<br><br>
    وموجود اكتر من نوع لل DBMS متل ال RDBMS وانظمة ال NoSQL وانظمة قائمة على الرسوم البيانية.<br>
    وفي اكتر من طريقة للتعامل مع ال DBMS متل سطر الاوامر(Terminal) أو الواجهات الرسومية أو حتى ال APIs.<br>
    ورح نتعامل معه كتير لاحقاً، ونفهمه بشكل اعمق.
</p>

<div class="htblogo">
  <img src="{{ '/images/HTB/2.webp' | relative_url }}" alt="HTB">
</div>

<p dir="rtl" style="font-size: 20px; line-height: 1.6;">
    المقصود من الجدول انو ال DBMS عنده كتير خصائص منها:<br> <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">Concurrency</span> ومعناها انو لما اكتر من شخص ينفذ اوامر على قعادة البيانات بنفس الوقت، بتضمن ال DBMS انو يتنفذ هاد الشي بدون ما تتخربط البيانات او يصير عندي خسارة الها.<br>
    <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">Consistency</span>
    ومقصود فيها انو ال DBMS بيتأكد من البيانات انو تكون صالحة ومتسقة بكل قاعدة البيانات.<br>
     <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">Security</span>
     بيتأكد أنو عندي ضوابط امنية من خلال التأكد من صلاحيات المستخدم والعديد من الأمور.<br>
     <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">Reliability</span>
    بيسمحلك تعمل نسخة من قاعدة البيانات وترجعلها دائما اذا صار اختراق الله لايقدر او فقدت بياناتك.<br>
    <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">Structured Query Language</span>
    ال SQL بيجعل التفاعل ابسط بين المستخدم وال Database.
</p>
---

<h3 id="🟢 Architecture" dir=""><span class="me-2"><strong>🟢 Architecture</strong></span><a href="#🟢 Architecture" class="anchor text-muted"></a></h3>
<p dir="rtl" style="font-size: 20px; line-height: 1.6;">
    بتوضحلي هالصورة انو عندي بنيتين بالموقع:
</p>

<div class="htblogo">
  <img src="{{ '/images/HTB/3.webp' | relative_url }}" alt="HTB">
</div>

<p dir="rtl" style=" font-size: 20px; line-height: 1.6;">
<br>
    <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">البنية الأولى:</span>
    اسمها ال Frontend بتتكون عادة من الجانب الخاص بالمستخدم، متل مواقع الويب والواجهات الرسومية او حتى الصور والألوان، يعني حرفيا كلشي بشوفه المستخدم، وبتكون غالباً تفاعلية يعني فيها صفحة تسجيل دخول ومكان للتعليقات، والبيانات يلي مندخلها بتنتقل للبنية التانية عن طريق شي اسمو API أو Requests.<br><br>
    <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">البنية الثانية:</span>
    اسمها ال Backend، وهي المسؤولة عن تفسير البيانات يلي بتجيني من الطبقة الاولى وتجهيزها بالشكل يلي بيقبله ال DBMS، وبتستخدم هالطبقة مكتبات وبرامج حسب نوع ال DBMS يلي عم تتعامل معو.<br><br>
    بعدين بيستلم ال DBMS الأوامر من الطبقة الثانية وبينفذ الأوامر المطلوبة متل ادخال بيانات او تعديل بيانات او حتى حذف بيانات.<br>
    وبرجع ال DBMS البيانات المطلوبة او الاخطاء يلي بتطلع عندي اذا كان في queries غير صحيحة.<br>
</p>
<div dir="rtl" style="border-left: 4px solid #0af; background-color: rgba(0, 170, 255, 0.1); padding: 1em; margin: 1em 0; border-radius: 8px; color: #eee;">
  💡 يعني حرفياً ال Frontend هو المكان يلي بيتعامل معه المستخدم ولما بيختار شي بحاجة التعامل مع قاعدة البيانات بتم ارسال هالشي لل Backend وهون بيتحول الطلب ل query بيفهمها ال DBMS، بعدين بيرجع الرد من ال DBMS لل Frontend مشان يتم عرضه للمستخدم.
</div>


<div class="note" dir="rtl" style=" font-size: 20px;">
    <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">✅ ملاحظة: </span>
ممكن اني ضيف ال Web Application وال DBMS على نفس السيرفر بس عادة بتنحط ال Database يلي فيها كمية كبيرة من المعلومات وفيها مستخدمين كتير على سيرفر لحال مشان يكون عندي قابلية لزيادة سعتها وتعطيني اداء افضل.
</div>
---
<h2 id="📌 Types of Databases" dir=""><span class="me-2"><strong>📌 Types of Databases</strong></span><a href="#📌 Types of Databases" class="anchor text-muted"></a></h2>

<p dir="rtl" style="font-size: 20px; line-height: 1.6;">
    حكينا سابقا انو عنا نوعين اساسيين لل Databases، ال
    <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">Relational Databases</span> وال
    <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">Non-Relational Databases</span>، 
    بالنسبة لل <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">Relational Databases</span> فهي بتستخدم لغة SQL فقط اما ال <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">Non-Relational Databases</span> فبتستخدم كذا طريقة للتواصل.<br>
    وهلق رح نحكي عنهن اكتر ونعرف الفروقات بيناتهم.
</p>
---
<h3 id="🟣 Relational Databases:" dir=""><span class="me-2"><strong>🟣 Relational Databases:</strong></span><a href="#🟣 Relational Databases:" class="anchor text-muted"></a></h3>

<p dir="rtl" style="font-size: 20px; line-height: 1.6;">
    ال <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">Relational Databases</span> هي النوع المنتشر والمتداول اكتر، بتستخدم شي اسمو <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">Schema</span> وهي متل قالب بحدد شكل البيانات جوا قاعدة البيانات.<br><br>
    خلينا نتخيل الموضوع متل شركة بتبيع منتجات، فمشان تنظم معلوماتها رح تكون بحاجة لمعلومات من الزباين متل: مين الزبون وشو المنتجات يلي اشتراها وقديش دفع...<br>
    فال <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">Schema</span> هون بتكون متل الخريطة يلي بتنظم كل هالمعلومات جوا جداول، يعني رح يكون عندي جدول للزباين وجدول للمنتجات يلي بتبيعهن الشركة ورح يكون فيه اسم المنتج وعدد القطع الموجودة ورح يكون في جدول للمشتريات موجود فيه مين الزبون يلي اشترى وقديش دفع وكيف دفع والخ...<br><br>
    وهالصورة بتوضح الجداول، عندي مثلا جدول اسمه Customer، بداخله اعمدة متل CustomerID ونوعه Integer, FirstName ونوعه String...
    </p>

<div class="htblogo">
  <img src="{{ '/images/HTB/13.png' | relative_url }}" alt="HTB">
</div>

<p dir="rtl" style="font-size: 20px; line-height: 1.6;">
    رح نحكي اكتر كيف نقرأ هيك جداول وكيف نصممها بأيدنا، بس هاد مثال عن البيانات كيف بتتخزن مو اكتر.<br><br>
    بترتبط الجداول ببعضها عن طريق شي اسمو Keys بتساعدني اني اوصل بسرعة للبيانات او بتعطيني ملخص عنها، وكل جدول فيه Key متل رقم تعريفي بيربطه بجدول تاني.<br>
    مثلا جدول الزباين فيه اعمدة متل CustomerID(رقم تعريفي لكل زبون), FirstName, Address... وجدول المنتجات فيه اعمدة متل ProductID(رقم تعريفي لكل منتج), ProdactName, Price...<br><br>
    هلق لما بجي بدي اعمل جدول ال Orders بكفي حط فيه ال CustomerID وال ProductID وال Quantity ويلي هي الكمية، وهو من جدول ال Customers بجيب كل تفاصيل الزبون ومن جدول ال Products بجيب كل تفاصيل المنتج.<br><br>
    ومشان نربط هدول الجداول مع بعض منستخدم <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">Relational Database Management System ( RDBMS )</span>
    وهو مفهوم بيطبق من انواع كبيرة من قواعد البيانات متل MySQL, PostgreSQL, Microsoft Access وهالشي بساعدني اربط الجداول ببعض من خلال ال Keys بسهولة، والشركات صارت تحب تستخدم ال RDBMS لانه سهل الواحد يتعلمه ويستخدمه.<br><br>
  <span style="  color:rgb(221, 39, 78); font-size: 27px; line-height: 1.6;"><strong>مثال عملي:</strong></span><br>
    عندي جدول اسمه
     <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">Users</span> فيه اعمدة متل:
    <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">id</span>,
    <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">username</span>, 
    <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">firstname</span>, 
    <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">lastname</span>.<br>
    وجدول ال
    <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">Posts</span> فيه منشورات المستخدمين مع التاريخ وفيه عمود اسمه <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">user_id</span> موجود فيه رقم ال <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">id</span> للمستخدم مع ال Post تبعه.<br>
    وهيك اذا بدنا نجيب  كل المنشورات لمستخدم معين منربط ال <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">id</span> تبع المستخدم مع ال <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">user_id</span> وهيك منجيب كلشي معلومات بدون تكرار😎<br><br>
    مثال عن ال query يلي بستخدمها لتنفيذ هالشي:
</p>
```sql
SELECT * FROM posts WHERE user_id=2;
```

<div class="htblogo">
  <img src="{{ '/images/HTB/4.jpg' | relative_url }}" alt="HTB">
</div>

<p dir="rtl" style="font-size: 20px; line-height: 1.6;">
    بجدول متل جدول ال Posts ممكن يكون عندي Key تاني متل ID للمنشورات حتى اربطه بجدول ثالث متل جدول التعليقات مثلا.<br>
    وهيك باستخدام ال RDBMS منقدر نجيب كل البيانات عن شي عنصر متل منتج او منشور من كل الجداول ب query وحدة وهالشي بخليها سريعة ومضمونة وخاصة بالبيانات الكبيرة واشهر مثال عنها ال MySQL ويلي رح نشرحه اكتر بهالوحدة.
</p>

<div class="note" dir="rtl" style="font-size: 20px;">
    <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">✅ ملاحظة: </span>
بسمي العلاقة بين الجداول جوا قاعدة البيانات ب Schema
</div>
---
<h3 id="🟣 Non-relational Databases:" dir=""><span class="me-2"><strong>🟣 Non-relational Databases:</strong></span><a href="#🟣 Non-relational Databases:" class="anchor text-muted"></a></h3>

<p dir="rtl" style="font-size: 20px; line-height: 1.6;">
    بالنسبة لل Non-relational Databases ويلي بقول عليها كمان <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">NoSQL</span> مابتستخدم الجداول ولا الاعمدة ولا ال Keys ولا حتى ال Schema.
    وبدال كل هالأمور بتخزن البيانات بأكتر من طريقة حسب انواع البيانات مشان هيك هي كتير مرنة وبتكون قابلة للتوسع ومناسبة للبيانات يلي مو منظمة بشكل واضح.<br><br>
    انواع التخزين بال NoSQL:<br>
</p>

-  Key-Value
-  Document-Based
-  Wide-Column
-  Graph

<p dir="rtl" style="font-size: 20px; line-height: 1.6;">
    كل وحدة من هالأنواع بتخزن البيانات بطريقة شكل يعني على سبيل المثال ال 
    <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">Key-Value</span>
    بتنحفظ البيانات بشكل أزواج (Key-Value) بصيغة JSON او XML
</p>

<div class="htblogo">
  <img src="{{ '/images/HTB/5.webp' | relative_url }}" alt="HTB">
</div>

<p dir="rtl" style="font-size: 20px; line-height: 1.6;">
    اذا بدي شوف المثال السابق بشي حقيقي اكتر:
</p>

<div class="code-title">Json</div>

<style>
  .json-box {
    background-color: #1e1e1e;
    color: #dcdcdc;
    padding: 15px;
    border-radius: 8px;
    font-family: monospace;
    font-size: 15px;
    overflow-x: auto;
    line-height: 1.6;
    border-left: 5px solid #2ea043;
    box-shadow: 0 2px 5px rgba(0,0,0,0.3);
  }

  .json-key { color: #9cdcfe; }
  .json-string { color: #ce9178; }
  .json-number { color: #b5cea8; }
</style>

<pre class="json-box">
<code>
<span class="json-key">"100001"</span>: {
  <span class="json-key">"date"</span>: <span class="json-string">"01-01-2021"</span>,
  <span class="json-key">"content"</span>: <span class="json-string">"Welcome to this web application."</span>
},
<span class="json-key">"100002"</span>: {
  <span class="json-key">"date"</span>: <span class="json-string">"02-01-2021"</span>,
  <span class="json-key">"content"</span>: <span class="json-string">"This is the first post on this web app."</span>
},
<span class="json-key">"100003"</span>: {
  <span class="json-key">"date"</span>: <span class="json-string">"02-01-2021"</span>,
  <span class="json-key">"content"</span>: <span class="json-string">"Reminder: Tomorrow is the ..."</span>
}
</code>
</pre>

<p dir="rtl" style="font-size: 20px; line-height: 1.6;">
    هاد الشي بيشبه ال Dictionary بلغات متل ال Python وال PHP وال Key بكون غالباً سلسلة نصية(String) بس ال Value ممكن تكون اي شي ممكن حتى تكون Dictionary تاني.
    واشهر شي عندي بال NoSQL هو ال 
    <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">MongoDB</span>
</p>

<div class="note" dir="rtl" style=" font-size: 20px;">
    <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">✅ ملاحظة: </span>
    ال 
    Non-relational Databases الها طريقة تانية تماماً لل Injection بتختلف عن ال SQL Injection، بالنسبة لل NoSQL Injection رح نحكي عنو بمرات جاية لحال إن شاء الله.
</div>
---
<p dir="rtl" style=" font-size: 23px; line-height: 1.6;">
<strong>
وهيك خلصنا قسم جديد وقربنا خطوة🔥<br>
اتعلمنا عن انواع قواعد البيانات والفرق بيناتهن ولقدام رح نفهمهم اكتر ونتسحن اكتر، لهيك خليك مبسوط بالتطور يلي عملته🫡<br>
وكمل يا وحش✨</strong>
</p>
---
<h2 id="MySQL" dir=""><span class="me-2"><strong>MySQL</strong></span><a href="#MySQL" class="anchor text-muted"></a></h2>

<h2 id="📌 Intro to MySQL" dir=""><span class="me-2"><strong>📌 Intro to MySQL</strong></span><a href="#📌 Intro to MySQL" class="anchor text-muted"></a></h2>

<p dir="rtl" style="font-size: 20px; line-height: 1.6;">
    بهي الوحدة رح يبلش الجد وحتى نقدر نستغل ال SQL Injection بشكل صحيح لازم نفهم ال SQL, MySQL اكتر، مشان نفهم آلية ال Injection  لهيك بهالقسم رح نحكي اكتر عن اساسيات SQL/MySQL والقواعد الخاصة فيهن، مع شوية امثلة مستخدمة بال MySQL/MariaDB Databases, لهيك يلا يا وحش جهز فنجان القهوة وورقة وقلم وركز معي.
</p>
---
<h3 id="⚪ Structured Query Language (SQL):" dir=""><span class="me-2"><strong>⚪ Structured Query Language (SQL):</strong></span><a href="#⚪ Structured Query Language (SQL):" class="anchor text-muted"></a></h3>

<p dir="rtl" style="font-size: 20px; line-height: 1.6;">
    حكينا سابقاً انو ال SQL هي لغة بتستعمل للتعامل مع ال RDBMS متل ال MySQL أو MariaDB وطبعا في اختلاف بطريقة الكتابة بين كل وحدة والتانية بس كلهن لازم يتبعوا معيار ISO لل SQL يعني حتى لو كان في اختلاف بطريقة الكتابة بس المنطق نفسه، وفينا نستخدمها للأغراض التالية:
</p>

<div dir="rtl">
- جلب البيانات (متل اني استعرض اسماء الزبائن)<br>
- تحديث البيانات (متل اني غير عنوان الزبون)<br>
- حذف البيانات (متل إزالة طلب)<br>
- إنشاء جداول أو قواعد بيانات جديدة<br>
- إضافة/إزالة مستخدمين وتحديد صلاحياتهم<br>
</div>
---
<h3 id="⚪ Command Line:" dir=""><span class="me-2"><strong>⚪ Command Line:</strong></span><a href="#⚪ Command Line:" class="anchor text-muted"></a></h3>

<p dir="rtl" style="font-size: 20px; line-height: 1.6;">
    منستخدم الأداة     
    <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">mysql</span>
    لتسجيل الدخول والتفاعل مع قاعدة البيانات MySQL/MariaDB، منستخدم الرمز -u لإدخال اسم المستخدم والرمز -p لإدخال كلمة السر، بس طبعا ما مندخل كلمة السر مباشرة بال Command مشان ما تنحفظ بال bash_history، لهيك لأمان اكثر منترك كلمة السر فاضية وبس يطلبها مندخلها:
</p>



<div class="code-title">Intro to MySQL</div>

<pre class="terminal-box"><code>
<span class="user">Farouk9423@htb</span><span class="path">[/htb]</span><span class="symbol">$</span> <span class="cmd">mysql -u root -p</span>

<span class="input">Enter password:</span> <span class="dim">&lt;password&gt;</span>
<span class="dim">...SNIP...</span>

<span class="prompt">mysql&gt;</span>
</code></pre>

<p dir="rtl" style="font-size: 20px; line-height: 1.6;">
    وهيك بتنطلب كلمة السر بعد ما نفذ الأمر وهيك بضمن انها ما رح تنحفظ.<br>
    برجع بكرر انو ممكن اني حط كلمة السر مباشرة بالأمر بس هالشي رح يكون مو آمن متل هيك:
</p>

<div class="code-title">Intro to MySQL</div>

<pre class="terminal-box"><code>
<span class="user">Farouk9423@htb</span><span class="path">[/htb]</span><span class="symbol">$</span> <span class="cmd">mysql -u root -p(password)</span>

<span class="dim">...SNIP...</span>

<span class="prompt">mysql&gt;</span>
</code></pre>


<div class="note" dir="rtl" style=" font-size: 20px;">
  <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">✅ ملاحظة: </span>
  ما لازم اترك مسافة بين ال -p وكلمة السر.
</div>

<p dir="rtl" style="font-size: 20px; line-height: 1.6;">
    بالأمثلة السابقة سجلنا دخول ك superuser يعني 
      <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">root </span> باستخدام كلمة سر، وهاد الشي بيعطيني صلاحيات لتنفيذ كل الأوامر  أما المستخدمين الآخرين فبكون عندهن صلاحيات محدودة لتنفيذ الأوامر، وحتى شوف كلشي صلاحيات عندي بستخدم أمر SHOW GRANTS ويلي رح نحكي عنو اكتر شوي تانية.<br><br>
      لما ما بحدد اي host، فهو بيتصل تلقائيا على ال 
      <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">localhost server </span>
      ومنقدر نتصل على سيرفر بعيد بتحديد ال host وال port باستخدام:
</p>

<div class="code-title">Intro to MySQL</div>

<pre class="terminal-box"><code>
<span class="user">Farouk9423@htb</span><span class="path">[/htb]</span><span class="symbol">$</span> <span class="cmd"> mysql -u root -h docker.hackthebox.eu -P 3306 -p </span>

<span class="input">Enter password:</span> <span class="dim">&lt;password&gt;</span>
<span class="dim">...SNIP...</span>

<span class="prompt">mysql&gt;</span>
</code></pre>

<div class="note" dir="rtl" style=" font-size: 20px;">
  <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">✅ ملاحظة: </span>
  ال Port الافتراضي ل MySQL/MariaDB هو 3306 بس ممكن يتغير من الأعدادات ل Port تاني لهيك بحدد ال Port بالرمز -P على عكس كلمة السر يلي بحددها بالرمز -p.
</div>
---
<h3 id="⚪ Creating a database:" dir=""><span class="me-2"><strong>⚪ Creating a database:</strong></span><a href="#⚪ Creating a database:" class="anchor text-muted"></a></h3>

<p dir="rtl" style="font-size: 20px; line-height: 1.6;">
    بعد ما تسجل دخول لقاعدة البيانات باستخدام اداة mysql، منقدر نستخدم اوامر SQL للتعامل مع قاعدة البيانات مثلا:<br>
    مشان ننشئ قاعدة بيانات جديدة في MySQL منستخدم امر 
    <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">CREATE DATABASE </span>
</p>

<div class="code-title">Intro to MySQL</div>

<pre class="terminal-box"><code>
<span class="prompt">mysql&gt;</span> <span class="cmd">CREATE DATABASE users;</span>
<span class="dim">Query OK, 1 row affected (0.02 sec)</span>
</code></pre>


<p dir="rtl" style="font-size: 20px; line-height: 1.6;">
    هاد الأمر بينشئ قاعدة بيانات اسمها users. وأهم شي لا تنسى الفاصلة المنقوطة (;) عقدة المبرمجين😂<br>
    وبعدين بعمل عرض لكل قواعد البيانات يلي موجودين عندي بأمر SHOW DATABASES ، وبس بدي اختار قاعدة البيانات يلي بدي ياها بكتب امر USE وثم اسم قاعدة البيانات، متل الموضح بالمثال:
</p>

<div class="code-title">Intro to MySQL</div>

<pre class="terminal-box"><code>
<span class="prompt">mysql&gt;</span> <span class="cmd">SHOW DATABASES;</span>

+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| users              |
+--------------------+
</span>
<span class="prompt">mysql&gt;</span> <span class="cmd">USE users;</span>
<span class="dim">Database changed  </span>
</code></pre>

<div class="note" dir="rtl" style=" font-size: 20px;">
    <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">✅ ملاحظة: </span>
    
اوامر ال SQL غير حساسة لحالة الأحرف، يعني فيني استخدم use users او USE users <br>
بس اسم قاعدة البيانات حساس لحالة الأحرف يعني USE USERS غير USE users
</div>
---
<h3 id="⚪ Tables:" dir=""><span class="me-2"><strong>⚪ Tables:</strong></span><a href="#⚪ Tables:" class="anchor text-muted"></a></h3>

<p dir="rtl" style="font-size: 20px; line-height: 1.6;">
    نظام ال DBMS بخزن البيانات على شكل جداول، وكل جدول فيه:<br>
- صفوف(أفقية): بتمثل السجلات<br>
- أعمدة(عمودية): بتمثل خصائص كل سجل<br>
- تقاطع الصف مع العمود منسميه خلية<br>
<br>
كل جدول بينشأ بعدد ثابت من الأعمدة وكل عمود عنده نوع محدد من البيانات مثل (أرقام،نصوص،تواريخ...)<br>
مثلا بال MySQL الأنواع المشهورة هي:<br><br>
- <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">INT </span>: 
للأرقام الصحيحة<br>
- <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">VARCHAR(n) </span>:
للنصوص والحرف n بحدد اكبر عدد ممكن دخله من المحارف.<br>
- <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">DATETIME </span>:
للتواريخ والوقت. <br>
<br>
حتى انشئ جدول بستخدم امر 
<span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">CREATE TABLE</span>:<br>
</p>

<style>
  .code-block {
  color: #ccc;
  font-family: monospace;
  font-size: 0.95rem;
}

/* تلوين العناصر */
.keyword { color: #00ffff; font-weight: bold; }     /* CREATE, TABLE */
.table   { color: #7CFC00; }                        /* اسم الجدول */
.field   { color: #ffaa00; }                        /* أسماء الحقول */
.type    { color: #00afff; font-style: italic; }    /* أنواع البيانات */
.code-block {
  background-color: #1e1e1e; /* اللون الجديد */
  color: #ccc;
}
</style>
<div class="code-title">SQL</div>

<pre class="code-block"><code>
<span class="keyword">  CREATE TABLE</span> <span class="table">logins</span> (
    <span class="field">id</span> <span class="type">INT</span>,
    <span class="field">username</span> <span class="type">VARCHAR</span>(100),
    <span class="field">password</span> <span class="type">VARCHAR</span>(100),
    <span class="field">date_of_joining</span> <span class="type">DATETIME</span>
    );
</code></pre>

<div class="code-title">Intro to MySQL</div>

<pre class="terminal-box"><code>
<span class="prompt">mysql&gt;</span> CREATE TABLE logins (
<span class="prompt">&gt;</span> id INT,
<span class="prompt">&gt;</span> username VARCHAR(100),
<span class="prompt">&gt;</span> password VARCHAR(100),
<span class="prompt">&gt;</span> date_of_joining DATETIME
<span class="prompt">&gt;</span> );

<span class="dim">Query OK, 0 rows affected (0.03 sec) </span>
</code></pre>

<p dir="rtl" style="font-size: 20px; line-height: 1.6;">
    وهيك عملنا جدول اسمه logins موجود فيه 4 اعمدة:<br>
    - <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">id </span>: 
    رقم صحيح<br>
    - <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">password, username </span>: 
    نصوص وبحد اقصى بقدر دخل 100 محرف واي ادخال اطول من هيك رح يسببلي Error<br>
    - <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">date_of_joining </span>: 
    تاريخ ووقت<br>

    وفيني اعرض قائمة بالجداول الموجودة عندي بالأمر <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">SHOW TABLES </span>
</p>

<div class="code-title">Intro to MySQL</div>

<pre class="terminal-box"><code>
<span class="prompt">mysql&gt;</span> SHOW TABLES;

+-----------------+
| Tables_in_users |
+-----------------+
| logins          |
+-----------------+
<span class="dim">
1 row in set (0.00 sec)
</span>
</code></pre>

<p dir="rtl" style="font-size: 20px; line-height: 1.6;">
  واذا بدي شوف الجدول مع الأعمدة وأنواعها بكتب امر
  <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">DESCRIBE</span>:
</p>

<div class="code-title">Intro to MySQL</div>

<pre class="terminal-box"><code>
<span class="prompt">mysql&gt;</span> DESCRIBE logins;

+-----------------+--------------+
| Field           | Type         |
+-----------------+--------------+
| id              | int          |
| username        | varchar(100) |
| password        | varchar(100) |
| date_of_joining | date         |
+-----------------+--------------+
<span class="dim">
4 rows in set (0.00 sec)
</span>
</code></pre>

<br>

<p dir="rtl" style="font-size: 20px; line-height: 1.6;">
  مع امر
  <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">CREATE TABLE</span>
  منقدر نحدد خصائص للأعمدة، يعني فيني خلي ال id يزيد تلقائيا كل مرة باستخدام 
  <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">AUTO_INCREMENT </span> وهيك بضمن انو ال id كل مرة بزيد واحد بشكل تلقائي:
</p>

<div class="code-title">SQL</div>

<style>
  .code-block {
  color: #ccc;
  font-family: monospace;
  font-size: 0.95rem;
  overflow-x: auto;
}

/* تخصيص الألوان */
.field      { color: #ffaa00; }                   /* اسم الحقل */
.type       { color: #00afff; font-style: italic; }/* نوع البيانات */
.constraint { color: #ffcc00; font-weight: bold; }/* قيود مثل NOT NULL */
.directive  { color: #7CFC00; font-weight: bold; }/* تعليمات مثل AUTO_INCREMENT */

</style>
<pre class="code-block"><code>
  <span class="field">id</span> <span class="type">INT</span> <span class="constraint">NOT NULL</span> <span class="directive">AUTO_INCREMENT</span>,
</code></pre>
<br>
<p dir="rtl" style="font-size: 20px; line-height: 1.6;">
  وبقدر كمان ضيف 
  <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">NOT NULL</span> 
  وهيك بضمن انو ممنوع اترك اي عمود فاضي واذا ضل فاضي بيطلع ايرور بوجهي، وكمان فيني استخدم امر
  <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">UNIQE</span>
  حتى اضمن انو القيمة دائما فريدة ومو مستخدمة قبل، يعني اذا استخدمتها مع عمود ال username، فهيك بضمن انو ماعندي تنين بنفس الأسم:
</p>

<div class="code-title">SQL</div>

<pre class="code-block"><code>
  <span class="field">username </span> <span class="type">INT</span> <span class="constraint">UNIQUE</span> <span class="directive">VARCHAR(100)</span>,
</code></pre>

<p dir="rtl" style="font-size: 20px; line-height: 1.6;">
  وكمان عندي ال
  <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">DEFAULT</span>
  وبستخدمها حتى حدد القيمة الأفتراضية، يعني مثلا بعمود 
  <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">date_of_joining</span>
  فينا نحط القيمة الافتراضية هي Now() وال MySQL بتحولها للوقت والتاريخ الحالي وقت التنفيذ:
</p>

<div class="code-title">SQL</div>

<pre class="code-block"><code>
  <span class="field">date_of_joining</span> <span class="type">DATETIME</span> <span class="constraint">DEFAULT</span> <span class="directive">NOW()</span>,
</code></pre>

<p dir="rtl" style="font-size: 20px; line-height: 1.6;">
  آخر واهم خاصية عندي هي ال 
  <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">PRIMARY KEY</span>
  ويلي بستخدمه لتحديد كل صف بشكل فريد، يعني مافي سجلين بالجدول بكون الهم نفس قيمة ال Primary Key، والهدف منه اني اقدر ميز كل سجل عن التاني بسهولة متل ماشرحنا قبل عن ربط الجداول، ورح نشوف هالشي بالأمثلة لقدام
</p>

<div class="code-title">SQL</div>

<pre class="code-block"><code>
   <span class="constraint">PRIMARY KEY</span> <span class="directive">(id)</span>
</code></pre>

<p dir="rtl" style="font-size: 20px; line-height: 1.6;">
  كيف بتطلع ال Query بشكل كامل:
</p>

<div class="code-title">SQL</div>

<style>
  .code-block {
    color: #ccc;
    font-family: monospace;
    font-size: 0.95rem;
    overflow-x: auto;
  }

  .field      { color: #ffaa00; }                   /* اسم الحقل */
  .type       { color: #00afff; font-style: italic; }/* نوع البيانات */
  .constraint { color: #ffcc00; font-weight: bold; }/* قيود مثل NOT NULL */
  .directive  { color: #7CFC00; font-weight: bold; }/* تعليمات مثل AUTO_INCREMENT */
</style>

<pre class="code-block"><code>
<span class="directive">  CREATE TABLE</span> <span class="field">logins</span> (
  <span class="field">  id</span> <span class="type">INT</span> <span class="constraint">NOT NULL</span> <span class="directive">AUTO_INCREMENT</span>,
  <span class="field">  username</span> <span class="type">VARCHAR(100)</span> <span class="constraint">UNIQUE</span> <span class="constraint">NOT NULL</span>,
  <span class="field">  password</span> <span class="type">VARCHAR(100)</span> <span class="constraint">NOT NULL</span>,
  <span class="field">  date_of_joining</span> <span class="type">DATETIME</span> <span class="directive">DEFAULT NOW()</span>,
  <span class="directive">  PRIMARY KEY</span> (<span class="field">id</span>)
      );
</code></pre>

<div class="note" dir="rtl" style=" font-size: 20px;">
    <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">✅ ملاحظة: </span>
السيرفرات (مثل Apache/MySQL) بتحتاج 10-15 ثانية عشان تبدأ، فانتظر شوية قبل ما تجرب الأوامر.
</div>

<p dir="" style="color: white; font-size: 22px; line-height: 1.6;">
  <strong>HTB Questions:</strong>
</p>

<p style="font-size: 20px; line-height: 1.6;">
Answer the question(s) below to complete this Section and earn cubes!<br><br>
Connect to the database using the MySQL client from the command line. Use the 'show databases;' command to list databases in the DBMS. What is the name of the first database? 
  
</p>

<p dir="" style="color: white; font-size: 22px; line-height: 1.6;">
  <strong>Answer: employees</strong>
</p>

<div class="htblogo">
  <img src="{{ '/images/HTB/7.png' | relative_url }}" alt="HTB">
</div>

<div class="htblogo">
  <img src="{{ '/images/HTB/6.png' | relative_url }}" alt="HTB">
</div>
---
<h2 id="📌 SQL Statements" dir=""><span class="me-2"><strong>📌 SQL Statements</strong></span><a href="#📌 SQL Statements" class="anchor text-muted"></a></h2>
<p dir="rtl" style="font-size: 22px; line-height: 1.6;">
بالقسم هاد رح نحكي عن اوامر SQL ونشوف كيف بدنا ندخل معلومات على الجداول او نحذف او حتى نعدل، كل شوي عم تصير الدروس اهم واهم لهيك غسل وجهك وركز منيح يابطل🔥
</p>
---
<h3 id="🟡 INSERT Statement:" dir=""><span class="me-2"><strong>🟡 INSERT Statement:</strong></span><a href="#🟡 INSERT Statement:" class="anchor text-muted"></a></h3>

<p dir="rtl" style="font-size: 22px; line-height: 1.6;">
  اتعلمنا قبل كيف نستخدم اداة ال mysql وانشأنا قواعد بيانات وجداول، خلينا نشوف هلق أمر مهم من أوامر ال SQL ويلي هو
  <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">INSERT</span>.<br>
  بعد ما انشأنا قاعدة البيانات وانشأنا جداول داخلها، صار الوقت لنعبي معلومات داخل هالجداول، امر INSERT بسمحلي ضيف صفوف جديدة بالجداول يلي عندي بالشكل التالي:
</p>

<div class="code-title">SQL</div>

<pre class="terminal-box"><code>
<span class="prompt">mysql&gt;</span> INSERT INTO logins VALUES(1, 'admin', 'p@ssw0rd', '2020-07-02');
<span class="dim">
Query OK, 1 row affected (0.00 sec)
</span>
</code></pre>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
  بهاد المثال ضفنا سطر جديد على جدول logins وحطينا البيانات الخاصة بكل عمود بالتريب.<br><br>
  طيب بلكي عندي اعمدة بدي اتركهن فاضيين وبدي حط مثلا بس ال username , password؟<br>
  ساعتها رح اكتب الأمر بهالطريقة:🫠
</p>

<div class="code-title">SQL</div>
<pre class="terminal-box"><code>
<span class="prompt">mysql&gt;</span>  INSERT INTO logins(username, password) VALUES('administrator', 'adm1n_p@ss');
<span class="dim">
Query OK, 1 row affected (0.00 sec)
</span>
</code></pre>

<div class="note" dir="rtl" style=" font-size: 20px;">
    <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">✅ ملاحظة: </span>
  اني اترك الأعمدة يلي هي "NOT NULL" فاضية رح يسببلي Errors
</div>

<div class="note" dir="rtl" style=" font-size: 20px;">
    <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">✅ ملاحظة: </span>
  بتقدر تضيف اكتر من سطر ورا بعض باستخدام ال ,
</div>

<div class="code-title">SQL</div>
<pre class="terminal-box"><code>
<span class="prompt">mysql&gt;</span>  INSERT INTO logins(username, password) VALUES ('john', 'john123!'), ('tom', 'tom123!');
<span class="dim">
Query OK, 2 rows affected (0.00 sec)
Records: 2  Duplicates: 0  Warnings: 0
</span>
</code></pre>


<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
  هون نحنا بس ادخلنا ال username , password واتخطينا ال id لانو رح يتولد تلقائياً بسبب ال AUTO_INCREMENT، اما ال date_of_joining فرح تاخد القيمة الافتراضية يلي هي now
</p>

<div class="note" dir="rtl" style=" font-size: 20px;">
    <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">✅ ملاحظة: </span>
بالأمثلة ادخلنا كلمة السر بشكل نص وهاد الشي على ارض الواقع مو آمن ابدا، الافضل اني اعملها Hash وهو نوع من التشفير بعدين احفظها.
</div>
---
<h3 id="🟡 SELECT Statement:" dir=""><span class="me-2"><strong>🟡 SELECT Statement:</strong></span><a href="#🟡 SELECT Statement:" class="anchor text-muted"></a></h3>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
  بعد ما استخدمنا امر INSERT وضفنا البيانات عالجداول، صار وقت نشوف كيف بدنا نستعرض هالبيانات ونشوفها يعني مو معقول خزن البيانات بدون ما ارجع شوفها🌚، لهيك لنرجع نعرض البيانات يلي بالجدول رح نستخدم امر
<span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">SELECT</span>
، والأمر هاد من اهم اوامر SQL ورح نعرف ليش بعدين.<br><br>
بس بشكل عام اذا بدك تشوف البيانات يلي مخزنها بالجداول رح تستخدم الأمر بهالطريقة:

</p>

<div class="code-title">SQL</div>

<pre class="code-block"><code>
<span class="directive">  SELECT</span> * <span class="directive">FROM</span> اسم الجدول;
  
</code></pre>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
  أمر <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">SELECT</span> ليقرأ البيانات، اما رمز النجمة * فمعناه انو اقرأ كل الأعمدة من هاد الجدول.<br><br>
  طيب لنفرض انو مابدي كل الأعمدة وبدي عدد محدد من الأعمدة فبحددهن بدال النجمة:<br>
</p>

<div class="code-title">SQL</div>

<pre class="code-block"><code>
<span class="directive">  SELECT</span> column1, column2 <span class="directive">FROM</span> اسم الجدول;
  
</code></pre>


<div class="code-title">SQL</div>

<pre class="terminal-box"><code>
<span class="prompt">mysql&gt;</span> SELECT * FROM logins;
+----+---------------+------------+---------------------+
| id | username      | password   | date_of_joining     |
+----+---------------+------------+---------------------+
|  1 | admin         | p@ssw0rd   | 2020-07-02 00:00:00 |
|  2 | administrator | adm1n_p@ss | 2020-07-02 11:30:50 |
|  3 | john          | john123!   | 2020-07-02 11:47:16 |
|  4 | tom           | tom123!    | 2020-07-02 11:47:16 |
+----+---------------+------------+---------------------+
<span class="dim">
4 rows in set (0.00 sec)
</span>
<span class="prompt">mysql&gt;</span> SELECT username,password FROM logins;
+---------------+------------+
| username      | password   |
+---------------+------------+
| admin         | p@ssw0rd   |
| administrator | adm1n_p@ss |
| john          | john123!   |
| tom           | tom123!    |
+---------------+------------+
<span class="dim">
4 rows in set (0.00 sec)
</span>
</code></pre>
---
<h3 id="🟡 SELECT DROP:" dir=""><span class="me-2"><strong>🟡 SELECT DROP:</strong></span><a href="#🟡 SELECT DROP:" class="anchor text-muted"></a></h3>


<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
  منستخدم الأمر
   <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">DROP</span>
   لنحذف جدول أو قاعدة بيانات من ال Server بشكل نهائي، يعني لما بستخدم هاد الأمر على جدول او قاعدة بيانات بينحذف بشكل نهائي مع البيانات يلي جواته ومستحيل اقدر رجعو إلا اذا كان عندي نسخة احتياطية.
</p>

<div class="code-title">SQL</div>

<pre class="terminal-box"><code>
<span class="prompt">mysql&gt;</span> DROP TABLE logins;
<span class="dim">
Query OK, 0 rows affected (0.01 sec)
</span>
<span class="prompt">mysql&gt;</span> SHOW TABLES;
<span class="dim">
Empty set (0.00 sec)
</span>
</code></pre>
---
<h3 id="🟡 ALTER DROP:" dir=""><span class="me-2"><strong>🟡 ALTER DROP:</strong></span><a href="#🟡 ALTER DROP:" class="anchor text-muted"></a></h3>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
  هاد الأمر منستخدمه حتى نغير بهيكيلية الجدول الموجود، يعني اذا بدنا نغير اسم الجدول، او نضيف عمود جديد، او نحذف عمود، او حتى انو نغير اسم عمود موجود رح نستخدم
   <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">ALTER</span>
   هو متل اداة لصيانة الجدول😅<br><br>
   فينا نستخدمه لإضافة اعمدة مع الأمر
   <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">ADD</span>:
</p>

<div class="code-title">SQL</div>

<pre class="terminal-box"><code>
<span class="prompt">mysql&gt;</span> ALTER TABLE logins ADD newColumn INT;
<span class="dim">
Query OK, 0 rows affected (0.01 sec)
</span>
</code></pre>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
  وكمان بقدر غير اسم عمود موجود بأمر
   <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">RENAME COLUMN</span>:
</p>

<pre class="terminal-box"><code>
<span class="prompt">mysql&gt;</span> ALTER TABLE logins RENAME COLUMN newColumn TO newerColumn;
<span class="dim">
Query OK, 0 rows affected (0.01 sec)
</span>
</code></pre>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
  وبقدر حتى اني عدل ال Datatype تبع الأعمدة الموجودة باستخدام 
   <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">MODIFY</span>:
</p>

<pre class="terminal-box"><code>
<span class="prompt">mysql&gt;</span> ALTER TABLE logins MODIFY newerColumn DATE;
<span class="dim">
Query OK, 0 rows affected (0.01 sec)
</span>
</code></pre>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
  واخيرا بقدر اني احذف عمود بشكل نهائي اذا بستخدم امر
   <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">DROP</span>:
</p>

<pre class="terminal-box"><code>
<span class="prompt">mysql&gt;</span> ALTER TABLE logins DROP newerColumn;
<span class="dim">
Query OK, 0 rows affected (0.01 sec)
</span>
</code></pre>

<div class="note" dir="rtl" style=" font-size: 20px;">
    <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">✅ ملاحظة: </span>
بقدر استخدم اي وحدة من الأوامر يلي فوق طول ما معي الصلاحيات الكافية.
</div>
---
<h3 id="🟡 UPDATE DROP:" dir=""><span class="me-2"><strong>🟡 UPDATE DROP:</strong></span><a href="#🟡 UPDATE DROP:" class="anchor text-muted"></a></h3>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
  بالنسبة لأمر
   <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">UPDATE</span>
   فهو بيشبه شوي ال ALTER، بس الفرق انو ALTER بعدل على الاعمدة والهيكلية تبع الجدول، بينما UPDATE بعدل القيم داخل الجدول(الصفوف).<br><br>
   يعني مثلا عندي عمود فيه اسماء السناغل، وحطيت اسمك يا عزيزي القارئ (لانو لو عندك حدا تحكي معو ماكنت ضيعت وقتك هون🫠)، بأمر UPDATE بقدر غير بقدر غير اسمك وقت ترتبط نشالله ونفرحلك ويضل اسمي لحالي.<br>
   بينما ال ALTER بغير العمود كلو وبخليه اسماء المرتبطين مثلا🌚<br><br>
   وبستخدمه بهالطريقة:
</p>

<div class="code-title">SQL</div>

<pre class="code-block"><code>
<span class="directive">  UPDATE</span> table_name <span class="directive">SET</span> column1=newvalue1, column2=newvalue2, ... <span class="directive">WHERE</span> __condition__;
  
</code></pre>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
  الفكرة اني بحط امر UPDATE بعدين بحط اسم الجدول اما بالنسبة ل SET فهو بحدد شو هي الأعمدة يلي بدي غير القيم تبعها وشو هي القيم الجديدة، وآخر الشي بستخدم WHERE حتى حط اذا عندي شرط للتغيير واذا ماحطيته رح يتغير الكل.<br><br>
  مثال عملي، لنفرض عندي هاد الجدول:<br>
</p>

<div class="code-title">SQL</div>

<pre class="terminal-box"><code>
<span class="prompt">mysql&gt;</span> UPDATE logins SET password = 'change_password' WHERE id > 1;
<span class="dim">
Query OK, 3 rows affected (0.00 sec)
Rows matched: 3  Changed: 3  Warnings: 0

</span>
<span class="prompt">mysql&gt;</span> SELECT * FROM logins;
+----+---------------+-----------------+---------------------+
| id | username      | password        | date_of_joining     |
+----+---------------+-----------------+---------------------+
|  1 | admin         | p@ssw0rd        | 2020-07-02 00:00:00 |
|  2 | administrator | change_password | 2020-07-02 11:30:50 |
|  3 | john          | change_password | 2020-07-02 11:47:16 |
|  4 | tom           | change_password | 2020-07-02 11:47:16 |
+----+---------------+-----------------+---------------------+
<span class="dim">
4 rows in set (0.00 sec)
</span>
</code></pre>


<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
  ال Query السابقة بدلت كلمات السر لكل السجلات يلي ال id تبعها اكبر من 1 ل change_password
</p>
---
<p dir="" style="color: white; font-size: 22px; line-height: 1.6;">
  <strong>HTB Questions:</strong>
</p>

<p dir="" style="font-size: 20px; line-height: 1.6;">
  Answer the question(s) below to complete this Section and earn cubes!
  What is the department number for the 'Development' department? 
</p>

<p dir="" style="color: white; font-size: 22px; line-height: 1.6;">
  <strong>Answer: d005</strong>
</p>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
  اول شي اتصلت على قاعدة البيانات وعرضت كلشي DATABASES
</p>
```sql
mysql -u root -h <Target system IP> -P <Port Namber> -p
SHOW DATABASES;
```

<div class="htblogo">
  <img src="{{ '/images/HTB/8.png' | relative_url }}" alt="HTB">
</div>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
  بعدين اخترتها واستعرضت كلشي جداول فيها حتى لقيت ال departments
</p>
```sql
USE employees;
SHOW TABLES;
```

<div class="htblogo">
  <img src="{{ '/images/HTB/9.png' | relative_url }}" alt="HTB">
</div>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
  آخر شي عرضت كلشي موجود جوا هالقاعدة البيانات
</p>
```sql
SELECT * FROM departments;
```

<div class="htblogo">
  <img src="{{ '/images/HTB/10.png' | relative_url }}" alt="HTB">
</div>

---
<h2 id="📌 Query Results" dir=""><span class="me-2"><strong>📌 Query Results</strong></span><a href="#📌 Query Results" class="anchor text-muted"></a></h2>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
  بهالقسم رح نشوف كيف فينا نتحكم بالنتائج يلي يلي بتطلع من ال Quries، بستخدم الأمر 
  <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">ORDER BY</span>
  مشان رتب نتائج الاستعلام من عمود معين.
</p>

<div class="code-title">SQL</div>
<pre class="terminal-box"><code>
<span class="prompt">mysql&gt;</span> SELECT * FROM logins ORDER BY password;
+----+---------------+------------+---------------------+
| id | username      | password   | date_of_joining     |
+----+---------------+------------+---------------------+
|  2 | administrator | adm1n_p@ss | 2020-07-02 11:30:50 |
|  3 | john          | john123!   | 2020-07-02 11:47:16 |
|  1 | admin         | p@ssw0rd   | 2020-07-02 00:00:00 |
|  4 | tom           | tom123!    | 2020-07-02 11:47:16 |
+----+---------------+------------+---------------------+
<span class="dim">
4 rows in set (0.00 sec)
</span>
</code></pre>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
  وهيك رتبنا النتائج حسب عمود
  <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">Password</span>
  بشكل تصاعدي من A الى Z.
</p>

<div class="code-title">SQL</div>
<pre class="terminal-box"><code>
<span class="prompt">mysql&gt;</span> SELECT * FROM logins ORDER BY password DESC;
+----+---------------+------------+---------------------+
| id | username      | password   | date_of_joining     |
+----+---------------+------------+---------------------+
|  4 | tom           | tom123!    | 2020-07-02 11:47:16 |
|  1 | admin         | p@ssw0rd   | 2020-07-02 00:00:00 |
|  3 | john          | john123!   | 2020-07-02 11:47:16 |
|  2 | administrator | adm1n_p@ss | 2020-07-02 11:30:50 |
+----+---------------+------------+---------------------+
<span class="dim">
4 rows in set (0.00 sec)
</span>
</code></pre>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
  وهون رتبناهن بشكل تنازلي من Z الى A.<br><br>
  واذا عندي قيم مكررة بقدر رتبهن حسب عمود تاني بهالطريقة:
</p>

<div class="code-title">SQL</div>
<pre class="terminal-box"><code>
<span class="prompt">mysql&gt;</span> SELECT * FROM logins ORDER BY password DESC, id ASC;
+----+---------------+-----------------+---------------------+
| id | username      | password        | date_of_joining     |
+----+---------------+-----------------+---------------------+
|  1 | admin         | p@ssw0rd        | 2020-07-02 00:00:00 |
|  2 | administrator | change_password | 2020-07-02 11:30:50 |
|  3 | john          | change_password | 2020-07-02 11:47:16 |
|  4 | tom           | change_password | 2020-07-02 11:50:20 |
+----+---------------+-----------------+---------------------+
<span class="dim">
4 rows in set (0.00 sec)
</span>
</code></pre>
---
<h3 id="⚫ LIMIT Results:" dir=""><span class="me-2"><strong>⚫ LIMIT Results:</strong></span><a href="#⚫ LIMIT Results:" class="anchor text-muted"></a></h3>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
  بستخدم الأمر 
  <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">LIMIT</span>
  مشان حدد عدد السجلات يلي بدي شوفها من النتائج وخاصة اذا كان الجدول فيو كتير Data.
</p>

<div class="code-title">SQL</div>
<pre class="terminal-box"><code>
<span class="prompt">mysql&gt;</span> SELECT * FROM logins LIMIT 2;
+----+---------------+------------+---------------------+
| id | username      | password   | date_of_joining     |
+----+---------------+------------+---------------------+
|  1 | admin         | p@ssw0rd   | 2020-07-02 00:00:00 |
|  2 | administrator | adm1n_p@ss | 2020-07-02 11:30:50 |
+----+---------------+------------+---------------------+
<span class="dim">
2 rows in set (0.00 sec)
</span>
</code></pre>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
  واذا بدي حط نقطة بداية (Offset) ابدأ منها مشان ما يبدألي من الأول بعمل هيك:
</p>

<div class="code-title">SQL</div>
<pre class="terminal-box"><code>
<span class="prompt">mysql&gt;</span> SELECT * FROM logins LIMIT 1, 2;
+----+---------------+------------+---------------------+
| id | username      | password   | date_of_joining     |
+----+---------------+------------+---------------------+
|  2 | administrator | adm1n_p@ss | 2020-07-02 11:30:50 |
|  3 | john          | john123!   | 2020-07-02 11:47:16 |
+----+---------------+------------+---------------------+
<span class="dim">
2 rows in set (0.00 sec)
</span>
</code></pre>

<div class="note" dir="rtl" style=" font-size: 20px;">
    <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">✅ ملاحظة: </span>
    ال Offset بتبدأ من ال 0 فانا هون اخدت السجل رقم 2 و 3
</div>
---
<h3 id="⚫ WHERE Clause:" dir=""><span class="me-2"><strong>⚫ WHERE Clause:</strong></span><a href="#⚫ WHERE Clause:" class="anchor text-muted"></a></h3>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
  بستخدم
<span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">WHERE</span>
مع امر SELECT مشان فلتر البحث بناء على شرط معين وما طلع كلشي نتائج عندي، يعني مثلا بدي كل الموظفين يلي بلشو شغل بعد تاريخ محدد او بدي يلي ال id تبعهن اكبر من عدد محدد وكتير غير امثلة...
</p>

<div class="code-title">SQL</div>
<pre class="terminal-box"><code>
<span class="prompt">mysql&gt;</span> SELECT * FROM logins WHERE id > 1;
+----+---------------+------------+---------------------+
| id | username      | password   | date_of_joining     |
+----+---------------+------------+---------------------+
|  2 | administrator | adm1n_p@ss | 2020-07-02 11:30:50 |
|  3 | john          | john123!   | 2020-07-02 11:47:16 |
|  4 | tom           | tom123!    | 2020-07-02 11:47:16 |
+----+---------------+------------+---------------------+
<span class="dim">
3 rows in set (0.00 sec)
</span>
</code></pre>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
انا طلبت كل العالم يلي ال id تبعهن اكبر من ال1 لهيك اتجاهل اول id
</p>

<div class="code-title">SQL</div>
<pre class="terminal-box"><code>
<span class="prompt">mysql&gt;</span> SELECT * FROM logins where username = 'admin';
+----+----------+----------+---------------------+
| id | username | password | date_of_joining     |
+----+----------+----------+---------------------+
|  1 | admin    | p@ssw0rd | 2020-07-02 00:00:00 |
+----+----------+----------+---------------------+
<span class="dim">
1 row in set (0.00 sec)
</span>
</code></pre>

<div class="note" dir="rtl" style=" font-size: 20px;">
    <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">✅ ملاحظة: </span>
النصوص او التواريخ لازم حطهن جوا علامات تنصيص مثل ('admin') اما الأرقام فمو بحاجة
</div>
---
<h3 id="⚫ LIKE Clause:" dir=""><span class="me-2"><strong>⚫ LIKE Clause:</strong></span><a href="#⚫ LIKE Clause:" class="anchor text-muted"></a></h3>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
  امر
<span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">LIKE</span>
بستخدمه مع الشرط WHERE مشان ابحث عن نمط معين بالنصوص، يعني لو بدك تدور على شي بيشبه كلمة معينة فمنقدر نستخدمه، يعني مثلا لو بدي دور بالجداول على اي حدا بأسمه في كلمة admin، فبستخدم هالطريقة
</p>

<div class="code-title">SQL</div>
<pre class="terminal-box"><code>
<span class="prompt">mysql&gt;</span> SELECT * FROM logins WHERE username LIKE 'admin%';
x+----+---------------+------------+---------------------+
| id | username      | password   | date_of_joining     |
+----+---------------+------------+---------------------+
|  1 | admin         | p@ssw0rd   | 2020-07-02 00:00:00 |
|  4 | administrator | adm1n_p@ss | 2020-07-02 15:19:02 |
+----+---------------+------------+---------------------+
<span class="dim">
2 rows in set (0.00 sec)
</span>
</code></pre>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
  الـ % معناها أي حروف بعد admin. فالأمر جاب admin و administrator لأنهم بيبدأو بـ admin.<br>
  طيب مثلا بدي جيب السجلات يلي ال username تبعها مكون من 3 خانات فبستخدم ال ( _ ) بالشكل التالي:
</p>

<div class="code-title">SQL</div>
<pre class="terminal-box"><code>
<span class="prompt">mysql&gt;</span> SELECT * FROM logins WHERE username like '___';
+----+----------+----------+---------------------+
| id | username | password | date_of_joining     |
+----+----------+----------+---------------------+
|  3 | tom      | tom123!  | 2020-07-02 15:18:56 |
+----+----------+----------+---------------------+
<span class="dim">
1 row in set (0.01 sec)
</span>
</code></pre>

<p dir="" style="color: white; font-size: 22px; line-height: 1.6;">
  <strong>HTB Questions:</strong>
</p>

<p dir="" style="font-size: 20px; line-height: 1.6;">
  Answer the question(s) below to complete this Section and earn cubes!
  What is the last name of the employee whose first name starts with "Bar" AND who was hired on 1990-01-01? 
</p>

<p dir="" style="color: white; font-size: 22px; line-height: 1.6;">
  <strong>Answer: Mitchem</strong>
</p>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
  اول شي اتصلت على قاعدة البيانات
</p>
```sql
mysql -u root -h <Target system IP> -P <Port Namber> -p
```
<div class="htblogo">
  <img src="{{ '/images/HTB/8.png' | relative_url }}" alt="HTB">
</div>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
  بعدين اخترتها واستعرضت كلشي جداول فيها 
</p>
```sql
USE employees;
SHOW TABLES;
```

<div class="htblogo">
  <img src="{{ '/images/HTB/9.png' | relative_url }}" alt="HTB">
</div>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
  بعدين استخدمت امر DESCRIBE على الجدول employees، مشان شوف اسماء الأعمدة يلي داخلها.
</p>
```sql
DESCRIBE employees;
```
<div class="htblogo">
  <img src="{{ '/images/HTB/11.png' | relative_url }}" alt="HTB">
</div>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
  بالنهاية حطيت امر البحث المطلوب مني بحيث ابحث عن ال first_name يلي بيبدأ ب Bar وانضم بتاريخ 1990-01-01
</p>
```sql
SELECT * FROM employees WHERE first_name like 'Bar%' AND hire_date = '1990-01-01';
```
<div class="htblogo">
  <img src="{{ '/images/HTB/12.png' | relative_url }}" alt="HTB">
</div>
---
<h2 id="📌 SQL Operators" dir=""><span class="me-2"><strong>📌 SQL Operators</strong></span><a href="#📌 SQL Operators" class="anchor text-muted"></a></h2>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
  احيانا ما بكفي شرط واحد لجلب البيانات من قاعدة البيانات وبحتاج اكتر من شرط بنفس الوقت، وشفنا هالشي بحل السؤال السابق.<br><br>
  بهاد الدرس رح نحكي عن ال Operators وكيف ندمج اكتر من شرط بنفس ال query، والخبر الحلو انو هاد آخر درس SQL ومن الدرس القادم رح نبلش بال Injection😍، لهيك صحصح معي وخلينا نخلص هالدرس بسرعة🔥
</p>

<h3 id="🟠 AND Operator:" dir=""><span class="me-2"><strong>🟠 AND Operator:</strong></span><a href="#🟠 AND Operator:" class="anchor text-muted"></a></h3>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
  ال <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">And Operator</span>
  بياخد شرطين وبرجع قيمة <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">True</span> او 
  <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">False</span> بناءً على تقييمهم
</p>
```sql
condition1 AND condition2
```
<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
النتيجة لل <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">And</span> بتكون <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">True</span> فقط اذا كان الشرط الاول والشرط الثاني نتيجتهم <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">True</span>، ولو وحدة بيناتهم <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">False</span> بتطلع النتيجة <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">False</span><br><br>
بعرف مافهمت كتير لهيك خلينا ناخد هالمثال، لو كتبنا هيك بقاعدة البيانات:
</p>

<div class="code-title">SQL</div>
<pre class="terminal-box"><code>
<span class="prompt">mysql&gt;</span> SELECT 1 = 1 AND 'test' = 'test';
+---------------------------+
| 1 = 1 AND 'test' = 'test' |
+---------------------------+
|                         1 |
+---------------------------+
<span class="dim">
1 row in set (0.00 sec)
</span>
<span class="prompt">mysql&gt;</span> SELECT 1 = 1 AND 'test' = 'abc';
+--------------------------+
| 1 = 1 AND 'test' = 'abc' |
+--------------------------+
|                        0 |
+--------------------------+
<span class="dim">
1 row in set (0.00 sec)
</span>
</code></pre>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
بالمثال الأول طلبنا من قاعدة البيانات تتحقق من إذا ما كان 1 = 1 (صحيح)، وإذا كان النص 'test' = 'test'(كمان صحيح)، النتيجة النهائية من جمعهم بتكون صحيح، القيمة 1 يعني صحيح لانه بال MySQL اي قيمة غير الصفر بتعني صحيح.<br><br>
بالمثال التاني عدلنا شوي عالإستعلام، 1=1 (صحيح) و 'test' = 'abc' (خطأ)، النتيجة كانت خطأ للاستعلام بالكامل(0)
</p>

> حتى تكون النتيجة صح لازم الشرطين يكونو صح، واذا كان في غلط بواحد النتيجة بتكون خطأ
{: .prompt-info }

---

<h3 id="🟠 OR Operator:" dir=""><span class="me-2"><strong>🟠 OR Operator:</strong></span><a href="#🟠 OR Operator:" class="anchor text-muted"></a></h3>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
كمان ال <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">OR Operator</span> بياخد شرطين، بس بتكون النتيجة صحيحة لما واحد على الأقل صح، وما بيشترط على الاثنين انو يكونو صح.<br><br>
  <span style="  color:rgb(221, 39, 78); font-size: 27px; line-height: 1.6;"><strong>مثال:</strong></span><br>
لو كتبنا استعلام متل هاد:
</p>

<div class="code-title">SQL</div>
<pre class="terminal-box"><code>
<span class="prompt">mysql&gt;</span> SELECT 1 = 1 OR 'test' = 'abc';
</code></pre>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
هون بقول تحقق اذا كانت 1 = 1 (صحيحة)، أو إذا كان النص 'test' = 'abc' (خطأ)، فالنتيجة بتكون:
</p>

<div class="code-title">SQL</div>
<pre class="terminal-box"><code>
+-------------------------+
| 1 = 1 OR 'test' = 'abc' |
+-------------------------+
|                       1 |
+-------------------------+
</code></pre>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
يس النتيجة بتكون صحيحة، طيب ليش!<br>
السبب يا صديقي أن الشرط الأول 1 = 1 صحيح، واتفقنا انو ال OR بيكتفي بأنو يكون واحد من الشرطين يكون صحيح.<br><br>
طيب لو كتبت ال query التانية بهالطريقة:
</p>

<div class="code-title">SQL</div>
<pre class="terminal-box"><code>
<span class="prompt">mysql&gt;</span> SELECT 1 = 2 OR 'test' = 'abc';
</code></pre>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
هون بقول تحقق اذا كانت 2 = 1 (خطأ)، أو إذا كان النص 'test' = 'abc' (خطأ)، فالنتيجة بتكون:
</p>

<div class="code-title">SQL</div>
<pre class="terminal-box"><code>
+-------------------------+
| 1 = 2 OR 'test' = 'abc' |
+-------------------------+
|                       0 |
+-------------------------+
</code></pre>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
لأن الشرطين خطأ، وانا بحاجة واحد صحيح على الأقل حتى اقدر اعتبرها صحيحة.
</p>

> حتى تكون النتيجة صح لازم شرط واحد على الأقل يكونو صح، واذا كانو كل الشروط خطأ النتيجة بتكون خطأ
{: .prompt-info }

---

<h3 id="🟠 NOT Operator:" dir=""><span class="me-2"><strong>🟠 NOT Operator:</strong></span><a href="#🟠 NOT Operator:" class="anchor text-muted"></a></h3>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
بالنسبة لل <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">NOT Operator</span>
بياخد شرط واحد وبيعكس النتيجة، يعني لو الشرط صحيح برجع نتيجة <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">False</span>, ولو الشرط خطأ برجع النتيجة <span style="  color:rgb(138, 201, 38); font-size: 20px; line-height: 1.6;">True</span><br><br>
<span style="  color:rgb(221, 39, 78); font-size: 27px; line-height: 1.6;"><strong>مثال:</strong></span><br>
لو كتبت استعلام متل هيك:
</p>

<div class="code-title">SQL</div>
<pre class="terminal-box"><code>
<span class="prompt">mysql&gt;</span> SELECT NOT 1 = 1;
</code></pre>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
هون عم قله يعطيني عكس نتيجة 1 = 1 ويلي هي صحيحة، فبيعطيني:
</p>

<div class="code-title">SQL</div>
<pre class="terminal-box"><code>
+-----------+
| NOT 1 = 1 |
+-----------+
|         0 |
+-----------+
</code></pre>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
طيب لو كتبت:
</p>

<div class="code-title">SQL</div>
<pre class="terminal-box"><code>
<span class="prompt">mysql&gt;</span> SELECT NOT 1 = 2;
</code></pre>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
هون عم قله يعطيني عكس نتيجة 2 = 1 ويلي هي غلط، فبيعطيني:
</p>

<div class="code-title">SQL</div>
<pre class="terminal-box"><code>
+-----------+
| NOT 1 = 2 |
+-----------+
|         1 |
+-----------+
</code></pre>

> بيعكس نتيجة الشرط: صحيح يصير غلط، وغلط يصير صحيح
{: .prompt-info }

---

<h3 id="🟠 Symbol Operator:" dir=""><span class="me-2"><strong>🟠 Symbol Operator:</strong></span><a href="#🟠 Symbol Operator:" class="anchor text-muted"></a></h3>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
بلغة ال SQL، بدل ما تستخدم AND, OR, NOT، ممكن تستخدم رموز بديلة:<br>
</p>

- AND = &&
- OR = \|\|
- NOT = !

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
وهي امثلة من الأوامر السابقة باستخدام الرموز:
</p>

<div class="code-title">SQL</div>

<pre class="terminal-box"><code>
<span class="prompt">mysql&gt;</span> SELECT 1 = 1 && 'test' = 'abc';
+-------------------------+
| 1 = 1 && 'test' = 'abc' |
+-------------------------+
|                       0 |
+-------------------------+
<span class="dim">
1 row in set, 1 warning (0.00 sec)
</span>
<span class="prompt">mysql&gt;</span> SELECT 1 = 1 || 'test' = 'abc';
+-------------------------+
| 1 = 1 || 'test' = 'abc' |
+-------------------------+
|                       1 |
+-------------------------+
<span class="dim">
1 row in set, 1 warning (0.00 sec)
</span>
<span class="prompt">mysql&gt;</span> SELECT 1 != 1;
+--------+
| 1 != 1 |
+--------+
|      0 |
+--------+
<span class="dim">
1 row in set (0.00 sec)
</span>
</code></pre>

> بالنسبة للرمز =! معناه غير يساوي يعني 1 لا يساوي 1
{: .prompt-tip }

---

<h3 id="🟠 Operators in queries:" dir=""><span class="me-2"><strong>🟠 Operators in queries:</strong></span><a href="#🟠 Operators in queries:" class="anchor text-muted"></a></h3>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
خلينا نشوف كيف نستخدم هي ال Operators بقاعدة بيانات حقيقية.<br><br>
لنفترض عنا جدول اسمه logins بيحتوي على بيانات المستخدمين متل id, username, password, date_of_joining<br>
وقررت جيب كل السجلات ما عدا السجلات الخاصة بالمستخدم john
</p>

<div class="code-title">SQL</div>
<pre class="terminal-box"><code>
<span class="prompt">mysql&gt;</span> SELECT * FROM logins WHERE username != 'john';
+----+---------------+------------+---------------------+
| id | username      | password   | date_of_joining     |
+----+---------------+------------+---------------------+
|  1 | admin         | p@ssw0rd   | 2020-07-02 00:00:00 |
|  2 | administrator | adm1n_p@ss | 2020-07-02 11:30:50 |
|  4 | tom           | tom123!    | 2020-07-02 11:47:16 |
+----+---------------+------------+---------------------+
<span class="dim">
3 rows in set (0.00 sec)
</span>
</code></pre>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
قاعدة البيانات عرضت كل المستخدمين عدا john.<br><br>
وفرضاً قررت اعرض كل المستخدمين عدا john ويلي ال id تبعهم اكبر من 1:
</p>

<div class="code-title">SQL</div>
<pre class="terminal-box"><code>
<span class="prompt">mysql&gt;</span> SELECT * FROM logins WHERE username != 'john' AND id > 1;
+----+---------------+------------+---------------------+
| id | username      | password   | date_of_joining     |
+----+---------------+------------+---------------------+
|  2 | administrator | adm1n_p@ss | 2020-07-02 11:30:50 |
|  4 | tom           | tom123!    | 2020-07-02 11:47:16 |
+----+---------------+------------+---------------------+
<span class="dim">
2 rows in set (0.00 sec)
</span>
</code></pre>

---

<h3 id="🟠 Multiple Operator Precedence:" dir=""><span class="me-2"><strong>🟠 Multiple Operator Precedence:</strong></span><a href="#🟠 Multiple Operator Precedence:" class="anchor text-muted"></a></h3>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
بلغة ال SQL ممكن استخدم اكثر من Operator بنفس الاستعلام، متل جمع وطرح ومقارنات، و AND, OR, NOT ، بس مو كل ال Operators بتتنفذ بنفس الوقت لانه في ترتيب للأولويات وهو يلي بحدد أي عملية تتنفذ اول والترتيب بكون عالشكل التالي:
</p>

- القسمة (/)، الضرب (*)، الباقي (%)ء
- الجمع (+)، الطرح (-).
- عوامل المقارنة (مثل =، >، <، <=، >=، !=، LIKE).
- NOT أو !.
- AND أو &&.
- OR أو \|\|.

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
<span style="  color:rgb(221, 39, 78); font-size: 27px; line-height: 1.6;"><strong>أمثلة على الأولوليات:</strong></span><br><br>
مثلا لو عندي هاد الاستعلام:
</p>

```sql
SELECT * FROM logins WHERE username != 'tom' AND id > 3 - 2;
```

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
بنفذ اول شي عملية الطرح:
</p>
`3 - 2 = 1`

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
بالنسبة لعمليات المقارنة != و > ، هنن التنين الهن نفس الأولوية فبيتنفذوا مع بعض:<br>
username != 'tom' بتجيب كل المستخدمين عدا يلي اسمهن tom<br>
id > 1 بيجيب كل السجلات اللي رقم المعرف فيها أكبر من 1.<br><br>
بالنهاية ال AND بيربط بيناتهن، ولازم الشرطين يكونو صحيحين عشان يظهر السجل.<br><br>
النتيجة:
</p>

<div class="code-title">SQL</div>
<pre class="terminal-box"><code>
<span class="prompt">mysql&gt;</span> SELECT * FROM logins WHERE username != 'tom' AND id > 3 - 2;
+----+---------------+------------+---------------------+
| id | username      | password   | date_of_joining     |
+----+---------------+------------+---------------------+
|  2 | administrator | adm1n_p@ss | 2020-07-03 12:03:53 |
|  3 | john          | john123!   | 2020-07-03 12:03:57 |
+----+---------------+------------+---------------------+
<span class="dim">
2 rows in set (0.00 sec)
</span>
</code></pre>

<p dir="rtl" style=" font-size: 22px; line-height: 1.6;">
يعني السجلات يلي ظهرت بتحقق الشرطين، الاسم ليس tom و ال id اكبر من 1.<br><br>
رح نشوف غير سيناريوهات لل operator precedence بالأقسام القادمة.
</p>

<p dir="" style="color: white; font-size: 22px; line-height: 1.6;">
  <strong>HTB Questions:</strong>
</p>

<p style="font-size: 20px; line-height: 1.6;">
Answer the question(s) below to complete this Section and earn cubes!<br><br>
 In the 'titles' table, what is the number of records WHERE the employee number is greater than 10000 OR their title does NOT contain 'engineer'?  
</p>
<br>
<p dir="" style="color: white; font-size: 22px; line-height: 1.6;">
  <strong>Answer: 654</strong>
</p>

<!-- <div class="htblogo">
  <img src="{{ '/images/HTB/.png' | relative_url }}" alt="HTB">
</div> -->

---









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