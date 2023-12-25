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

So, The result for the 'foundX' files are:
```python
{'S': 462, 'Q': 341, 'J': 303, 'U': 260, 'B': 249, 'N': 243, 'C': 228, 'G': 228, 'D': 211, 'V': 134, 'Z': 132, 'W': 131, 'M': 88, 'Y': 85, 'T': 75, 'X': 72, 'K': 69, 'E': 64, 'L': 60, 'A': 56, 'F': 28, 'I': 20, 'O': 12, 'H': 4, 'R': 4, 'P': 2}
```

And you can read [here](https://mathcenter.oxford.emory.edu/site/math125/englishLetterFreqs/) about frequencies and find out the following:

![](1.png)

Can I guess the key? Maybe. But the amount here is no enough.. so we can use in this also:

![](2.png)

In the following Python code we will try to decrypt the key. Little by little he will improve:

```python
# decrypt.py

# this two lines (together) may be the key:
enc_alphabet  = 'SQJUBNCGDVZWMYTXKELAFIOHRP'
dec_frequency = 'ETAOINSHRDLCUMWFGYPBVKJXQZ'

encrypted = input('Enter encrypted text: ')
plaintext = ''

for l in encrypted:
    if l.isupper():
        plaintext += dec_frequency[enc_alphabet.find(l)]

print("Plaintext:\n" + plaintext)
```

![](3.png)

No sense (even in the `foundX` files...). But Im sure that `S` is `E` in the plaintext. We will now search by common trigrams:

```python
# frequency_trigrams.py
from string import ascii_uppercase as upper

# Get file content
with open(input('Enter filename: '), 'r') as f:
    content = f.read().replace(' ', '')

# Create the dict {trigram:amount}
d = {l1+l2+l3:content.count(l1+l2+l3) for l1 in upper for l2 in upper for l3 in upper}

# Sorted the dict by amount
print({k:v for k,v in sorted(d.items(), key=lambda x:x[1], reverse=True) if v > 1})
```

```
python3 frequency_trigrams.py
```

![](4.png)

It is possible that the encrypted word `JDS` is actually the word `THE`, and `JDQ` is `THA` Let's check it out:

```python
# decrypt2.py

# this two lines (together) may be the key:
enc_alphabet  = 'SJQUBNCDGVZWMYTXKELAFIOHRP'
dec_frequency = 'ETAOINSHRDLCUMWFGYPBVKJXQZ'

encrypted = input('Enter encrypted text: ')
plaintext = ''

for l in encrypted:
    if l.isupper():
        plaintext += dec_frequency[enc_alphabet.find(l)]

print("Plaintext:\n" + plaintext)
```
```
cat /krypton/krypton3/krypton4 | python3 decrypt2.py | grep THE
```

![](5.png)

Maybe the word `DEKED` is actualy `LEVEL`?
The encrypted password is `KSVVWBGS JDS VSISV XBMNYQUUKBNWCUANMJS`, so it means `V` is `L` and `I` is `V`

```python
# decrypt3.py

# this two lines (together) may be the key:
enc_alphabet  = 'SJQUBNCDGZVWMYTXKELAIFOHRP'
dec_frequency = 'ETAOINSHRDLCUMWFGYPBVKJXQZ'

encrypted = input('Enter encrypted text: ')
plaintext = ''

for l in encrypted:
    if l.isupper():
        plaintext += dec_frequency[enc_alphabet.find(l)]

print("Plaintext:\n" + plaintext)
```

![](6.png)

It's nice but still not very clear. In level `Level1 to Level2` the decrypted text was `LEVEL TWO PASSWORD ...`. Maybe here is `...THE LEVEL FOUR PASSWORD...`.
It can be work becaues the `OO` in the place of `SS`.

So, the encrypted password is `KSVVWBGS JDS VSISV XBMN YQUUKBNW CUANMJS`. It means that we can fix the **encryption** key with: `O->B`, `R->N`, `P->Y`, `S->U`, `W->K`, `D->W`

```python
# decrypt4.py

# this two lines (together) may be the key:
enc_alphabet  = 'SJQBCGUDNWVZMLKXTEYAIFOHRP'
dec_frequency = 'ETAOINSHRDLCUMWFGYPBVKJXQZ'

encrypted = input('Enter encrypted text: ')
plaintext = ''

for l in encrypted:
    if l.isupper():
        plaintext += dec_frequency[enc_alphabet.find(l)]

print("Plaintext:\n" + plaintext)
```

![](7.png)

`WELL DONE THE LEVEL FOUR PASSWORD IS BRUTE`.

This does not mean that we have managed to fully discover the whole key, but we will stop here.

Note: the whole challenge could be done without effort [here](https://www.dcode.fr/monoalphabetic-substitution), once we understood that it was a substitution cipher. All that needs to be done is to connect the contents of the four files (3 `foundX` and the password file `krypton4`) to the website. Exactly for the same reason that we combined them into an `encrypted` file.

```
exit
```
```
rm -r /tmp/tmp.egboJUMqrj
```

## Password for the next level:
```
BRUTE
```