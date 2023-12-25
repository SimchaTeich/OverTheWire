# Level 3 → Level 4

## Level Info
Well done. You’ve moved past an easy substitution cipher.

The main weakness of a simple substitution cipher is repeated use of a simple key. In the previous exercise you were able to introduce arbitrary plaintext to expose the key. In this example, the cipher mechanism is not available to you, the attacker.

However, you have been lucky. You have intercepted more than one message. The password to the next level is found in the file ‘krypton4’. You have also found 3 other files. (found1, found2, found3)

You know the following important details:

* The message plaintexts are in American English (*** very important) - They were produced from the same key (*** even better!)<br />
Enjoy.

## Solution
```
ssh krypton3@krypton.labs.overthewire.org -p 2231
```
```
CAESARISEASY
```
```
cd /krypton/krypton3 ; ls -al
```

Since the additional files (file1, file2 and file3) are quite large, and since there is an emphasis on the source language, my suspicion is that it is a substitution cipher that can be broken by the frequency of the letters. Below is a Python code that prints the number of occurrences of letters for the given file:

```
cd $(mktemp -d)
```
```python
# frequency.py
from string import ascii_uppercase as upper

# Get file content
with open(input('Enter filename: '), 'r') as f:
    content = f.read()

# Create the dict {letter:amount}
d = {l:content.count(l) for l in upper}

# Sorted the dict by amount
print({k:v for k,v in sorted(d.items(), key=lambda x:x[1], reverse=True)})
```

Because it is a given that all the files are from the same key, so in order to be precise in finding the frequency I will combine them:

```
cat /krypton/krypton3/found1 /krypton/krypton3/found2 /krypton/krypton3/found3 /krypton/krypton3/krypton4 > encrypted
```
```
python3 frequency.py
```

![](0.png)

Thh result for the 'foundX' files are:
```python
{'S': 462, 'Q': 341, 'J': 303, 'U': 260, 'B': 249, 'N': 243, 'C': 228, 'G': 228, 'D': 211, 'V': 134, 'Z': 132, 'W': 131, 'M': 88, 'Y': 85, 'T': 75, 'X': 72, 'K': 69, 'E': 64, 'L': 60, 'A': 56, 'F': 28, 'I': 20, 'O': 12, 'H': 4, 'R': 4, 'P': 2}
```

## Password for the next level:
```

```