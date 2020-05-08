# Ptool(Pokotarou tool)

Ptool is convinient seed data generater.

## gen_br_text

generate following br_text.

```
"text\ntext"
```

|param               |description                                                        |
|:-------------------|-------------------------------------------------------------------|
| none               | none                                                              |

```yml
Default:
  Pref: 
    loop: 3
    col:
      name: <Ptool.gen_br_text>
```

## gen_created_at

generate created_at array.

|param             |description                                                        |
|:-----------------|-------------------------------------------------------------------|
| n                | generate day number, if 7 then generate 7size array, default 7    |
| datetime_str     | begin datetime, default DateTime.now.to_s                         |
| with_random_time | boolean, if true register with random_time, default false         |


```yml
Default:
  Pref: 
    loop: 3
    col:
      created_at: <Ptool.gen_created_at(10, "1997-02-05", true)>
```

## gen_updated_at

generate updated_at array.

|param               |description                                                        |
|:-------------------|-------------------------------------------------------------------|
| created_arr        | datetime array                                                    |
| with_random_time   | boolean, if true register with random_time, default false         |

```yml
Default:
  Pref: 
    loop: 3
    col:
      created_at: <Ptool.gen_created_at(10, "1997-02-05", true)>
      updated_at: <Ptool.gen_updated_at(maked_col[:Pref][:created_at], true)>
```