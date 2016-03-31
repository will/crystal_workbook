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

# hi * 2
# lo * 2

# lo + 2
# hi + 2

# ok let's get rid of the else

# (hi as Int32) + 2
# (lo as Int32) + 2
```

