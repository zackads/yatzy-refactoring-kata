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

## 3.  Re-order all methods to mimick the Yahtzee score card

Code should read like a newspaper: top to bottom.  So we moved the `initialize` method to the top of the Class and re-ordered the others to match a Yahtzee scorecard.

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

```ruby
def self.ones(*dice)
    score_upper_section(1, dice)
end

# ...

private

def self.score_upper_section(number, dice)
  dice.filter {|i| i == number}.sum 
end
```

## 5.  Extract method again (and call it using `self.class.score_upper_section`)

As above (ish).

## 6.  Used the good ol' splat operator again

After:

```ruby
def initialize(*dice)
    @dice = dice
end
```

## 7. Substitue algorithm in `yatzy`

<https://sourcemaking.com/refactoring/substitute-algorithm>

Before:

```ruby
def self.yatzy(dice)
  counts = [0]*(dice.length+1)
  for die in dice do
    counts[die-1] += 1
  end
  for i in 0..counts.size do
    if counts[i] == 5
      return 50
    end
  end
  return 0
end
```

After:

```ruby
def self.yatzy(dice)
  if dice.uniq.size == 1 then
    return 50
  else
    return 0
  end
end
```

## 8. Replace magic literal in `score_pair`

Before:

```ruby
def self.score_pair( d1,  d2,  d3,  d4,  d5)
  counts = [0]*6
  counts[d1-1] += 1
  counts[d2-1] += 1
  counts[d3-1] += 1
  counts[d4-1] += 1
  counts[d5-1] += 1
  at = 0
  (0...6).each do |at|
    if (counts[6-at-1] >= 2)
      return (6-at)*2
    end
  end
  return 0
end
```

After:

```ruby
def self.score_pair( d1,  d2,  d3,  d4,  d5)
  counts = [0]*SIZE_OF_DIE
  counts[d1-1] += 1
  counts[d2-1] += 1
  counts[d3-1] += 1
  counts[d4-1] += 1
  counts[d5-1] += 1
  at = 0
  (0...SIZE_OF_DIE).each do |at|
    if (counts[SIZE_OF_DIE-at-1] >= 2)
      return (SIZE_OF_DIE-at)*2
    end
  end
  return 0
end
```