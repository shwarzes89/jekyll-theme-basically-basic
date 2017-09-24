---
layout: post
title: ImportError on Mac OS Crypto
description: "ImportError on Mac OS: No module named 'Crypto'"
tags: [python, pycrypto]
---

if you install pycrypto through

`pip3 install pycrypto`

Even if you install through
`ImportError: No module named 'Crypto'` error occurs.
The solution for this is as follows.

```python
import crypto
import sys
sys.modules['Crypto'] = crypto
```
