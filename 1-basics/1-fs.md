### fs : File System

* [readFile](#)
* [readFileSync](#)
* [writeFile](#)
* [writeFileSync](#)
* [appendFile](#)
* [appendFileSync](#)
* [access](#)
* [stat](#)
* [rename](#)
* [unlink](#)
* [mkdir](#)
* [readdir](#)
* [rmdir](#)
* [rm](#)
* [createReadStream](#)
* [createWriteStream](#)
* [Pipe](#)
* [chmod](#)
* [fs/promises](#)
  
---

#### our have two method for "fs":

- Async ==> Callback ,  Promise
- Sync

---

### readFile , readFileSync

#### Async
```js
const fs = require('fs');

fs.readFile('file.txt', 'utf8', (err, data) => {
    if (err) throw err;
    console.log(data); // محتوای فایل
});
```

#### Sync
```js
const fs = require('fs');

try {
    const data = fs.readFileSync('file.txt', 'utf8');
    console.log(data); // محتوای فایل
} catch (err) {
    console.error(err);
}
```

---

### writeFile , writeFileSync

#### Async
```js
const fs = require('fs');

fs.writeFile('file.txt', 'Hello Node.js!', 'utf8', (err) => {
    if (err) throw err;
    console.log('فایل ذخیره شد!');
});
```

#### Sync
```js
const fs = require('fs');

try {
    fs.writeFileSync('file.txt', 'Hello Node.js!', 'utf8');
    console.log('فایل ذخیره شد!');
} catch (err) {
    console.error(err);
}
```