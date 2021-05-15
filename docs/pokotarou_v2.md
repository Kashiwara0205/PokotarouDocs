# Pokotarou::V2

You can be changed test data by using V2

```ruby
Pokotarou::V2.new("./db/pref_data.yml")
             .change_loop(:Default, :Pref, 5)
             .change_seed(:Default, :Pref, :name,  ["a", "b", "c"])
             .make
```

__Result__
```ruby
Pref.count => 5
Pref.pluck(:name) => ["a", "b", "c", "a", "b"]
```