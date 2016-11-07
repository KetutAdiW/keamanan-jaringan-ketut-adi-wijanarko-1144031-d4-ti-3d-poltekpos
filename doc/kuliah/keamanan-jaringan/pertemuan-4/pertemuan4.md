Now it's time to get back to Python and see how a class is implemented in Python. We start with the most simple class, which can be defined. We just give it a name but omit all further specifications by using the n keyword. 
class Account(object):
	pass
We haven't defined any attributes or any methods in our simple account class. Now we will create an instance of this empty class: 
>>> from Account import Account
>>> x = Account()
>>> print x
<Account.Account object at 0x7f364120ab90>
>>>
Definition of Methods

A method differs from a function only in two aspects:

it belongs to a class and it is defined within a class
the first parameter in the definition of a method has to be a reference "self" to the instance of the class
a method is called without this parameter "self"
We extend our class by defining some methods. The body of these methods is still not specified:
class Account(object): 

    def transfer(self, target, amount): 
        pass 
 
    def deposit(self, amount): 
        pass 
 
    def withdraw(self, amount): 
        pass 
 
    def balance(self): 
        pass
Constructor

Python doesn't have explicit constructors like C++ or Java, but the __init__() method in Python is something similar, though it is strictly speaking not a constructor. It behaves in many ways like a constructor, e.g. it is the first code which is executed, when a new instance of a class is created. The name sounds also like a constructor "__init__". But strictly speaking it would be wrong to call it a constructor, because a new instance is already "constructed" by the time the method __init__ is called. 
But anyway, the __init__ method is used - like constructors in other object oriented programming languages - to initialize the instance variables of an object. The definition of an init method looks like any other method definition:

def __init__(self, holder, number, balance,credit_line=1500): 
        self.Holder = holder 
        self.Number = number 
        self.Balance = balance
        self.CreditLine = credit_line
Destructor

What we said about constructors holds true for destructors as well. There is no "real" destructor, but something similiar, i.e. the method __del__. It is called when the instance is about to be destroyed. If a base class has a __del__() method, the derived class's __del__() method, if any, must explicitly call it to ensure proper deletion of the base class part of the instance. 
The following example shows a class with a constructor and a destructor:

class Greeting:
    def __init__(self, name):
        self.name = name
    def __del__(self):
        print "Destructor started"
    def SayHello(self):
        print "Hello", self.name
If we use this class, we can see, the "del" doesn't directly call the __del__() method. It's apparent that the destructor is not called, when we delete x1. The reason is that del decrements the reference count for the object of x1 by one. Only if the reference count reaches zero, the destructor is called:
>>> from hello_class import Greeting
>>> x1 = Greeting("Guido")
>>> x2 = x1
>>> del x1
>>> del x2
Destructor started
Complete Listing of the Account Class

class Account(object):
    def __init__(self, holder, number, balance,credit_line=1500): 
        self.Holder = holder 
        self.Number = number 
        self.Balance = balance
        self.CreditLine = credit_line

    def deposit(self, amount): 
        self.Balance = amount

    def withdraw(self, amount): 
        if(self.Balance - amount < -self.CreditLine):
            # coverage insufficient
            return False  
        else: 
            self.Balance -= amount 
            return True

    def balance(self): 
        return self.Balance

    def transfer(self, target, amount):
	if(self.Balance - amount < -self.CreditLine):
            # coverage insufficient
            return False  
        else: 
            self.Balance -= amount 
            target.Balance += amount 
            return True
If the code is saved under Account.py, we can use the class in the interactive shell of Python:
>>> import Account
>>> k = Account.Account("Guido",345267,10009.78)
>>> k.balance()
10009.780000000001
>>> k2 = Account.Account("Sven",345289,3800.03)
>>> k2.balance()
3800.0300000000002
>>> k.transfer(k2,1000)
True
>>> k2.balance()
4800.0300000000007
>>> k.balance()
9009.7800000000007
>>> 
Our example has a small flaw: We can directly access the attributes from outside:
>>> k.balance()
9009.7800000000007
>>> k2.Balance
4800.0300000000007
>>> k2.Balance += 25000
>>> k2.Balance
29800.029999999999
>>> 

reference :http://www.python-course.eu/object_oriented_programming.php
