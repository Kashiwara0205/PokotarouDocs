# Pipeline Execute

Pokotarou can run by pipeline.  
Pokotarou.pipleine_execute means that continuous execution of __Pokotarou.execute__.  
You can set filepath, change_data, args parameter.

```ruby
Pokotarou.pipeline_execute([{
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
Pokotarou.pipeline_execute([{
    filepath: "./first_step_yml_filepath", 
  },{
    filepath: "./second_step_yml_filepath", 
  }])
```

__Result__
```ruby
Pref.all.pluck(:name) => ["a", "b", "c", "hoge", "fuga", "piyo"]
```