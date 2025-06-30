- Use dirsearch to discover possible interesting files.

```
dirsearch -u http://10.10.58.13:1337/
```

![[Dirsearch-20250630201717363.webp]]


- Go to http://10.10.58.13:1337/logs.

![[Dirsearch-20250630202023210.webp]]

- Open `app.log`.
	- Inside `app.log`, I found a series of log entries detailing user activity. Notably, there was a POST request to `/index.php` which generated an invite code for the email `alpha@fake.thm`. The code was encoded in Base64:
	 MTM0ODMzNzEyMg==
	- A few minutes later, at `14:36:30`, the user `alpha@fake.thm` deactivated their account. Immediately afterward, at `14:38:40`, a new user `hello@fake.thm` was created.

This suggests that the invite code generation process for `hello@fake.thm` may follow a similar pattern to the one used for `alpha@fake.thm`.

![[Dirsearch-20250630210012665.webp]]





