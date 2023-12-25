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

Since the additional files (file1, file2 and file3) are quite large, and since there is an emphasis on the source language, my suspicion is that it is a substitution cipher that can be broken by the frequency of the letters. Let's build a Python code that prints for each file the frequency of letters in it:

```
cd $(mktemp -d)
```
```python
# vim freq_checker.py
from string import ascii_uppercase

with open(input('Enter filename: '), 'r') as f:
    content = f.read()

freq = {}
for letter in ascii_uppercase:
    freq[letter] = content.count(letter)

# Sorting the letters from high to low frequency.
print(''.join({k: v for k, v in sorted(freq.items(), key=lambda item: item[1], reverse=True)}.keys()))
```

```
python3 freq_checker.py
```

![](0.png)

Now we will find [the frequency](https://pi.math.cornell.edu/~mec/2003-2004/cryptography/subs/frequencies.html) of letters in the American English language:

![](1.png)

```
cat /krypton/krypton3/krypton4
```

![](2.png)

```
rm -r /tmp/tmp.qJqnN1y7I7
```

## Password for the next level:
```

```