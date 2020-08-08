# Python-identity-interview
a == b and a is b interview question - solved

Both “==” and “is” are operators in Python(Link to operator page in Python). For beginners, they may interpret “a == b” as “a is equal to b” and “a is b” as, well, “a is b”. Probably this is the reason why beginners confuse “==” and “is” in Python.
I want to show some examples of using “==” and “is” first before the in-depth discussion.
```
>>> a = 5
>>> b = 5
>>> a == b
True
>>> a is b
True
```
Simple, right? a == b and a is b both return True. Then go to the next example.
```
>>> a = 1000
>>> b = 1000
>>> a == b
True
>>> a is b
False
```
WTF ?!? The only change from the first example to the second is the values of a and b from 5 to 1000. But the results already differ between “==” and “is”. Go to the next one.
```
>>> a = []
>>> b = []
>>> a == b
True
>>> a is b
False
```
Here is the last example if your mind is still not blown.
```
>>> a = 1000
>>> b = 1000
>>> a == b
True
>>> a is b
False
>>> a = b 
>>> a == b
True
>>> a is b
True
```
The official operation for “==” is equality while the operation for “is” is identity. You use “==” for comparing the values of two objects. “a == b” should be interpreted as “The value of a is whether equal to the value of b”. In all examples above, the value of a is always equal to the value of b (even for the empty list example). Therefore “a == b” is always true.
Before explaining identity, I need to first introduce id function. You can get the identity of an object with idfunction. This identity is unique and constant for this object throughout the time. You can think of this as an address for this object. If two objects have the same identity, their values must be also the same.
```
>>> id(a)
2047616
```
The operator “is” is to compare whether the identities of two objects are the same. “a is b” means “The identity of a is the same as the identity of b”.
Once you know the actual meanings of “==” and “is”, we can start going deep on those examples above.
First is the different results in the first and second examples. The reason for showing different results is that Python stores an array list of integers from -5 to 256 with a fixed identity for each integer. When you assign a variable of an integer within this range, Python will assign the identity of this variable as the one for the integer inside the array list. As a result, for the first example, since the identities of a and b are both obtained from the array list, their identities are of course the same and therefore a is bis True.
>>> a = 5
>>> id(a)
1450375152
>>> b = 5
>>> id(b)
1450375152
But once the value of this variable falls outside this range, since Python inside does not have an object with that value, therefore Python will create a new identity for this variable and assign the value to this variable. As said before, the identity is unique for each creation, therefore even the values of two variables are the same, their identities are never equal. That’s why a is bin the second example is False
>>> a = 1000
>>> id(a)
12728608
>>> b = 1000
>>> id(b)
13620208
(Extra: if you open two consoles, you will get the same identity if the value is still within the range. But of course, this is not the case if the value falls outside the range.)
Image for post
Once you understand the difference between the first and second examples, it is easy to understand the result for the third example. Because Python does not store the “empty list” object, so Python creates one new object and assign the value “empty list”. The result will be the same no matter the two lists are empty or with identical elements.
>>> a = [1,10,100,1000]
>>> b = [1,10,100,1000]
>>> a == b 
True
>>> a is b
False
>>> id(a)
12578024
>>> id(b)
12578056
Finally, we move on to the last example. The only difference between the second and the last example is that there is one more line of code a = b.However this line of code changes the destiny of the variable a. The below result tells you why.
>>> a = 1000
>>> b = 2000
>>> id(a)
2047616
>>> id(b)
5034992
>>> a = b
>>> id(a)
5034992
>>> id(b)
5034992
>>> a
2000
>>> b
2000
As you can see, after a = b, the identity of a changes to the identity of b. a = bassigns the identity of bto a. So both aand b have the same identity, and thus the value of a now is the same as the value of b, which is 2000.
The last example tells you an important message that you may accidentally change the value of an object without notice, especially when the object is a list.
>>> a = [1,2,3]
>>> id(a)
5237992
>>> b = a
>>> id(b)
5237992
>>> a.append(4)
>>> a
[1, 2, 3, 4]
>>> b
[1, 2, 3, 4]
From the above example, because both a and bhave the same identity, their values must be the same. And thus after appending a new element to a, the value of bwill be also impacted. To prevent this situation, if you want to copy the value from one object to another object without referring to the same identity, the one for all method is to use deepcopyin the module copy (Link to Python document). For list, you can also perform by b = a[:] .
>>> import copy
>>> a = [1,2,3]
>>> b= copy.deepcopy(a)
>>> id(a)
39785256
>>> id(b)
5237992
Using [:]for copying elements to a new variable
>>> a = [1,2,3]
>>> id(a)
39785256
>>> b = a[:]
>>> id(b)
23850216
>>> a.append(4)
>>> a
[1, 2, 3, 4]
>>> b
[1, 2, 3]
