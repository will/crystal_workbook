# macros

```
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

### shell out

```
macro buildtime
  "{{`date`}}"
end

puts buildtime
puts `date`
```

```
➤ crystal build bt.cr && ./bt
Wed Dec  9 12:09:51 JST 2015
Wed Dec  9 12:09:51 JST 2015
➤ ./bt
Wed Dec  9 12:09:51 JST 2015
Wed Dec  9 12:10:28 JST 2015
➤ ./bt
Wed Dec  9 12:09:51 JST 2015
Wed Dec  9 12:11:01 JST 2015
```


#### What is base64 encoded?
```playground
require "base64"
puts Base64.decode_string "dGhpcyB0ZXh0IHJpZ2h0IGhlcmU="
```

# [linking](050_linking)
