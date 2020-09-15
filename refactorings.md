# Refactoring kata

## 1.  Modified `self.chance` to be sum of args

Before:

```ruby
def self.chance(d1, d2, d3, d4, d5)
  total = 0
  total += d1
  total += d2
  total += d3
  total += d4
  total += d5
  return total
end
```

After:

```ruby
def self.chance(*dice)
  return dice.sum
end
```

## 2.  Consolidate duplicate conditional fragmets in `self.ones`

Before:

```ruby
def self.ones( d1,  d2,  d3,  d4,  d5)
    sum = 0
    if (d1 == 1)
      sum += 1
    end
    if (d2 == 1)
      sum += 1
    end
    if (d3 == 1)
      sum += 1
    end
    if (d4 == 1)
      sum += 1
    end
    if (d5 == 1)
      sum += 1
    end

    sum
  end
```

After:

```ruby
def self.ones(*dice)
  dice.filter {|i| i == 1}.sum 
end
```

## 3.  Re-order `initialize` method

Code should read like a newspaper: top to bottom.  So we moved the `initialize` method to the top of the Class.

## 4.  Extract method

Before:

```ruby
def self.twos( d1,  d2,  d3,  d4,  d5)
    sum = 0
    if (d1 == 2)
      sum += 2
    end
    if (d2 == 2)
      sum += 2
    end
    if (d3 == 2)
      sum += 2
    end
    if (d4 == 2)
      sum += 2
    end
    if (d5 == 2)
      sum += 2
    end
    return sum
  end
```

After:

