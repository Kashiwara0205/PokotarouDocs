# Basic Usage

Just make yml file.  
If you register data to following prefecture data. 
     
__prefecture table__

|Field|Type|NULL|
|:---|:---|:---|
|id|bigint(20)| NO|
|name|varchar(255)|YES|
|created_at|datetime|NO|
|updated_at|datetime|NO|


__prefecture model__

|Model|
|:---|
|Pref|

First, Please make following yml file your favorite directory of rails project.  
The file name can be anything.  
In my case, made yml file in db directory and named file __pref_data__.  

```yml
Default:
  Pref:
    loop: 3
```

and write following ruby code in seeds.rb.

```ruby
Pokotarou.make("./db/pref_data.yml")

or 

Pokotarou::V2.new("./db/pref_data.yml").make
```

and execute following command

```bash
$ rails db:seed
```

prefecture data is registerd your db.
Let's check with the following code.

```ruby
# You have to get 3
Pref.all.count => 3
```