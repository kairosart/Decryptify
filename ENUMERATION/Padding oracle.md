- Check the `/dashboard.php` code page.
```
HTML
<form method="get">
    <input type="hidden" name="date" value= "/RozKXrvotl7sbZYeqduzy8D3CpBUPTbj/OuQVvAI+o=">
```

Noticed that the server response included a hidden input field named `date` containing a weird value. `/RozKXrvotl7sbZYeqduzy8D3CpBUPTbj/OuQVvAI+o=`.

> [!Note] 
The value you see here changes any time you refresh the page, for this reason you will see different values in the rest of the writeup_

- If you remove the value of the `date` parameter, the server respond with the following error:
```
Padding error: error:0606506D:digital envelope routines:EVP_DecryptFinal_ex:wrong final block length
```

This error is a strong indicator that the application is decrypting the `date` parameter using a block cipher mode with padding (likely AES in CBC mode), and the decryption failed due to invalid padding. Specifically, `EVP_DecryptFinal_ex` is part of OpenSSL's API, and this kind of error often arises when the ciphertext’s padding is tampered with.

This behavior suggests a potential **Padding Oracle vulnerability**, where the server leaks information about whether the padding is correct during decryption. If exploitable, it could allow an attacker to decrypt data without knowing the encryption key.

**Next step:** [[Exploiting the Padding Oracle for RCE]]
