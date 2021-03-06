# Pokotarou Handler

If you want to make test data dynamically, you should use __PokotarouHandler__.

When you use __PokotarouHandler__, can update pokotarou's parameter  in ruby code.


## Change setting yml

In the following example, the number of loops is changed

```ruby
  handler = Pokotarou.gen_handler("./config_filepath")
  # change loop config
  handler.change_loop(:Default, :Pref, 6)
  handler.make
```


In the following example, seed data is changed

```ruby
  handler = Pokotarou.gen_handler("./yml_filepath")
  # change seed data config number
  handler.change_seed(:Default, :Pref, :name, ["a", "b", "c"])
  handler.make
```

## Delete setting yml

In the following example, delete block config

```ruby
  handler = Pokotarou.gen_handler("./yml_filepath")
  # delete model config in parameter
  handler.delete_block(:Default)
  handler.make
```

In the following example, delete model config

```ruby
  handler = Pokotarou.gen_handler("./yml_filepath")
  # delete model config in parameter
  handler.delete_model(:Default, :Pref)
  handler.make
```

In the following example, delete col config

```ruby
  handler = Pokotarou.gen_handler("./yml_filepath")
  # delete col config in parameter
  handler.delete_col(:Default, :Pref, :name)
  handler.make
```

## Set autoincrement

can change autoincrement status

```ruby
  handler = Pokotarou.gen_handler("./yml_filepath")
  handler.set_autoincrement(:Default, :Pref, :false)
  handler.change_seed(:Default, :Pref, :id, [5, 6, 7])
  handler.make
```

## Set randomincrement

can change randomincrement status

```ruby
  handler = Pokotarou.gen_handler("./yml_filepath")
  handler.set_randomincrement(:Default, :Pref, :true)
  handler.make
```