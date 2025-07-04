## خب من دستور process.memoryUsage() اجرا کردم و این خروجی رو به من داد:

```json
{
  rss: 28573696,
  heapTotal: 5062656,
  heapUsed: 3388944,
  external: 1173708,
  arrayBuffers: 10515
}
```
 بیایید تک‌تک موارد را بررسی کنیم:

### ۱. **`rss` (Resident Set Size) - ۲۸۵۷۳۶۹۶ بایت (~۲۸.۶ مگابایت)**
- **معنی**: کل حافظه فیزیکی اختصاص‌یافته به پروسه Node.js شما در RAM.
- **توضیح**: شامل heap، stack و کد برنامه است. هرچه این عدد بزرگ‌تر باشد، پروسه حافظه بیشتری از سیستم مصرف کرده است.

### ۲. **`heapTotal` - ۵۰۶۲۶۵۶ بایت (~۵.۱ مگابایت)**
- **معنی**: کل حجم heap اختصاص‌یافته توسط موتور V8 (موتور جاوااسکریپت Node.js).
- **توضیح**: این حافظه برای اشیاء، متغیرها و داده‌های پویا استفاده می‌شود.

### ۳. **`heapUsed` - ۳۳۸۸۹۴۴ بایت (~۳.۴ مگابایت)**
- **معنی**: بخشی از `heapTotal` که در حال حاضر توسط برنامه شما استفاده می‌شود.
- **نکته**: اگر `heapUsed` به `heapTotal` نزدیک شود، ممکن است به زودی **Garbage Collection** اتفاق بیفتد یا حافظه بیشتری اختصاص داده شود.

### ۴. **`external` - ۱۱۷۳۷۰۸ بایت (~۱.۲ مگابایت)**
- **معنی**: حافظه مصرف‌شده توسط منابع خارجی (مثل بافرها، داده‌های شبکه، فایل‌ها و کتابخانه‌های native).
- **مثال**: وقتی از `fs.readFile` یا `Buffer` استفاده می‌کنید، این مقدار افزایش می‌یابد.

### ۵. **`arrayBuffers` - ۱۰۵۱۵ بایت (~۱۰ کیلوبایت)**
- **معنی**: بخشی از `external` که مخصوص `ArrayBuffer` و `SharedArrayBuffer` است (داده‌های باینری مانند فایل‌ها یا داده‌های شبکه).

---

### نکات مهم:
- **واحد**: همه اعداد **بر حسب بایت** هستند. برای خوانایی بهتر می‌توانید آنها را به مگابایت تبدیل کنید (تقسیم بر `1024 * 1024`).
- **مقایسه**:  
  - `heapUsed < heapTotal`: یعنی هنوز حافظه heap آزاد دارید.  
  - اگر `heapUsed ≈ heapTotal` باشد، ممکن است برنامه شما بهینه‌سازی حافظه نیاز داشته باشد.
- **بهینه‌سازی**: اگر `rss` یا `heapUsed` به طور غیرمنتظره‌ای بالا باشد، احتمال **نشتی حافظه (Memory Leak)** وجود دارد.

---

### مثال کاربردی:
اگر بخواهید این اعداد را به مگابایت نمایش دهید:
```javascript
const usage = process.memoryUsage();
console.log({
  rss: `${Math.round(usage.rss / 1024 / 1024 * 100) / 100} MB`,
  heapTotal: `${Math.round(usage.heapTotal / 1024 / 1024 * 100) / 100} MB`,
  heapUsed: `${Math.round(usage.heapUsed / 1024 / 1024 * 100) / 100} MB`,
  external: `${Math.round(usage.external / 1024 / 1024 * 100) / 100} MB`,
  arrayBuffers: `${Math.round(usage.arrayBuffers / 1024 / 1024 * 100) / 100} MB`
});
```

خروجی:
```json
{
  "rss": "27.25 MB",
  "heapTotal": "4.83 MB",
  "heapUsed": "3.23 MB",
  "external": "1.12 MB",
  "arrayBuffers": "0.01 MB"
}
```

---
---
---

