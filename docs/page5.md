# Pokotarou option
Option is useful function.
If you can master it, it may be easier to create test data.

## Random
Shuffle seed data when register.

```yml
Default:
  Pref:
    loop: 3
    col:
      name: ["Hokkaido", "Aomori", "Iwate"]
    # This is option
    option:
      name: ["random"]
```

__Result__
```ruby
# The following results change from run to run
Pref.all.pluck(:name) => ["Aomori", "Iwate", "Iwate"]
```

## Add identical numbers 
Add identical numbers to seed data of String type
```yml
Default:
  Pref:
    loop: 3
    col:
      name: ["Hokkaido", "Aomori", "Iwate"]
    option:
      name: ["add_id"]
```

__Result__
```ruby
Pref.all.pluck(:name) => ["Hokkaido_0", "Aomori_1", "Iwate_2"]
```

## Combine serveral options

Combination of options is possible

```yml
Default:
  Pref:
    loop: 3
    col:
      name: ["Hokkaido", "Aomori", "Iwate"]
    option:
      name: ["add_id", "random"]
```

The following results change from run to run

__Result__
```ruby
Pref.all.pluck(:name) => ["Hokkaido_0", "Iwate_1", "Hokkaido_2"]
```