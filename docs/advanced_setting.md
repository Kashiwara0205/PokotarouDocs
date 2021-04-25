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

## Preset

You can set preset_path.  
preset-data can be created using this function.

※ If you use this function, then You must use different block name in each yml file.
Cannot be used at the same time as the function of const', template' and import_path'.

pref.preset.yml
```yml
PrefPreset:
  Pref: 
    loop: 3
    col:
      name: ["北海道", "青森県", "岩手県"]
```

test_data.yml
```yml

preset_path':
  - ./pref.preset.yml

Default:
  Member:
    loop: 1
    col:
      pref_id: F|Pref
      name: "hoge"
```

__Result__
```ruby
Pokotarou.execute("./test_data.yml")
Pref.all.pluck(:name) =>  ["北海道", "青森県", "岩手県"]
Member.all.pluck(:name) =>  ["hoge"]
```


## Template

You can set template config by template' key.

This feature is similar to inheritance.

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

You can also set template by file path

template_file1 yml

```yml
PrefTemplate:
  loop: 3
  col:
    name: ["hogeta", "fuga", "pokota"]
```

template_file2 yml

```yml
MemberTemplate:
  loop: 3
  col:
    name: ["t1", "t2", "t3"]
```

Pokotarou yml

```yml
template_path':
  - ./template_file1
  - ./template_file2
Pref:
  Pref: 
    template: PrefTemplate
  Member:
    template: MemberTemplate
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