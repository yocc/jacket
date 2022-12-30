# SQL

----

[toc]





```php
# 隐式 $and
db.getCollection('predict').find({"ispause":'1', "sna.Lottery.Category":"jczqOnSellMatches", "$or":[{"sna.Result":{"$exists":false}}, {"sna.Result.status":{"$in":['1', '2']}}]})

# 显式 $and
db.getCollection('predict').find({
  "$and": [
    {"ispause":'1'}, 
    {"sna.Lottery.Category":"jczqOnSellMatches"}, 
    {"$or":[
      {"sna.Result":{"$exists":false}}, 
      {"sna.Result.status":{"$in":['1', '2']}}
    ]}
]})

  
  
public static function getsResultJCZQ()
{
    /// BEGIN PREDICT 
    // 操作 DB, 显式 $and
    $filter = [];
    $filter=> [
        '$and' => [
            'sna.Lottery.Category'  => strval($_GET['ca']), 
            '$or'       => [
                'sna.Result'        => ['$exists' => false],
                'sna.Result.status' => ['$in' => ['1', '2']],
            ],
            'ispause'   => '1',
            'isend'     => '0',
            'isdel'     => '0',
        ], 
    ];
  	// // 显式 $and, 另外表达
    // $filter['$and'] => [
    //     'sna.Lottery.Category'  => strval($_GET['ca']), 
    //     '$or'       => [
    //         'sna.Result'        => ['$exists' => false],
    //         'sna.Result.status' => ['$in' => ['1', '2']],
    //     ],
    //     'ispause'   => '1',
    //     'isend'     => '0',
    //     'isdel'     => '0',
    // ];
    // // 操作 DB, 隐式 $and
    // $filter = [];
    // $filter['sna.Lottery.Category']     = strval($_GET['ca']);      // ca, 玩法, Category, jczqOnSellMatches
    // $filter['$or']                      = [['sna.Result' => ['$exists' => false]], ['sna.Result.status' => ['$in' => ['1', '2']]]]; 
    // $filter['ispause']                  = '1';      // 下过注的
    // $filter['isend']                    = '0';
    // $filter['isdel']                    = '0';

    $options = [
        'limit' => 50,
        'sort'  => ['sna.Lottery.MatchTime' => 1],
        'projection' => [
        ],
    ];

    // var_dump($filter, $options);exit;
    $rs = PredictPugrModelDB::gets($filter, $options);
    /// END PREDICT 

    return $rs;
}

    /**
     * 获取，多条
     * https://www.mongodb.com/docs/php-library/current/tutorial/crud/#find-many-documents
     * https://www.mongodb.com/docs/php-library/current/reference/method/MongoDBCollection-find/#phpmethod.MongoDB\Collection::find
     *
     * function find($filter = [], array $options = []): MongoDB\Driver\Cursor
     * @param associative array $filter = []
     * @param associative array $options = []
     *  
     * @return associative array 
     * https://www.php.net/class.mongodb-driver-cursor
     */ 
    public static function gets($filter, $options)
    {
        $mongo_db0 = new MongoDB($db = 'db2', ['readPreference' => 'primary']);
        $rs = $mongo_db0->{DictConfig::$mongo['db2']['db']}->{DictConfig::$mongo['db2']['collection']}->find(
            $filter,
            $options
        );
        
        // 对返回结果分别处理
        /// 结果 false, 异常返回
        if (false === $rs) {
            Message::showError(
                __CLASS__ . '|' . __FUNCTION__,
                ['warning' => ['return' => $rs], 'backtrace' => Utils::stackTrace(), 'abort' => 'find()'],
                770050
            );
            exit;
        }
        
        // /// 结果 is_null, 正常返回
        // if (is_null($rs)) {
        //     return null;
        //     // Message::showSucc('null.succ.find.gets.predict', $rs);
        //     // exit;
        // }
        
        /// 常规预期的结果, 返回一条文档 
        //// find() 返回多条才需要 toArray(); findOne(), 只返回一条不需要 toArray()
        $rs = json_decode(json_encode($rs->toArray()), true);
        return $rs;
    }
```









## FAQ



## See Also





