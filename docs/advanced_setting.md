# Advanced Setting

## Grouping

Grouping is useful function.
Especially useful when setting multiple options.

```yml
Default:
  Member:
    grouping: 
      # set columns you want to group
      str_g: ["name", "remark"]
    col:
      # you can use "str_g" at col
      str_g: <['yarh!']>
    option:
      # also you can use "str_g" at option
      str_g: ["add_id"]

```

__Result__
```ruby
Member.all.pluck(:name) =>  ['yarh!_0', 'yarh!_1', 'yarh!_2']
```

## Template

You can set template config by template' key.

The template can be overwritten with the one set later.

```yml
template':
  pref_template:
    loop: 3
    col:
      pref_id: F|Pref
      name: ["hogeta", "fuga", "pokota"]
Pref:
  Pref: 
    loop: 3
    col:
      name: ["Hokkaido", "Aomori", "Iwate"]

Member1:
  Member:
    template: pref_template
  
Member2:
  Member:
    template: pref_template
    col:
      name: ["hogeta2", "fuga2", "pokota2"]
```

__Result__
```ruby
Member.all.pluck(:name) =>  ["hogeta", "fuga", "pokota", "hogeta2", "fuga2", "pokota2"]
```

## Return
You can set return value by return' key.

```yml
Default:
  Pref: 
    loop: 3
    col:
      name: ["Hokkaido", "Aomori", "Iwate"]

return': <maked[:Default][:Pref][:name]>

```

__Result__
```ruby
 return_val = Pokotarou.execute("yml_filepath")
 return_val => ["Hokkaido", "Aomori", "Iwate"]
```

## Args

You can set args by hash.

```ruby
Pokotarou.set_args({ name:  ["Hokkaido", "Aomori", "Iwate"] })
Pokotarou.execute("yml_filepath")
```

```yml
Default:
  Pref: 
    loop: 3
    col:
      name: <args[:name]>
```

__Result__
```ruby
Pref.all.pluck(:name) => ["Hokkaido", "Aomori", "Iwate"]
```

## Const
You can set const variables by const' key.

```yml
const':
  name: "hoge"
Default:
  Pref:
    loop: 3
  col:
    name: <const[:name]>
```

## Validation

Run validation when regist

```yml
Default:
  Pref:
    loop: 3
    validate: true
```

## Disable Autoincrement

You can disable the autoincrement setting
If you disable the setting, you can register id data prepared by yourself

```yml
Default:
  Pref:
    loop: 3
    autoincrement: false
    col:
      id: [100, 101, 102]
```

__Result__
```ruby
Pref.all.pluck(:id) =>  [100, 101, 102]
```

## Enable Randomincrement

You can enable the randomincrement setting
this mode shuffled autoincrement array, and register it.

```yml
Default:
  Pref:
    loop: 3
    randomincrement: true
```

__Result__
```ruby
# The following results change from run to run
Pref.order(:name).pluck(:id) =>  [3, 2, 1]
```