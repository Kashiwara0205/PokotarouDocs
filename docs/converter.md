# Converter

|convert   |description                               |
|:---------|------------------------------------------|
| empty    | convert val to empty                     |
| nil      | convert val to nil                       |
| big_text | convert val to big_text("text" * 50)     |
| br_text  | convert val to br_text("text\n" * 5)     |

For example, following configfile register seed data while replacing with nil

```yml
Default:
  Pref: 
    loop: 3
    col:
      name: ["Hokkaido", "Aomori", "Iwate"]
    convert:
      name: ["nil(0..2)"]
```

__Result__
```ruby
Pref.pluck(:name) => [nil, nil, nil]
```

a little complex version

```yml
Default:
  Pref: 
    loop: 3
    col:
      name: ["Hokkaido", "Aomori", "Iwate"]
    convert:
      name: ["empty(0..0)", "nil(1..2)"]
```

__Result__
```ruby
Pref.pluck(:name) => ["", nil, nil]
```
