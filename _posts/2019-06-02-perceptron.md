---
layout: single
title: "First post"
date: 2019-06-02
tags: [first, machine learning]
header:
  image: "images/mac.jpg"

excerpt: "Machine Learning, Perceptron"
mathjax: "true"
---
How to use md file

# H1 heading

## H2 heading

### H3 heading

this is *italics*
this is **bold**

what about [link](google.com)

bulleted list
* First
+ second
- third

Numbered List
1. hi
2. hello
3. bye

Python code block:
```python
  import numpy as np
  import pandas as pd
  from tensorflow import keras

  def test_function(x, y):
    z = np.sum(x, y)
    return z
```

here is some in-line code `x+y`

Here is an image
<img src="{{ site.url }}{{ site.baseurl }}/images/mac.jpg" alt="mac pic">

Heres kramdown method
![alt]({{ site.url }}{{ site.baseurl }}/images/mac.jpg)

Heres some math:
$$ x + y = z$$

You can also put in in-line like $$a+b=c$$
