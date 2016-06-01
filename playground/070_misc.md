# miscellaneous

### structs

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

### io

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

## [crystal-lang.org/docs](crystal-lang.org/docs)

# [end](999_end)
