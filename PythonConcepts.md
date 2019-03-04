# split()--->
### At some point, you may need to break a large string down
### into smaller chunks, or strings. This is the opposite of concatenation
### which merges or combines strings into one.


# join()--->
### The join() is a string method which returns a string concatenated with the elements of an iterable
### The join() method takes an iterable - objects capable of returning its members one at a time
### Some of the example of iterables are:
### Native datatypes - List, Tuple, String, Dictionary and Set
### File objects and objects you define with an __iter__() or __getitem()__ method

### It only works on strings and not int or any other data type

# tuple=('8','9','3')
### print ','.join(tuple)

```
import math
c=50
h=30
value = []
items=[x for x in raw_input().split(',')]  # user enter a comma separated value
print items            # returns a list
for d in items:
    value.append(str(int(round(math.sqrt(2*c*float(d)/h)))))

print value            # returns a list
print ','.join(value)  #return in comma separated output
```

# 2-D Matrix
```
b= [[i*j for j in range(y)] for i in range(x)]

```

# 3-D Matrix
```
b= [[[i*j*z for j in range(y)] for i in range(x)]for z in range(g)]
```

# ASCII value conversion
```
ord(<enter the char>)
```

# Sorting without using sorted() in python
```
a=raw_input("Enter\n")
b=[]

list= [b.append(i) for i in a.split(',') if i not in b]
print b
for i in range(len(b)):
    for j in range(len(b)-1):
        if b[j]>b[j+1]:     #replacing if the condition is true
            b[j],b[j+1]=b[j+1],b[j]
print b
```

# Conversion from Binary to integer and vice-versa

```angular2
a=(raw_input("Enter\n"))
for i in a.split(','):
    i=int(i,2)  # bin to int
    j=bin(i)   # int to binary
    print j[::2]  # omit the '0b' in the bin converted number
    print i
```

# Check if the character is a letter or a digit

## isalpha() ---> The isalpha() returns:
## isaplha() doesn't take any inputs
### True if all characters in the string are alphabets (can be both lowercase and uppercase).
### False if at least one character is not alphabet.

# STRING METHODS

## str.isaplha()--->
### Return true if all characters in the string are alphabetic and there is at least one character, false otherwise

## str.isalnum()--->
### Return true if all characters in the string are alphanumeric and there is at least one character, false otherwise
###  A character c is alphanumeric if one of the following returns True: c.isalpha(), c.isdecimal(), c.isdigit(), or c.isnumeric().

## str.isdecimal()---> 
### Return true if all characters in the string are decimal characters and there is at least one character, false otherwise.
### ex. "12345½" -->false

## str.isdigit()--->
### Return true if all characters in the string are digits and there is at least one character, false otherwise.
### ex. "12345½"--->false

## str.islower()--->
### Return true if all cased characters [4] in the string are lowercase and there is at least one cased character, false otherwise.

## str.isnumeric--->
### Return true if all characters in the string are numeric characters, and there is at least one character, false otherwise
### ex. "12345½"---> True

## str.strip()--->
### strips off the space left and right of the character or end os sentence

## str.rstrip()--->
### Strips only the right side space only

## str.lstrip()---->
### Strips off the space on the left side only

# Accessing Dictionary

```angular2
s = raw_input()
d={"UPPER CASE":0, "LOWER CASE":0}
print d["UPPER CASE"]  #you can access a particular value like this
for c in s:
    if c.isupper():
        d["UPPER CASE"]+=1 # incrementing the value
    elif c.islower():
        d["LOWER CASE"]+=1
    else:
        pass
print "UPPER CASE", d["UPPER CASE"] #you can access a particular value
print "LOWER CASE", d["LOWER CASE"]
```
```
a=9
n1=int("%s" % a)
print n1
# aa is undefined unless I define like this below
n2= int("%s%s" % (a,a))
print n2
```

# Python Dictionary get()

### dict.get(key[, value])
### The get() method takes maximum of two parameters:

#### key - key to be searched in the dictionary
#### value (optional) - Value to be returned if the key is not found. The default value is None.
#### the value for the specified key if key is in dictionary.
#### None if the key is not found and value is not specified.
#### value if the key is not found and value is specified
### The get() method returns the value for the specified key if key is in dictionary.
```angular2
person = {'name': 'Phill', 'age': 22}

print('Name: ', person.get('name'))
print('Age: ', person.get('age'))

# value is not provided
print('Salary: ', person.get('salary'))

# value is provided
print('Salary: ', person.get('salary', 0.0))
```

```
Name:  Phill
Age:  22
Salary:  None
Salary:  0.0
```


# Error_Handling

### try and except
### example:-
```angular2
try:
    return 5/0
except ZeroDivisionError:
     print "Zero Division Error"
```

# Lambda Function

```angular2
evenNumbers = filter(lambda x: x%2==0, li
squaredNumbers = map(lambda x: x**2, range(1,21))```
