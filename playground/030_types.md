# types

### secretly two methods
```playground
def double(i)
  i * 2
end

answer = double(3)
puts typeof(answer)

answer2 = double("hi")
puts typeof(answer2)
```

### union
```playground
def validate(i)
  if i > 10
    i
  else
    "too low"
  end
end

hi = validate(15)
hi.class
typeof(hi)

lo = validate(1)
lo.class
typeof(lo)

# hi * 2 # if hi
# lo * 2 # if lo

# lo + 2
# hi + 2

# ok let's get rid of the else

# (hi as Int32) + 2
# hi.not_nil! + 2
# (lo as Int32) + 2
```

### overloading
```
def foo(a,b)
  if b.kind_of? String
    case a
    when Fixnum
      1            # foo(3, "c")    # => 1
    when TrueClass
      2            # foo(true, "c") # => 2
    else
      3            # foo(:hi, "c")  # => 3
    end
  else
    4              # foo(:hi, :c)   # => 4
  end
end
```


```playground
def foo(a : Int, b : Char)
  1
end

def foo(a : Bool, b : Char)
  2
end

def foo(a, b : Char)
  3
end

def foo(a, b)
  4
end

foo 3, 'c'
foo true, 'c'
foo :hi, 'c'
foo :hi, :c
```

### Abstract
```playground
class Animal end

class Dog < Animal
  def talk; "Woof" end
end

class Cat < Animal
  def talk; "Meow" end
end

class Person
  getter pet : Animal

  def initialize(@name : String, @pet) end
end

will = Person.new "Will", Dog.new
alice = Person.new "Alice", Cat.new

p will.pet.talk
```

### containers

```playground
a = [ [1,'a'], [2,'b'] ]
# a << ['c', 3]
# a.first.first + 100

b = [ {1,'a'}, {2,'b'} ]
# b << {'c', 3}
b.first.first + 100

# c = []
```


#### What's the worst nails to hammer?

```playground
require "base64"
puts Base64.decode_string "ZmluZ2VybmFpbHM=\n"
```

# [macros](040_macros)
