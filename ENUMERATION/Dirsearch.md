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

## Reverse Engineering Invite Code Generation



## Generating constant value

- Create file called `constant_value.php`.
- Copy the following code and save it.

```
<?php

// Token generation example

function calculate_seed_value($email, $constant_value) {
    $email_length = strlen($email);

    // Take the first 8 characters, convert each char to ASCII hex and concatenate
    $email_substr = substr($email, 0, 8);
    $email_hex_str = '';
    for ($i = 0; $i < strlen($email_substr); $i++) {
        $email_hex_str .= dechex(ord($email_substr[$i]));
    }
    
    // Convert the concatenated hex string to a decimal number
    $email_hex = hexdec($email_hex_str);

    // Sum length + constant + hex value (all decimal numbers)
    $seed_value = $email_length + $constant_value + $email_hex;

    return $seed_value;
}

// Example usage
$email = "user@example.com";
$constant_value_base64 = "MTM0ODMzNzEyMg==";  // The base64 string
$constant_value_str = base64_decode($constant_value_base64); // decode to string
$constant_value = (int)$constant_value_str; // convert string to integer

// Now use $constant_value as a number
$seed_value = calculate_seed_value($email, $constant_value);
mt_srand($seed_value);
$random = mt_rand();

// Convert the integer to a binary string, then base64 encode
$random_bytes = pack('N', $random); // 32-bit unsigned long (big-endian)
$invite_code = base64_encode($random_bytes);

echo "Invite code: " . $invite_code . "\n";
?>

```

- Run the code.

```
php constant_value.php
```

