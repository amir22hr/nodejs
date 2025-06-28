# **نشتی حافظه (Memory Leak) در Node.js — توضیح کامل با مثال کاربردی**

## **۱. نشتی حافظه چیست؟**
نشتی حافظه (**Memory Leak**) زمانی اتفاق می‌افتد که برنامه شما حافظه را **اختصاص می‌دهد اما هرگز آن را آزاد نمی‌کند**، حتی وقتی دیگر نیازی به آن نیست. با گذشت زمان، این حافظه‌های استفاده‌نشده **انباشته می‌شوند** و باعث می‌شوند برنامه شما حافظهٔ بیشتری مصرف کند تا جایی که ممکن است **کرش کند** یا عملکردش به شدت افت کند.

---

## **۲. چرا نشتی حافظه اتفاق می‌افتد؟**
در Node.js (و جاوااسکریپت)، **Garbage Collector (GC)** مسئول آزاد کردن حافظهٔ غیرقابل‌دسترس است. اما اگر **اشتباهاً به یک شیء یا داده‌ای اشاره (Reference) نگه داشته شود**، GC نمی‌تواند آن را پاک کند.

### **دلایل رایج Memory Leak در Node.js:**
1. **اشاره‌های (References) باقی‌مانده در آرایه‌ها، آبجکت‌ها یا کلاس‌ها**
2. **عدم آزاد کردن Event Listeners و تایمرها**
3. **ذخیره‌سازی داده‌های زیاد در کش (Cache) بدون محدودیت**
4. **استفاده نادرست از Closureها**
5. **نگه‌داشتن Connectionهای باز (مثل دیتابیس یا شبکه)**

---

## **۳. مثال‌های کاربردی از Memory Leak**
### **مثال ۱: افزودن داده‌ها به آرایه بدون پاکسازی (Array Accumulation)**
```javascript
const dataStorage = [];

function processLargeData() {
  const largeData = new Array(100000).fill("Some Data");
  dataStorage.push(largeData); // هر بار یک آرایه بزرگ اضافه می‌شود، اما هیچ‌وقت پاک نمی‌شود!
}

setInterval(processLargeData, 1000); // هر ثانیه یک بار اجرا می‌شود
```
**مشکل**: آرایه `dataStorage` مدام بزرگ‌تر می‌شود، اما هیچ‌وقت خالی نمی‌شود → **Memory Leak!**

**راه‌حل**:
```javascript
const dataStorage = [];
const MAX_STORAGE = 10; // حداکثر ۱۰ داده ذخیره شود

function processLargeData() {
  const largeData = new Array(100000).fill("Some Data");
  dataStorage.push(largeData);
  
  if (dataStorage.length > MAX_STORAGE) {
    dataStorage.shift(); // قدیمی‌ترین داده را حذف کن
  }
}
```

---

### **مثال ۲: Event Listenerهای فراموش‌شده**
```javascript
const EventEmitter = require('events');
const emitter = new EventEmitter();

function logMessage() {
  console.log("Event emitted!");
}

// اضافه کردن Listener بدون حذف آن
emitter.on('message', logMessage);

// بعد از مدتی دیگر نیازی به این Listener نیست، اما همچنان در حافظه است!
```
**مشکل**: اگر `emitter` زنده بماند، `logMessage` همیشه در حافظه می‌ماند → **Memory Leak!**

**راه‌حل**:
```javascript
// بعد از استفاده، Listener را حذف کنید
emitter.off('message', logMessage);
```
یا از `once` استفاده کنید که فقط یک بار اجرا شود:
```javascript
emitter.once('message', logMessage); // فقط یک بار اجرا می‌شود و سپس حذف می‌شود
```

---

### **مثال ۳: تایمرهای فراموش‌شده (`setInterval` بدون `clearInterval`)**
```javascript
function updateData() {
  console.log("Updating data...");
}

setInterval(updateData, 1000); // هر ثانیه اجرا می‌شود

// اگر این Interval هیچ‌وقت متوقف نشود، حتی اگر دیگر نیازی نباشد، Memory Leak رخ می‌دهد
```
**راه‌حل**:
```javascript
const intervalId = setInterval(updateData, 1000);

// بعد از مدتی آن را متوقف کنید
clearInterval(intervalId);
```

---

### **مثال ۴: Closureهای ناخواسته**
```javascript
function createHeavyOperation() {
  const bigData = new Array(1000000).fill("Data"); // داده سنگین
  
  return function() {
    console.log("Operation done!");
    // با این که bigData استفاده نمی‌شود، اما Closure آن را نگه می‌دارد!
  };
}

const heavyFunc = createHeavyOperation();
heavyFunc(); // bigData هنوز در حافظه است!
```
**مشکل**: `bigData` در Closure باقی می‌ماند و GC نمی‌تواند آن را پاک کند.

**راه‌حل**:
```javascript
function createHeavyOperation() {
  const bigData = new Array(1000000).fill("Data");
  
  // کاری کنیم که بعد از اجرا، bigData آزاد شود
  return function() {
    console.log("Operation done!");
    bigData.length = 0; // حافظه را خالی کن
  };
}
```

---

## **۴. چگونه Memory Leak را تشخیص دهیم؟**
### **ابزارهای ردیابی Memory Leak**
1. **`node --inspect` + Chrome DevTools**:
   - اجرای `node --inspect script.js`
   - باز کردن `chrome://inspect` در کروم
   - بررسی تب **Memory** و **Heap Snapshot**

2. **`process.memoryUsage()`**:
   - اگر `heapUsed` مدام افزایش یابد، احتمال Memory Leak وجود دارد.

3. **ابزار `heapdump`**:
   ```bash
   npm install heapdump
   ```
   سپس در کد:
   ```javascript
   const heapdump = require('heapdump');
   heapdump.writeSnapshot(); // ذخیره وضعیت حافظه را بررسی کنید
   ```

---

## **۵. جمع‌بندی: چگونه از Memory Leak جلوگیری کنیم؟**
✅ **همیشه Listenerها و تایمرها را حذف کنید (`off`, `clearInterval`).**  
✅ **از کش‌های بدون محدودیت استفاده نکنید (مثلاً `cache.set` با TTL).**  
✅ **داده‌های بزرگ را در Closureها نگه ندارید.**  
✅ **از ابزارهای تحلیل حافظه (مثل `heapdump`) استفاده کنید.**  
✅ **آرایه‌ها و آبجکت‌های بزرگ را پس از استفاده خالی کنید (`arr.length = 0`).**  