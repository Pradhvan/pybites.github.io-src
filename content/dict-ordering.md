Title: How to Order Dict Output in Python
Date: 2017-02-01 11:30
Category: Tips
Tags: python, tips, tricks, code, pybites, dicts, data-structures
Slug: dict-ordering
Authors: Julian
Summary: Learn how to order the output of a Python Dict
cover: images/featured/dict-ordering.png
Status: Draft

Dicts are awesome, even for a beginner like me. What isn't so awesome is trying to figure out how to list out their contents for the first time! Lists are easy enough but how on earth do you list out the key/value contents of a dict, let alone in any sort of order?

##Listing the Contents of a Dict

Let's start by simply listing out the dict contents. In the below example I have a dict stored in the *ages* variable.

> Disclaimer: I'm not having a mid-life crisis. I'm quite aware that I'm no longer 20.

> Disclaimer 2: I'm being generous when I say Bob is 23. Sorry Bob!

~~~~
>>> ages = {'julian': 20, 'bob': 23, 'zack': 3, 'anthony': 95, 'daniel': 41}
>>>
>>> for k, v in ages.items():
    print(k, v)

julian 20
bob 23
zack 3
anthony 95
daniel 41
>>>
~~~~

1. First we create the dict. For the sake of this example I've made sure the keys and the values are not in alphabetical or numerical order.

2. The for loop iterates over the keys, *k* and values, *v* in *ages.items*. Each key/value pair in a dict is called an **item** thus we use .items().

3. We print the key and value.

4. Note that the output is in the same "order" as it was inside the dict. It wasn't automatically ordered alphabetically or numerically.


##Using a Lambda to Order the Output in Alphabetical Order

> If you're unsure of what a Lambda is, I strongly urge you to read [this article by Dan Bader](https://dbader.org/blog/python-lambda-functions). It was my source for learning what they were.

The previous output is great but what if I wanted to print the *ages* data in alphabetical order? Not only do I need to sort it by the letter but also make sure I point my sorting method at the **key** in the dict. I can do this with a lambda!

First, let's sort it alphabetically with the help of a lambda:

~~~~
>>> sorted(ages.items(), key=lambda x: x[0])
[('anthony', 95), ('bob', 23), ('daniel', 41), ('julian', 20), ('zack', 3)]
~~~~

1. First, note that we're going to use *sorted*. This will sort everything between the () in ascending order. (Run *help(sorted)* to see the available options to *sorted*. You'll see that we can specify a key function to help sort the data).

2. *ages.items()* is called to break the *ages* dict up into the five individual **items**.

3. We then use a lambda function as the key to help sort. *lambda x* at this point will be the individual **item** in *ages.items()*.

4. The function of *lambda x* is to sort by *x[0]* The contents of x[] is the key/value pair in the dict. For example, {'julian', 20}. The 0 indicates the first position in the pair, the key, which in this case is the name 'julian'.

5. The output is then sorted by the key position in ascending, alphabetical order.

Note: The output of *sorted()* is a new list. The *ages* dict was not altered, a new list was generated and can thus be stored (hint hint!).


##Sorting the Output in Numerical Order

Now for the flip side. What if I wanted to sort it in numerical order which would be by the **value** in this case?

Identical as the above sort with one tiny change:

~~~~
>>> sorted(ages.items(), key=lambda x: x[1])
[('zack', 3), ('julian', 20), ('bob', 23), ('daniel', 41), ('anthony', 95)]
~~~~

Yep! All we do is change the lambda x function to point at position *x[1]* - the value.


##Sorting in Reverse!

Sorting that output in reverse is quite simple as well. We use the *reverse* flag that *sorted()* so handily supports:

~~~~
>>> #Reverse/Descending Name Sort
>>> sorted(ages.items(), key=lambda x: x[0], reverse=True)
[('zack', 3), ('julian', 20), ('daniel', 41), ('bob', 23), ('anthony', 95)]
>>>
>>> #Reverse/Descending Age Sort
>>> sorted(ages.items(), key=lambda x: x[1], reverse=True)
[('anthony', 95), ('daniel', 41), ('bob', 23), ('julian', 20), ('zack', 3)]
~~~~


##Storing the Sort in a Dict

You'll have noticed that we still have the output in a list and haven't used *print()* yet. There's a reason for that.

The thing is, it's a lot harder and less Pythonic to print the output of a dict as a list, then iterate over that to get our friendlier *print()* output.

It'd be much better to iterate over the output like we did at the start of this post tbut to do that, our *sorted()* output would need to be a dict. How do we do that if we know *sorted()* always returns a list?

Easy!

~~~~
>>> dict(sorted(ages.items(), key=lambda x: x[0]))
{'anthony': 95, 'bob': 23, 'daniel': 41, 'julian': 20, 'zack': 3}
~~~~

We simply call dict on the output of *sorted()*. How cool is that? The output of is now a dict!


##Printing the Final Result

The moment of truth. Let's print the sorted dict output:

~~~~
>>> alpha = dict(sorted(ages.items(), key=lambda x: x[0]))
>>>
>>> for k, v in alpha.items():
    print(k, v)

anthony 95
bob 23
daniel 41
julian 20
zack 3
>>>
>>>
>>> num = dict(sorted(ages.items(), key=lambda x: x[1]))
>>>
>>> for k, v in num.items():
    print(k, v)

zack 3
julian 20
bob 23
daniel 41
anthony 95
>>>
~~~~


##Bonus: Substituting the Lambda for Readability

We *could* leave things as they are but we could make this a little more readable by storing the lambda function in a variable:

~~~~
>>> get_alpha = lambda x: x[0]
>>>
>>> sorted(ages.items(), key=get_alpha)
[('anthony', 95), ('bob', 23), ('daniel', 41), ('julian', 20), ('zack', 3)]
~~~~

Not too bad! We could also do that for the numerical sort by making a *get_num* variable!


##Bonus: Printing the Highest/Lowest Dict Item

Okay this is way out of scope for this post but I got playing and figured I'd add it in for good measure.

What if I wanted to list out the oldest chap in this list? Well, we don't need to sort anything, we just need to know the *max* number right? (For readability, I'm substituting the **value** lambda from the previous examples with *get_num*.

~~~~
>>> max(ages.items(), key=get_num)
('anthony', 95)
~~~~

We can also pull the youngest/lowest entry:

~~~~
>>> min(ages.items(), key=get_num)
('zack', 3)
~~~~


##Bonus: Wrap it all up in a def

Okay last one, I swear!

Why don't we put all of this into a function that we can call easily at any time? I mean, it'd be nice to have the ability to reuse this code on any dict we want to sort alphabetically or numerically right?

Keep in mind the below is assuming the dict key is the string we're sorting by. If we tried to give it a dict where the key was a number and the value was a string it would sort it by the key/number.

~~~~
>>> def alpha_sort(some_dict):
...     alpha = dict(sorted(ages.items(), key=lambda x: x[0]))
...     for k, v in alpha.items():
...         print(k,v)
... 
>>> 
>>> alpha_sort(ages)
anthony 95
bob 23
daniel 41
julian 20
zack 3
>>>
~~~~

We can then use this same *alpha_sort* function on any similarly constructed dict we want!

~~~~
>>> #Top 5 pet peeves
>>> alpha_sort(pet_peeves)
Bad Drivers 2
Laziness 3
Predictable TV Shows 4
Rude People 1
Telemarketers 5
>>> 

Note: Not really my top 5 pet peeves!


##Conclusion

How great is Python? If you have any other ideas or comments regarding ordering the output of a dict, please let me know! Always be Learning!

Keep Calm and Code in Python!

-- Julian