title: Yii 根据公司id来产品分表，以及读取产品表时的一些操作
date: '2020-03-03 11:43:36'
updated: '2020-03-03 13:47:29'
tags: [PHP, mysql, Yii]
permalink: /articles/2020/03/03/1583207015907.html
---
![](https://img.hacpai.com/bing/20180314.jpg?imageView2/1/w/960/h/540/interlace/1/q/100)

## 场景描述

###### 由于业务需求要存储很多家的公司以及公司的产品详情，那我就想着根据公司的 id 来建立一一对应的产品详情表。这样便于管理，也分摊了只有一张产品表的查询压力，也会优化查询的速度。

###### 当然如果一个公司的产品足够多，那么也要对于该公司对应的产品表进行分表。这时分表的核心思想可以采取对产品 id 取余的方式存储、读取。这个方式在这篇笔记里就不详细说明了。

## 开发环境

 `PHP` `Yii` `MySql` 

## 如何创建产品表

#### 在添加一个公司的信息时自动创建公司对应的产品表

```php
//新增一个公司信息，返回公司id
$companyId= $this->CompanyModel()->add($data);
if (false !== $companyId) {
    //创建公司对应的产品表
    ProductTable::create($companyId);
    Tools::sendJson(0, "添加成功", \Yii::$app->urlManager->createUrl(['company/lists']));
} else {
    Tools::sendJson(40000205, "添加失败");
}
```

#### ProductTable::create($companyId) 方法的实现

```php
class ProductTable
{
    static public function create($companyId){
        $tableName = 'test_product_'.$companyId;
        $sql = <<<EOT
CREATE TABLE if not exists `$tableName` (
  `id` int(11) unsigned NOT NULL AUTO_INCREMENT,
  `cid` int(11) unsigned NOT NULL COMMENT '公司id',
  `name` varchar(30) NOT NULL DEFAULT '' COMMENT '产品名称',
  `barcode` varchar(30) NOT NULL DEFAULT '' COMMENT '条码',
  `image` varchar(225) NOT NULL DEFAULT '' COMMENT '产品图片',
  `company_classify_id` int(11) unsigned NOT NULL COMMENT '公司分类id',
  `size` varchar(20) NOT NULL COMMENT '规格',
  `expire_date` date NOT NULL COMMENT '保质期',
  `production_date` datetime NOT NULL COMMENT '生产日期',
  `batch_num` varchar(20) DEFAULT NULL COMMENT '生产批次',
  `status` tinyint(2) unsigned NOT NULL COMMENT '状态：0，停用；1，启用',
  `classify_id` int(11) unsigned NOT NULL COMMENT '系统分类id',
  `create_time` datetime DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `update_time` datetime DEFAULT CURRENT_TIMESTAMP COMMENT '更新时间',
  `created_id` int(11) unsigned DEFAULT NULL COMMENT '创建者id',
  `updated_id` int(11) unsigned DEFAULT NULL COMMENT '更新者id',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8mb4 COMMENT='公司产品信息表';
EOT;

        $connection = Yii::$app->db;
        $command = $connection->createCommand($sql);
        $rowCount = $command->execute();
        return $rowCount;

    }

}
```

## 如何调用产品表，将产品存入对应公司的产品表

#### 当添加产品信息时

```php
$data = array(
            'cid' => $companyId,
            'name' => $name,
            'barcode' => $barcode,
            'image' => $res['url'],
            'company_classify_id' => $company_classify_id,
            'classify_id' => $classify_id,
            'size' => $size,
            'expire_date' => $expire_date,
            'production_date' => $production_date,
            'created_id' => Yii::$app->user->id,
            'status' => $status,
        );
$result = $this->ProductModel($companyId)->add($data);
```

#### ProductModel($companyId)的实现

```php
 protected function ProductModel($companyId){
        ProductModel::resetPartitionIndex($companyId);
        return new ProductModel();
  }
```

#### ProductModel::resetPartitionIndex($cid)的实现以及数据表的操作

```php
/**
 * This is the model class for table "{{%test_product}}".

 */
class ProductModel extends \common\PopToolActiveRecord
{

    static private $partitionIndex;


    /**
     * 初始化 partitionIndex
     * @param $companyId
     */
    static public function resetPartitionIndex($companyId){
        self::$partitionIndex = '_'.$companyId;
    }

    /**
     * 将表名拼接上公司id
     * @inheritdoc
     */
    public static function tableName()
    {
        return '{{test_product'.self::$partitionIndex.'}}';
    }


    /**
     * 添加
     */
    public function add($data = []) {
        $mix_result = false;
        if(true == empty($data)) {
            return $mix_result;
        }
        PopToolActiveRecord::beginTransaction();
        try {

            $result = $this->setData($data)->insert();
            if (false == $result) {
                throw new \Exception("添加信息失败，请重试", 1004001);
            }
            $int_adves_id = (int)$this->id;

            PopToolActiveRecord::commit();
            $mix_result = $int_adves_id;
        } catch(\Exception $e) {
            PopToolActiveRecord::rollback();
            $mix_result = false;
        }

        return $mix_result;
    }


   /*下面是具体对tableName进行增删改查的函数实现就不一一列出了*/
```

- ps：以前的写业务时遇到的问题以及处理方案，拿出来做一下笔记。
