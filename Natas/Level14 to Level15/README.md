# Level 14 → Level 15

## Details
Username: `natas15`<br />
Password: `TTkaI7AWG4iDERztBcEyKV7kRXH1EZRB`<br />
URL:      http://natas15.natas.labs.overthewire.org

## Solution
<img src="./0.png"></img>

<img src="./1.png"></img>

We can edit the query by entering the username. But no matter what we inject the only output will be an indication of the user's existence. Therefore we can try to "guess" the password, and if the output is "existing user" then it is correct.

Username: `natas16`:

<img src="./2.png"></img>
<img src="./3.png"></img>

We will check that the length of the password is indeed 32 characters, like all the passwords so far.<br />
Username: `natas16" and password like "________________________________";#`
<img src="./4.png"></img>
<img src="./5.png"></img>

Now we will compile a script that Brute Force the password, character by character:

```python

```

## Password for the next level:
```

```
