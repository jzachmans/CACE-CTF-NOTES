# CTF Notes
- **email address** CustomAccessCertificationEntity26@proton.me
- **git repo:** https://github.com/x509ghost/x509ghost-drop

---


**Prompt & Prelude:**
`You are not worthy to know my full name. The KEY to advance forward IS enough for you to know that I am GHOST. Everything you need is waiting. But first you have to find it. Start here: ====== 90TQjZnSIpFdRNzY2hmMaVTQU5EN5MEZ6lzRh5GbE1UMnNDT0ljMZVXSXR2bShVYulTeMZTTINGMShUY ====== Read it backwards or read it twice. I don't care, it's up to you. — GHOST`

 - Take this string, reverse it, then unbase64 (twice). This gets you the task repo listed above
 ```json
 [
  { "op": "Reverse",
    "args": ["Character"] },
  { "op": "From Base64",
    "args": ["A-Za-z0-9+/=", true, false] },
  { "op": "From Base64",
    "args": ["A-Za-z0-9+/=", true, false] }
]
```
- From task readme, we need to identify the email in which to send the flag reports
- Vegenere decode using "GHOST" as a key, this gets you the email address noted above

`IbglhsHquxyzQwkzptavgawggKuhame26@wfgmuu.aw`

```json
[
  { "op": "Vigenère Decode",
    "args": ["GHOST"] }
]
```

---

### TASK-1
- Curl the domain, get the javascript: https://x509ghost.github.io/x509ghost-drop/declassified/flash.html
- Turn on developer console in browser, execute:
```javascript
const dataUrl = _e.map((b, i) =>
    String.fromCharCode(
        b ^ _k.charCodeAt(i % _k.length)
    )
).join('');

const base64 = dataUrl.split(',')[1];

console.log(base64);
```
```base64
iVBORw0KGgoAAAANSUhEUgAAAewAAAHsAQAAAAA4UaPNAAADCUlEQVR4nO2dXY7bMAyEh0XfpRvk/sfKDcwTsA8k9eNsUaTxxtF6iGDhyPogG6CZMUl7xfCC6a9XaIA4ceLEiRMnTpw4ceIH4NKsQioAQKWP5+A47cDViRM/AS/mBsC2GHG3RzHbYHfZTTtydeLEz8B79FaRChFBMXd4qUCx/bRDVydO/ERcbgYV2wCV+Oxd/RtXJ078m/Hfj0NNuAiAsgGACsomx69OnPj78e7zu+KUAKbVVHyXqHw1belzJ35NHPM9KVzD//UzTDMz25Y+d+LXxCPOj9HbANNJyJgKVOzht+DsgydO/CXcczU9YwPYXcYsZaQu/Za2Hrw6ceLvwlPbuG7xVPwGAH2jiZ/QM/D51DbE18TDpd2Tm7djvBY85re9OU6fJ742PsR5uRkQlakubHLanKX/jIMnTvwZs2YbWkgfw3tUY1Pe+N9Elj534tfEu1zv0iU32mD7Okkd+jzxpfH0ZG+hNDMRcZ3j5oPIq4B5G+LL4l3btI4yIBM4bVfObHMY54kvi0dNSrIO5bUoUzEAWlG2+OumEruOWZ048ffjg56fVX3ct2ZUBzAlLZmrJL443jLzvVXeu8vuItX7KjO6T20JH3HwxIk/Y13buJlWZFNlGxTkJaA1ov0xqxMn/n58qKtuc2ayPAibIVHPe1jiy+L73oPR/8feG4D9NsR/Bp7aphi0oljImGJogqdssdc1zyCEzj544sT/w7J/3tvjVUyrS3rMV4H5hl8d3e2XPnfi18R7nA+XBtzPw6vd/30j++eZnye+Mj70mD1UXccOnDFjD97DEl8fH19Q5k4uEl3EIiI386+eq480/nGrEyd+Al56kA9vRzTSx+OBJau008Pgn3HwxIk/Yf1dH/1tNpnGMaC9vq+Vpajnif88vD/3reLxP5RMlqikturVxx08ceL/si/eY9ZyOLt3HWBovJT4RVj63IlfE5+f6R7bKefibM/YeH2WeRviq+MaaZv2FYikzTS9mD8GzrwN8WXx8Yb0eeP/UCNOnDhx4sSJEydO/Ez8DyiHhCoHijBYAAAAAElFTkSuQmCC
```

- This is a base64 encoded PNG that contains a QR code
- Obtain flag with this recipe:
```json
[
  { "op": "From Base64",
    "args": ["A-Za-z0-9+/=", true, false] },
  { "op": "Render Image",
    "args": ["Raw"] },
  { "op": "Parse QR Code",
    "args": [false] }
]
```

---
### TASK-2

