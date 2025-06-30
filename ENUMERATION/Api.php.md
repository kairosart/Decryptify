- Visit http://10.10.58.13:1337/api.php.
- Introduce the password from the previous step.
- 

![[Api.php-20250630200435951.webp]]

This function generates a invite_code against a user email.

It looks like you can forge our own invite code! This is because `mt_srand()` will always output the same number for a given seed. To get the seed we need a constant value and an email?  It needs to be registered first.

**Next step:** [[Dirsearch]]
