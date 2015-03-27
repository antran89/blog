---
layout: article
title: "Torch Notes"
date: 2015-03-25T12:07:32-0700
modified:
categories: research
excerpt:
comments: true
tags:
toc: true
ads: true
image:
  feature:
  teaser:
---

iTorch: iPython Notebook like dev environment.

## Lua basic

- Javascript like syntax
- By default, all variables are global (local if you want to define locally)
- 1-base indexing!
- one data structure: table: {}
- No class definition, it only has the `function` type
- lua socket programming [link](http://w3.impa.br/~diego/software/luasocket/home.html)

## Torch Basic
- ♯ is the length operator i.e. ♯b returns the size of a table b
- `a:storage()` will return the serialized `a`
- No copy, it only passes pointer of the variable (or with offset)
- Python numpy like indexing i.e. `a[2]` will return the second row which is `a:select(1,2)`
- Reshape using `b:unfold(1, 5, 2)`, also it can do the transpose as well if you define the dimension to be different.
- Assign memroy first before creating a temp variable
- Can pass the output pointer as the first argument
    {% highlight lua %}
    c = torch.Tensor(n,m)
    c.mm(a,b)

    c = torch.mm(a,b)
    torch.mm(c,a,b)
    {% endhighlight %}
- Use MKL blas library 
- Transpose `A:t()`
- Function definition, Python like definition `function` `return` `end`
- nn has three types of net structures: Sequential, Concat, Parallel
- Recurrent neural network extension [link](https://github.com/clementfarabet/lua---nnx)
- `criterion` defines the loss function. You compute `gradient` using `criterion:backward(output...)`
- package `optim` supports L-BFGS or other variations of gradient descent
- Nice LSTM for natural language processing from NVIDIA using Torch [link](http://devblogs.nvidia.com/parallelforall/understanding-natural-language-deep-neural-networks-using-torch/)
- `setmetatable` is an easy way to override index operator
    {% highlight lua %}
    { __index = function(i)...}
    {% endhighlight %}
- To transfer data to GPU, use `:cuda()`


