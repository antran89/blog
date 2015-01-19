---
layout: article
title: "Ipython Notebook Debugging"
date: 2015-01-17T22:30:21-0800
modified:
categories: notes
comments: true
excerpt:
tags: [ipython, notebook, qtconsoel]
ads: true
image:
  feature:
  teaser:
---

Ipython notebook is very convenient since it allows to run a portion of a code yet debugging is very inconvenient. The debugging procedure that I’m using is combination of ipython + qtconsoel + pdb.

When you first open qtconsole using the line magic function `%qtconsole` on ipython notebook, it provides interactive console window. Yet this does not allow putting break point on the code. Instead you can enable pdb calling using `%pdb` line magic function. For example, in ipython notebook or the qtconsole connected to the notebook type `%pdb`.

{% highlight python %}
>> %pdb
Automatic pdb calling has been turned ON

>> , grad_vectorized = svm_loss_vectorized(W, X_train, y_train, 0.00001)
---------------------------------------------------------------------------
AttributeError                            Traceback (most recent call last)
<ipython-input-91-64e5142682af> in <module>()
----> 1 _, grad_vectorized = svm_loss_vectorized(W, X_train, y_train, 0.00001)

/home/chrischoy/Dropbox/Stanford/2014_15/CS231N/assignment1/cs231n/classifiers/linear_svm.py in svm_loss_vectorized(W, X, y, reg)
     99   X_mask = (WX > 0)
    100   X_mask[y, range(num_train)] -= np.sum(X_mask, axis=0)
--> 101   import pdb; pdb.set_trac()
    102   X_mask = X_mask.reshape(num_classes, 1, num_train)
    103   dW = np.sum(X_mask * X.reshape(1, num_features, num_train), axis=-1) / num_train

AttributeError: 'module' object has no attribute 'set_trac'

(Pdb) 

>> %pdb
Automatic pdb calling has been turned OFF
{% endhighlight %}


You can use interactive pdb to print variables, run python commands or use all standard pdb commands (step, up, down, clear, break, quit).
Once you are done, you can switch off the automatic pdb calling mode by typing `%pdb` again.

