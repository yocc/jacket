# php



[toc]



## A numeric string as array key in PHP

```php
A numeric string as array key in PHP
https://stackoverflow.com/questions/4100488/a-numeric-string-as-array-key-in-php
1. 不行, 没有
2. 
$data =  new stdClass;
$data->{"12"} = 37;
$data = (array) $data;
var_dump( $data );
array(1) {
  ["12"]=>
  int(37)
}
```

## mongodb, php

```php
// mongodb
// nami 数据源落地库
// @see: https://www.mongodb.com/docs/php-library/current/tutorial/connecting/
// $collection = (new MongoDB\Client('mongodb://10.211.21.18:27018/sport_nano?replicaSet=Sport'))      // uri
$collection = (new MongoDB\Client('mongodb://10.211.21.18:27018/?replicaSet=Sport'))      // uri
    ->{'sport_nano'}                                                                      // database
    ->{'football_match'};                                                                 // collection

// find
// @see: https://www.mongodb.com/docs/php-library/current/tutorial/crud/#find-many-documents
// db.getCollection('football_match').find({"da.season_id": 11239, "da.competition_id": 13})
// db.getCollection('football_match').find({"da.season_id": 11239, "da.competition_id": 13}).count()    // 2023女足世界杯64场比赛
$cursor = $collection->find(
    [   // $filter
        "da.season_id"      => 11239,                   // season, 2023女足世界杯 11239 
        "da.competition_id" => 13,                      // competition, 2023女足世界杯 13 
    ],
    [   // $options
        "sort"              => [                        // 按比赛时间正序
            "da.match_time" => 1,
        ]
    ]
);

// 整理
$matches = json_decode(json_encode($cursor->toArray()), true);
// var_dump($matches);exit;


```

