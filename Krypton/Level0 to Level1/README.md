# Level 0 → Level 1

## Level Info
Welcome to Krypton! The first level is easy. The following string encodes the password using Base64:

**S1JZUFRPTklTR1JFQVQ=**<br />
Use this password to log in to krypton.labs.overthewire.org with username krypton1 using SSH on port 2231. You can find the files for other levels in /krypton/

## Solution
```python
from base64 import b64decode as d
print(d('S1JZUFRPTklTR1JFQVQ='))
# Output: b'KRYPTONISGREAT'
```

## Password for the next level:
```
KRYPTONISGREAT
```
