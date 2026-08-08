# OffSec Proving Grounds Writeups

Hands on writeups from OffSec's Proving Grounds practice labs. Each entry documents full enumeration, the exploitation path that led to a foothold, privilege escalation to root, and the mitigations that would have stopped it, mapped to CWE and MITRE ATT&CK where relevant.

Written by Alexander Lavrinenko — [Portfolio](https://cyberlav.io) · [LinkedIn](https://www.linkedin.com/in/alexander-lavrinenko-80996950/)

## Structure

Each writeup lives in its own directory with a `README.md` and an `images/` folder containing the original, full resolution screenshots referenced in the writeup, in the order they appear.

```
<machine-slug>/
  README.md
  images/
    <machine-slug>-01.png
    <machine-slug>-02.png
    ...
```

Every screenshot is followed by an italic caption on its own line, with a blank line between the two. That blank line matters: it forces the image and the caption into separate paragraphs, so the caption renders stacked underneath the image on every markdown viewer instead of floating beside it.

```
![alt text](images/<machine-slug>-01.png)

*Caption text.*
```
