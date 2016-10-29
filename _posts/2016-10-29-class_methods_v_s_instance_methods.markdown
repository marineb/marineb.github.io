---
layout: post
title:  "Class Methods V.S. Instance Methods"
date:   2016-10-29 17:36:03 -0400
---


I'm going to try to explain the difference between **Class Methods** and **Instance Methods** in ruby. 

First let me demonstrate how they can be visually differentiated: 

Class Method:

```
class App
  def self.welcome
    puts "hello world!"
  end
end
```

Instance Method:

```
class App
  def welcome
    puts "hello world!"
  end
end
```

Ok. That's easy, the class method is precedeed by the word `self`. So what makes them different? 

* **Class methods** are called on a class
* **Instance methods** are called on an instance of a class. It means there must at least be one instance of a class created for them to be able to function. 

Let's jump into some examples to understand this. Copy and paste this below into your irb: 

```
class App
  def welcome
    puts "hello world!"
  end
end
```

What do you think will happen if we then call `App.welcome`?

Try it. 

It gave us an error. Because we're calling an instance method on App, when we haven't created an instance of App yet. How can we fix this? 

We can either change our program so our method becomes a **class method**, like this:

```
class App
  def self.welcome
    puts "hello world!"
  end
end
```

OR 

Or we can create a new instance of the class App when we call the method so that we have: `App.new.welcome`. 

In summary we have these 2 options below:

```
class App
  def self.welcome
    puts "hello world! i'm a class method!"
  end
end

App.welcome
```

```
class App
  def self.welcome
    puts "hello world! i'm an instance method!"
  end
end

App.new.welcome
```


## When should we use a class method vs a instance method?
Let's say our class is a little bit less anstract: it's `Movie`.

* We should use **class methods** for anything that doesn't operate on an instance of `Movie`
* We should use **instance methods** for when we're operating on specific `Movie` instances (for instance, adding a movie names to each movie instance)

I hope that cleared it up for you. If you have questions or corrections, feel free to reach out to me at [@marineboudeau](http://twitter.com/marineboudeau). 




