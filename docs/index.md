# Home
![pokotarou_logo](img/Pokotarou.png)

__Pokotarou__ is convinient Mysql seeder of Ruby on Rails.

__Easy to use!!__  
Just use yml file.  
very very very simple!  
You dont' have to write ruby program about seed data!

__Fast speed!!__  
__Pokotarou__ is fast seeder.  
Because contains [Activerecord-import](https://github.com/zdennis/activerecord-import).  
so, always run bulk insert when execute rails db:seed command!

If you use __Pokotarou__ about following table.

|Field|Type|NULL|
|:---|:---|:---|
|id|bigint(20)| NO|
|name|varchar(255)|YES|
|created_at|datetime|NO|
|updated_at|datetime|NO|

__Pokotarou__ can register 10,000 records in 0.41s on average.