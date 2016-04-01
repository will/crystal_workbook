# Similarities

### syntax, blocks, much of stdlib, enumerable
```playground
p (1..5).to_a
  .sort { |a,b| b<=>a}
  .reject { |a| a.even? }
```

### classes, oop, implicit return
```playground
class Adder
  def initialize(a)
    @a = a
  end

  def add(b)
    @a + b
  end
end
a = Adder.new(5)
a.add(2)
```

### open classes

```playground
class A; def b; 1 end end
class A; def b; 2 end end
A.new.b
```

even stdlib

### modules, mixins
```playground
module C; def c; 'c' end end
class A; extend C end
class B; extend C end
A.c == B.c
```

### idioms
```playground
def do_something
  puts "hey everyone"
end

def you_shouldnt?
  false
end

do_something unless you_shouldnt?
```

```playground
class Foo
  def a(i)
    @memo ||= i * i
  end
end
foo = Foo.new
foo.a(5)
foo.a 2
```

### even a spec framework
```playground
require "spec"
describe "thing" do
  it "works" do
    (1+2).should eq(3)
  end
end
```

### overall structure of programs
files, classes, get up to speed quickly


# [so what, is this literally just the same thing as ruby?](020_differences)
