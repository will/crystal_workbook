# Will Leinweber

* @leinweber
* bitfission.com
* citusdata.com

## Crystal

* large contributor
* github.com/will/crystal-pg

# Crystal

* ruby inspired
* compiled with llvm
* static types static dispatch
* still feels rubyish
* very fast

# How fast

### crystal
```
# Crystal 0.17.4
# kemal-0.12.0
require "kemal"

get "/" do
  "Hello World!"
end

Kemal.run
```

### ruby
```
# ruby 2.3.0p0
# puma-3.4.0
# sinatra-1.4.7

require "sinatra"

get "/" do
  "Hello World!"
end
```

### wrk -d 1m
```
Crystal                         |  Ruby
                                |
Threed Stats   Avg      Stdev   |  Thread Stats   Avg      Stdev
  Latency   489.98us    0.88us  |    Latency     4.85ms    3.98ms
  Req/Sec    12.14k     2.95k   |    Req/Sec     1.12k   170.08
Requests/sec:  24150.52         |  Requests/sec:   2227.03
Transfer/sec:      2.30MB       |  Transfer/sec:    402.35KB
                                |
RSS 3.0 MB                      |  RSS 164.2 MB
```

## sidekiq
"I started this project from scratch not knowing the language at all a week ago and had the core job processor running in 3 days."

— http://www.mikeperham.com/2016/05/25/sidekiq-for-crystal/

"Nearly identical to Sidekiq.rb but look at that execution time: 121µs! In real world code, I’m seeing Crystal execute 5-10x faster than MRI."

— http://www.mikeperham.com/2016/06/14/test-driving-sidekiq-and-crystal/



# Similarities

### syntax, blocks, much of stdlib, enumerable
### classes, oop, implicit return
```playground
class Thing
 def go
   (1..5).to_a
    .sort { |a,b| b<=>a}
    .reject { |a| a.even? }
  end
end

Thing.new.go
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


# differences

### @initialize

```playground
class Greeter
  property name : String

  def initialize(name, salutation = "Hello")
    @name = name
    @salutation = salutation
  end

  def greet
    "#{@salutation}, #{@name}"
  end
end

g = Greeter.new("Will")
g.greet
g.name
g.name = "Alice"
g.greet
```

### fancier to_proc
```playground
a = %w[apple bat cat]

a.map(&.upcase)

a.map(&.reverse.upcase)

a.map( &.split(//).sort.join )
```




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


# macros

```playground
  p (1..100).sum
  pp (1..100).sum
```

```playground
class Object
  macro getter(*names)
    {% for name in names %}
      {% name = name.var if name.is_a?(DeclareVar) %}

      def {{name.id}}
        @{{name.id}}
      end
    {% end %}
  end
end

class Person
  getter name, age
end

class Person
  def name; @name end
  def age; @age end
end
```


# linking

```playground
@[Link(ldflags: "-lpq -L`pg_config --libdir`")]
lib LibPQ
  fun connect = PQconnectdb(
      conninfo : UInt8*
  ) : Void*
  fun exec = PQexec(
    conn : Void*, query : UInt8*
  ) : Void*
  fun getvalue = PQgetvalue(
    res : Void*, row : Int32, column : Int32
  ) : UInt8*
end

conn = LibPQ.connect("postgres:///")
q = "select 'Hello it is ' || now()"
res = LibPQ.exec(conn, q)
p String.new(LibPQ.getvalue(res, 0, 0))
```



# structs

```playground
class Person
  property name : String
  def initialize(@name = "Will")
  end
end

def change_name(person)
  person.name = "Alice"
end

p = Person.new
p.name
change_name p
p.name
```

# io

```playground
class Person
  property name : String
  def initialize(@name = "Will")
  end

  def to_s_not(io : IO)
    io << "This is "
    io << name
    io << " so it is"
  end
end

Person.new.to_s

"3*3 is #{3*3} ok"
String.build { |io|
  io << "3*3 is " << 3*3 << " ok"
}.to_s
```

### formatter

### concurrency

``` playground
ch = Channel(Int32).new

spawn do
  pp (1..3).map { ch.receive.tap { |i| pp i } }.sum
end

spawn do
  100.upto(500) { |i| ch.send i }
end

Fiber.yield
```

## [crystal-lang.org/docs](crystal-lang.org/docs)
## @leinweber

