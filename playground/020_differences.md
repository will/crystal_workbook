# no, not really

### @initialize

```playground
class Greeter
  property name

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

```
➤ crystal tool hierarchy -e Greeter g.cr
- class Object (4 bytes)
  |
  +- class Reference (4 bytes)
     |
     +- class Greeter (24 bytes)
            @name       : String (8 bytes)
            @salutation : String (8 bytes)
```

### fancier to_proc
```playground
a = %w[apple bat cat]

a.map(&.upcase)

a.map(&.reverse.upcase)

a.map( &.split(//).sort.join )
```

# [about those types](030_types)

