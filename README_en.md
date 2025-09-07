# JG-Cursor Activation Tool Cracking Attempt

[简体中文](README.md) | **English**

> 📖 **Tool Usage Instructions**: [Click to view detailed usage instructions and feature introduction](README_backup.md)

> ⚠️ **Disclaimer**: This project is for educational and research purposes only. Do not use it for commercial purposes. Users are responsible for any consequences arising from the use of this tool.

## I. Origin

Cursor's renewal requires many email addresses for continuous registration, and maintaining email addresses myself is a bit troublesome. A friend bought an unlimited renewal tool from Taobao. The advantage is that the merchant created a client that encapsulates all the logic for switching email accounts, so you don't have to worry about it yourself. The merchant's after-sales service is also very good. Overall, it's quite cost-effective. He shared the activation code he bought with me, and I tried it first. During the trial, I discovered a small problem: this tool uses activation codes for activation, but the activation codes are bound to the machine:

![image-20250419032830941](./docs/%E6%9E%81%E5%85%89Cursor%E6%BF%80%E6%B4%BB%E5%B7%A5%E5%85%B7%E7%A0%B4%E8%A7%A3%E5%B0%9D%E8%AF%95.assets/image-20250419032830941.png)

As shown above, the activation code is bound to the machine code. If the activation code doesn't match the already bound machine code, it cannot be used. When refreshing requests, it will report an error indicating that the machine code is incorrect. Then I scratched my head and thought that since this is a client, I should be able to manually modify the machine code it calculates. As long as I modify the machine code to be consistent with my friend's machine code, I should also be able to use his activation code.

## II. Unpacking and Reverse Engineering

This client looks like it has a strong Electron style. I'm on a Mac system, so next I'll introduce how to reverse engineer Electron programs on Mac.

Client download link: https://bwcxynefwek.feishu.cn/docx/XgtZdePyEoarjXxp9eLciNZFnkd

In the data folder under the project root directory, there is all the client code that has been reverse engineered, which you can also refer to for understanding.

On Mac, right-click on the executable file icon and select "Show Package Contents":

![image-20250419033559093](./docs/%E6%9E%81%E5%85%89Cursor%E6%BF%80%E6%B4%BB%E5%B7%A5%E5%85%B7%E7%A0%B4%E8%A7%A3%E5%B0%9D%E8%AF%95.assets/image-20250419033559093.png)

This will open the package directory of this Electron file:

![image-20250419033810077](./docs/%E6%9E%81%E5%85%89Cursor%E6%BF%80%E6%B4%BB%E5%B7%A5%E5%85%B7%E7%A0%B4%E8%A7%A3%E5%B0%9D%E8%AF%95.assets/image-20250419033810077.png)

Then open a Terminal, drag the Contents folder above to Terminal to see the absolute path of this folder, then enter this directory:

```bash
(base) ➜  Contents cd /Users/cc11001100/Downloads/JG-Cursor-1.4.0-mac.app/Contents 
(base) ➜  Contents pwd
/Users/cc11001100/Downloads/JG-Cursor-1.4.0-mac.app/Contents
(base) ➜  Contents 
```

Let's see what files are in this directory:

```bash
(base) ➜  Contents ls -l 
total 16
drwxr-xr-x@ 10 cc11001100  staff   320  3 10 22:41 Frameworks
-rw-r--r--@  1 cc11001100  staff  2947  3 10 22:41 Info.plist
drwxr-xr-x@  3 cc11001100  staff    96  3 10 22:41 MacOS
-rw-r--r--@  1 cc11001100  staff     8  3 10 22:41 PkgInfo
drwxr-xr-x@ 86 cc11001100  staff  2752  4 19 01:09 Resources
```

The `Resources` directory is where Electron application's own code logic is stored. The other directories are framework code itself, which we can ignore for now in this example. Enter `Resources` and see what files are there:

```bash
(base) ➜  Contents cd Resources
(base) ➜  Resources ls -l 
total 162960
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 af.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 am.lproj
-rw-r--r--@ 1 cc11001100  staff  3203416  4 19 01:09 app.asar
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 ar.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 bg.lproj
drwxr-xr-x@ 4 cc11001100  staff      128  3 10 22:41 bin
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 bn.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 ca.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 cs.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 da.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 de.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 el.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 en.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 en_GB.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 es.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 es_419.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 et.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 fa.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 fi.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 fil.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 fr.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 gu.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 he.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 hi.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 hr.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 hu.lproj
-rw-r--r--@ 1 cc11001100  staff    45448  3 10 22:41 icon.icns
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 id.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 it.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 ja.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 kn.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 ko.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 lt.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 lv.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 ml.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 mr.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 ms.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 nb.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 nl.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 pl.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 pt_BR.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 pt_PT.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 ro.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 ru.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 sk.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 sl.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 sr.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 sv.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 sw.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 ta.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 te.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 th.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 tr.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 uk.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 ur.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 vi.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 zh_CN.lproj
drwxr-xr-x@ 2 cc11001100  staff       64  3 10 22:41 zh_TW.lproj
```

Among the many files below, we need to focus on `app.asar`. This file can be understood as a compressed package that contains the packaged logic of the Electron application itself. However, it uses a custom compression format when compressing, so we need to install its packaging tool to decompress it:

```bash
npm install -g asar
```

If you have already installed `cnpm`, you can use `cnpm` to install:

```bash
npm install -g asar
```

After installation, verify if the installation was successful:

```bash
(base) ➜  Resources asar --version
v3.3.1
```

I won't elaborate on the asar packaging format here. You can understand it as similar to zip format. Here you just need to understand that the asar command can unpack and repack. We use asar to unpack the app.asar file:

```bash
asar extract app.asar app-extract
```

Now enter the app-extract directory and see what files have been unpacked:

```bash
(base) ➜  app-extract ls -l 
total 8
drwxr-xr-x   5 cc11001100  staff  160  4 19 03:49 dist
drwxr-xr-x  26 cc11001100  staff  832  4 19 03:49 node_modules
-rw-r--r--   1 cc11001100  staff  388  4 19 03:49 package.json
```

The file layout above looks very familiar. This looks like a frontend project layout. `package.json` contains project metadata and dependency declarations. Let's look at the content:

```json
{
  "name": "free-cursor",
  "version": "1.4.0",
  "description": "A helper tool for Cursor",
  "author": "Dreamease",
  "main": "dist/main/main.js",
  "dependencies": {
    "electron-is-dev": "^2.0.0",
    "electron-store": "8.1.0",
    "uuid": "^9.0.1"
  },
  "config": {
    "electron_mirror": "https://npmmirror.com/mirrors/electron/",
    "electron_custom_dir": "{{ version }}"
  }
}
```

The `node_modules` folder contains specific dependencies. Since this is a black hole, we won't show its contents.

![image-20250419035316110](./docs/%E6%9E%81%E5%85%89Cursor%E6%BF%80%E6%B4%BB%E5%B7%A5%E5%85%B7%E7%A0%B4%E8%A7%A3%E5%B0%9D%E8%AF%95.assets/image-20250419035316110.png)

The dist directory looks familiar because this directory contains the packaged Electron application code. Let's enter and take a look:

```bash
(base) ➜  app-extract cd dist 
(base) ➜  dist ls -l 
total 0
drwxr-xr-x  3 cc11001100  staff   96  4 19 03:49 main
drwxr-xr-x  3 cc11001100  staff   96  4 19 03:49 preload
drwxr-xr-x  4 cc11001100  staff  128  4 19 03:49 renderer
```

Entering the dist directory, this is a typical Electron application file layout:

- The renderer directory contains frontend interface files
- preload bridges frontend and backend
- main contains backend logic

What we want to modify is the machine code, which is definitely collected and generated by the backend and passed to the frontend for display. So we focus on the logic in the main file. Enter this folder:

```bash
(base) ➜  dist cd main 
(base) ➜  main ls -l 
total 32
-rw-r--r--  1 cc11001100  staff  15345  4 19 03:49 main.js
```

Very simple, just a single js file. Note the size of this js file:

```bash
(base) ➜  main ls -lh main.js 
-rw-r--r--  1 cc11001100  staff    15K  4 19 03:49 main.js
```

This single file is 15KB in size and contains a lot of logic. Now that technology has advanced, we don't need to live primitively anymore. I directly used Cursor to format and add comments to the logic in this large file, and found the function related to generating device codes:

```js
function P() {
    return (P = r(function () {
        var e, n, s, o, a, i;
        return t(this, function (c) {
            switch (c.label) {
                case 0:
                    var u;
                    // Check if deviceId already exists, return directly if it does
                    if (e = M.get("deviceId")) return [2, e];
                    // Define function to get MAC address
                    u = r(function () {
                        return t(this, function (e) {
                            return [2, new Promise(function (e, r) {
                                var n = "";
                                // Choose different commands based on different platforms to get MAC address
                                switch (process.platform) {
                                    case"win32":
                                        n = "getmac";
                                        break;
                                    case"darwin":
                                        n = "ifconfig";
                                        break;
                                    case"linux":
                                        n = "ip link";
                                        break;
                                    default:
                                        r(Error("Unsupported platform"));
                                        return
                                }
                                // Execute command to get MAC address
                                (0, v.exec)(n, function (n, t) {
                                    if (n) {
                                        r(n);
                                        return
                                    }
                                    var s, o = "";
                                    // Parse command output to get MAC address based on different platforms
                                    if ("win32" === process.platform) o = (null === (s = t.split("\n")[3]) || void 0 === s ? void 0 : s.split(" ")[0]) || ""; else if ("darwin" === process.platform) {
                                        var a = t.match(/ether\s+([0-9a-fA-F:]+)/);
                                        o = (null == a ? void 0 : a[1]) || ""
                                    } else if ("linux" === process.platform) {
                                        var i = t.match(/link\/ether\s+([0-9a-fA-F:]+)/);
                                        o = (null == i ? void 0 : i[1]) || ""
                                    }
                                    // Return error if no MAC address found
                                    if (!o) {
                                        r(Error("No MAC address found"));
                                        return
                                    }
                                    e(o)
                                })
                            })]
                        })
                    }), n = function () {
                        return u.apply(this, arguments)
                    }, c.label = 1;
                case 1:
                    return c.trys.push([1, 3, , 4]), [4, n()];
                case 2:
                    // Get MAC address and generate device ID using SHA256 hash
                    return s = c.sent(), o = "".concat(s).concat("your-secret-salt"), a = b.default.createHash("sha256").update(o).digest().toString("base64").replace(/\+/g, "-").replace(/\//g, "_").replace(/=/g, "").slice(0, 16), M.set("deviceId", a), [2, a];
                case 3:
                    // If getting MAC address fails, generate device ID using UUID
                    return c.sent(), i = (0, E.v4)().slice(0, 16), M.set("deviceId", i), [2, i];
                case 4:
                    return [2]
            }
        })
    })).apply(this, arguments)
}
```

Spoiled by AI, I no longer have the patience to read code. I directly asked Deepseek to summarize:

### **Device Code Generation Rules**

1. **Priority Cache Reading**: If `deviceId` already exists, return directly.
2. **Generation Method**:
   - **Primary Method (MAC Address)**:
     - Get MAC address through system commands (Windows: `getmac`, macOS: `ifconfig`, Linux: `ip link`).
     - Concatenate fixed salt value (`"your-secret-salt"`), use SHA256 hash + Base64 encoding + character replacement (`+→-` `/→_` remove `=`), take first 16 characters.
   - **Backup Method (UUID)**: If getting MAC fails, generate random UUID and take first 16 characters.
3. **Store Result**: Generated `deviceId` is cached to avoid repeated generation.

**Features**: Cross-platform, hardware binding priority, uniqueness guarantee.

Oh, so we just need to modify the part that checks if deviceId already exists, make it always return true, and then return our given machine code. To minimize impact on other code logic, we won't format it. We'll use deviceId as a keyword and modify and replace all the code logic around it. This needs to be planned in advance based on the formatted code, otherwise it's easy to make mistakes:

![image-20250419041646717](./docs/%E6%9E%81%E5%85%89Cursor%E6%BF%80%E6%B4%BB%E5%B7%A5%E5%85%B7%E7%A0%B4%E8%A7%A3%E5%B0%9D%E8%AF%95.assets/image-20250419041646717.png)

After replacement, we need to repackage the modified code and replace the original app.asar file for it to take effect. Let's first backup the original app.asar file in case we need to recover if problems occur:

```bash
cp app.asar app.asar.bak
```

Then delete the app.asar file:

```bash
rm app.asar
```

Then use the directory we just modified to repackage a new app.asar file:

```bash
asar pack app-extract app.asar
```

Then restart this Cursor renewal client, and find that the machine code has been recognized as what we just set:

![image-20250419042202018](./docs/%E6%9E%81%E5%85%89Cursor%E6%BF%80%E6%B4%BB%E5%B7%A5%E5%85%B7%E7%A0%B4%E8%A7%A3%E5%B0%9D%E8%AF%95.assets/image-20250419042202018.png)

At this point, we achieved using one activation code on multiple machines. We just need to unpack and modify the machine code to be consistent with the originally bound machine code. So for the next few weeks, I happily freeloaded off my friend's activation code until Cursor started strengthening risk control.

## III. Another Village After Dark Willows and Bright Flowers

Probably because I was freeloading too hard, Cursor's official website started strengthening risk control. Suddenly one day, the unlimited renewal didn't work. Then I joined the merchant's customer service group. In the customer service group, many people posted screenshots to report problems. Some people included their activation codes and machine codes in screenshots when reporting problems:

![image-20250419042657749](./docs/%E6%9E%81%E5%85%89Cursor%E6%BF%80%E6%B4%BB%E5%B7%A5%E5%85%B7%E7%A0%B4%E8%A7%A3%E5%B0%9D%E8%AF%95.assets/image-20250419042657749.png)

emm...?

![image-20250419042735101](./docs/%E6%9E%81%E5%85%89Cursor%E6%BF%80%E6%B4%BB%E5%B7%A5%E5%85%B7%E7%A0%B4%E8%A7%A3%E5%B0%9D%E8%AF%95.assets/image-20250419042735101.png)

Probably because of this sentence, many people later reported problems without any censoring and posted freely in the group.

Although I had already purchased a new activation code at this time, these activation codes didn't mean much to me. But as a otaku with a collecting habit, how could I resist not collecting useful things? Then I collected a bunch of machine codes and activation codes like harvesting vegetables. However, after verification, some were abandoned:

- I estimate some people got refunds directly after reporting problems, so the activation codes were abandoned
- Some activation codes contained characters like iL1, which are difficult to distinguish with OCR recognition. I tried exhaustive combinations several times and gave up, really couldn't figure out whether it was 1 or l

My friend shared an activation code with me. To reciprocate, I gave back a bunch of activation codes, borrowing flowers to offer to Buddha hahaha:

![image-20250419043101816](./docs/%E6%9E%81%E5%85%89Cursor%E6%BF%80%E6%B4%BB%E5%B7%A5%E5%85%B7%E7%A0%B4%E8%A7%A3%E5%B0%9D%E8%AF%95.assets/image-20250419043101816.png)

Of course, we didn't actually use these, just collected them. During this period, because I frequently involved Electron unpacking and replacement when verifying, I wrote a tool specifically to help me replace machine codes automatically to free my hands:

```text
https://github.com/cursor-home/JG-Cursor-cracker
```

![image-20250419043545766](./docs/%E6%9E%81%E5%85%89Cursor%E6%BF%80%E6%B4%BB%E5%B7%A5%E5%85%B7%E7%A0%B4%E8%A7%A3%E5%B0%9D%E8%AF%95.assets/image-20250419043545766.png)

Modifying the machine code of the Cursor activator:

![image-20250419043653276](./docs/%E6%9E%81%E5%85%89Cursor%E6%BF%80%E6%B4%BB%E5%B7%A5%E5%85%B7%E7%A0%B4%E8%A7%A3%E5%B0%9D%E8%AF%95.assets/image-20250419043653276.png)

With this tool as assistance, I verified and logged into several machine code + activation code combinations:

![image-20250419011143493](./docs/%E6%9E%81%E5%85%89Cursor%E6%BF%80%E6%B4%BB%E5%B7%A5%E5%85%B7%E7%A0%B4%E8%A7%A3%E5%B0%9D%E8%AF%95.assets/image-20250419011143493.png)

There are also those that are purchased with quota-based payment. It seems this merchant's business is quite large, with all kinds of SKUs:

![image-20250419010243765](./docs/%E6%9E%81%E5%85%89Cursor%E6%BF%80%E6%B4%BB%E5%B7%A5%E5%85%B7%E7%A0%B4%E8%A7%A3%E5%B0%9D%E8%AF%95.assets/image-20250419010243765.png)

## IV. Happy End

After a period of freeloading, I found Cursor very useful. Switching accounts back and forth was also troublesome, so I decided to pay for it.

## V. Bad End

Then after using paid Cursor for a while, I discovered this product has a very magical business logic.

Its operation depends on large model computing power, and with limited computing power, it prioritizes user requests with different response priorities for different users.

Its resource limitation is roughly:

```text
Paid user fast request > Free user >= Paid user slow request > Paid user slow request reaching a certain threshold
```

That is, if you are a paid user, after using it for a while, your user experience will be very poor, far worse than the resource allocation priority of freeloading. This is the first time I've encountered treating paid users like this. I've decided to continue freeloading...

# VI. Truth Cannot Be Hidden

I found that when customer service in the group handles problems, they are already explicitly saying not to screenshot device numbers. I estimate this cracker's cracker may have been privately spread to their hands... So I directly made this project public and open source, just sharing experiences for everyone's entertainment.

![](./docs/极光Cursor激活工具破解尝试.assets/2ebc1382-f773-4e4b-b099-dd053ffd9a08.png)

---

> If you found this interesting after reading, please give it a Star ⭐️, thank you very much!