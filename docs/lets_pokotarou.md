# Let's Pokotarou

By the end of reading this document, you will be a pokotarou master...


## Register seed data by yml

If you are troublesome to set columns data in yml.  
you don't set columns data.  
Pokotarou will register automatically prepared data.

Let's see the following example!

```yml
Default:
  Pref:
    loop: 3
```

This yml file register automatically prepared data __three times__.

Offcourse, you can set seed data by yourself!

```yml
Default:
  Pref:
    loop: 3
    col:
      name: "Hokkaido"
```

## Array

You can set array data in yml.  
Array data is registerd one by one.

```yml
Default:
  Pref:
    loop: 3
    col:
      name: ["Hokkaido", "Aomori", "Iwate"]
```

__Result__
```ruby
Pref.pluck(:name) => ["Hokkaido", "Aomori", "Iwate"]
```


Don't worry if the array exceeds the number of loops.    
Circulates automatically.

```yml
Default:
  Pref:
    loop: 6
    col:
      name: ["Hokkaido", "Aomori", "Iwate"]
```

__Result__
```ruby
Pref.pluck(:name) =>  ["Hokkaido", "Aomori", "Iwate", "Hokkaido", "Aomori", "Iwate"]
```

## Maked hash
'maked' is bery userfule function.
It is hash and accumulate data registed in the past. 

For example, in the case of below, reffer name of Pref in Default block by maked.

```yml
Default:
  Pref:
    loop: 2
    col:
      name: ["Hokkaido", "Aomori"]
  Member:
    loop: 2
    col:
      name: <maked[:Default][:Pref][:name]>
```

__maked hash structure__

```
{
  Block1:{
    Model:{
      col1:xxx,
      col2:xxx
      col3:xxx
    }
  },

  Block2:{
    Model:{
      col1:xxx,
      col2:xxx
      col3:xxx
    }
  },
}
```

__Result__
```ruby
Member.pluck(:name) =>  ["Hokkaido", "Aomori"]
```


## MakedCol hash

'maked_col' is like 'maked'
but you can write without 'block key'
Let's look following example.


```yml
Default:
  Pref:
    loop: 2
    col:
      name: ["Hokkaido", "Aomori"]
  Member:
    loop: 2
    col:
      name: <maked_col[:Pref][:name]>
```

__maked_col structure__

```
{
  Model:{
    col1:xxx(merged array from each blocks),
    col2:xxx,
    col3:xxx
  }
}
```

__Result__
```ruby
Member.pluck(:name) =>  ["Hokkaido", "Aomori"]
```


## Foreign key

__※ If you set association(belongs_to, has_many...), Pokotarou automatically register foreign keys__
{ F|Model } means that Model.pluck(:id).
Let's see the following file.   
Member model record is registerd with pref_id(foregin key).

```yml
Default:
  Pref:
    loop: 3
    col:
      name: ["Hokkaido", "Aomori", "Iwate"]
  Member:
    loop: 3
    col:
      pref_id: F|Pref
```

__Result__
```ruby
Pref.pluck(:id) =>  [1, 2, 3]
Member.pluck(:pref_id) => [1, 2, 3]
```
## Seeting Columns

If you want to register data from each model.
you can write following example.

```yml
Default:
  Pref:
    loop: 3
    col:
      name: ["Hokkaido", "Aomori", "Iwate"]
  Member:
    loop: 3
    col:
      pref_id: C|Pref|id
      name: C|Pref|name
```

__Result__
```ruby
Pref.pluck(:id) =>  [1, 2, 3]
Member.pluck(:pref_id) => [1, 2, 3]
Member.pluck(:name) => ["Hokkaido", "Aomori", "Iwate"]
```

## Run ruby code in yml
'< >' means expression expansion.
You can run ruby code in '< >'.

```yml
Default:
  Pref:
    loop: 3
    col:
      name: <["Hokkaido", "Aomori", "Iwate"]>
      created_at: <Date.parse('1997/02/05')>
```

__Result__
```ruby
Pref.pluck(:name) => ["Hokkaido", "Aomori", "Iwate"]
Pref.pluck(:created_at) => [1997/02/05, 1997/02/05, 1997/02/05]

```

## Import(require ruby file in pokotarou)
If you want to require ruby file and add method which run in yml.
You can add method.  
Let's see the follwoing example.

First, setting yml file.  
I want to run pref_name method.

```yml
Default:
  Pref:
    loop: 3
    col:
      # this is ruby method
      name: <pref_name>
```

setting the following ruby file

```ruby
def pref_name
  ["Hokkaido", "Aomori", "Iwate"]
end
```

and run the following code in seeds.rb.

```ruby
Pokotarou.import("./method_filepath")
Pokotarou.make("./config_filepath")
```

or

you can also use array

```ruby
Pokotarou.import(["./method_filepath"])
Pokotarou.make("./config_filepath")
```


__Pokotarou.import__ means that require which run in pokotarou.

__Result__
```ruby
Pref.pluck(:name) => ["Hokkaido", "Aomori", "Iwate"]

```

## Import in yml

You can import in pokotarou yml file.

```yml
import':
  - /method_filepath
  - /method_filepath2

Default:
  Pref:
    loop: 3
    col:
      # you can use imported method
      name: <pref_name>
```


## Multiple blocks

```yml
Default:
  Pref:
    loop: 3
Default2:
  Pref:
    loop: 3
```

__Result__
```ruby
Pref.all.count => 6

```

and, You can change the name of the block

```yml
Hoge:
  Pref:
    loop: 3
Fuga:
  Pref:
    loop: 3
```

__Result__
```ruby
Pref.all.count => 6

```