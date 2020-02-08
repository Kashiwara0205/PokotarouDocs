# Pokotarou Handler

If you want to make test data dynamically, you should use __PokotarouHandler__.

When you use __PokotarouHandler__, can update pokotarou's parameter  in ruby code.


## Change setting yml

In the following example, the number of loops is changed

```ruby
  handler = Pokotarou.gen_handler("./config_filepath")
  # change loop config
  handler.change_loop(:Default, :Pref, 6)
  Pokotarou.execute(handler.get_data)
```


In the following example, seed data is changed

```ruby
  handler = Pokotarou.gen_handler("./yml_filepath")
  # change seed data config number
  handler.change_seed(:Default, :Pref, :name, ["a", "b", "c"])
  Pokotarou.execute(handler.get_data)
```

## Delete setting yml

In the following example, delete block config

```ruby
  handler = Pokotarou.gen_handler("./yml_filepath")
  # delete model config in parameter
  handler.delete_block(:Default)
  Pokotarou.execute(handler.get_data)
```

In the following example, delete model config

```ruby
  handler = Pokotarou.gen_handler("./yml_filepath")
  # delete model config in parameter
  handler.delete_model(:Default, :Pref)
  Pokotarou.execute(handler.get_data)
```

In the following example, delete col config

```ruby
  handler = Pokotarou.gen_handler("./yml_filepath")
  # delete col config in parameter
  handler.delete_col(:Default, :Pref, :name)
  Pokotarou.execute(handler.get_data)
```

## Pipeline Execute
Pokotarou can run by pipeline.  
Pokotarou.pipleine_execute means that continuous execution of __Pokotarou.execute__.  
You can set filepath, change_data, args parameter.

```ruby
Pokotarou.pipleine_execute([{
    filepath: "./yml_filepath", 
    change_data: { Default: { Pref: { name: ["a", "b", "c"] } } },
    args: { created_at: [DateTime.now] }
  },{
    filepath: "./yml_filepath", 
  }])
```

return val created in the previous step can get by __args[:passed_return_val]__ in the next step.


```yml
# First step
Default:
  Pref: 
    loop: 3
    col:
      name: ["a", "b", "c"]

return': <["hoge", "fuga", "piyo"]>
```

```yml
# Second step
Default:
  Pref: 
    loop: 3
    col:
      name: <args[:passed_return_val]>
```

```ruby
Pokotarou.pipleine_execute([{
    filepath: "./first_step_yml_filepath", 
  },{
    filepath: "./second_step_yml_filepath", 
  }])
```

__Result__
```ruby
Pref.all.pluck(:name) => ["a", "b", "c", "hoge", "fuga", "piyo"]
```