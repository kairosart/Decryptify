Visit http://10.10.246.99:1337/.


![[Nmap-20250625201412741.webp]]

## The API.js file

Once you land on the page, you can see two login forms.
- One that requires a username and an invite code.
- Other requires an email instead of the username. 

Another interesting thing was the API documentation page, which you can find in the footer of the login page, but unfortunately this is password protected.  
  
After digging into the source code of the page you'll finid an interesting file, API.js.

![[Web server-20250625202951505.webp]]

```
function b(c, d) {
  const e = a();
  return b = function (f, g) {
    f = f - 357;
    let h = e[f];
    return h;
  },
  b(c, d);
}
const j = b;
function a() {
  const k = [
    '16OTYqOr',
    '861cPVRNJ',
    '474AnPRwy',
    'H7gY2tJ9wQzD4rS1',
    '5228dijopu',
    '29131EDUYqd',
    '8756315tjjUKB',
    '1232020YOKSiQ',
    '7042671GTNtXE',
    '1593688UqvBWv',
    '90209ggCpyY'
  ];
  a = function () {
    return k;
  };
  return a();
}(
  function (d, e) {
    const i = b,
    f = d();
    while (!![]) {
      try {
        const g = parseInt(i(363)) / 1 + - parseInt(i(367)) / 2 + parseInt(i(359)) / 3 * (parseInt(i(362)) / 4) + parseInt(i(364)) / 5 + parseInt(i(360)) / 6 * (parseInt(i(357)) / 7) + - parseInt(i(358)) / 8 * (parseInt(i(366)) / 9) + parseInt(i(365)) / 10;
        if (g === e) break;
         else f['push'](f['shift']());
      } catch (h) {
        f['push'](f['shift']());
      }
    }
  }(a, 934896)
);
const c = j(361);

```

This code is **obfuscated JavaScript**. It defines a function `b` that is redefined internally to decode an array of strings via numeric offsets. Then, it runs a loop to reorder the array to match a checksum (934896). Finally, it tries to access and log a decoded string.

Let’s walk through and **deobfuscate** it step-by-step.

---

### 🧠 Summary of the Logic

1. `a()` returns a scrambled array of strings.
    
2. `b(c, d)` uses that array and some math to map a number to a string.
    
3. An IIFE (Immediately Invoked Function Expression) is used to reorder the array based on a checksum.
    
4. Then `j(361)` is used to retrieve a value from the now-reordered array. 
### Run the code on a browser console

- Open  Developer Console in your browser.
- Copy the previous code.
- Add a line with: `console.log()`
- Run the code with ENTER.


![[Web server-20250625211303324.webp]]

The result is `H7gY2tJ9wQzD4rS1`,

**Next step:** [[Api.php]]



