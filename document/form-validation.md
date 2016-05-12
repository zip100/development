form-validation 插件简介和应用
--
>	简介：在项目中数据验证是一个很常见的需求，在表单提交时候校验用户的输入显得尤为重要，传统框架中验证字段都绑定在数据库字段上，这种做法在涉及到多表插入等比较复杂的操作时候显得力不从心，在这种需求下结合实际的开发需求编写了此插件，在经过三个版本的迭代之后，最终的版本最小化版本诞生了，插件中内置了基本的验证规则，似乎比较少，但是这完全没有关系，在配置文件中可以很简单的扩展验证规则，而完全不需要修改插件源码。这种松散的设计模式，最终让验证框架本身和业务规则隔离开来，使用者只需要关注输入的参数和得到的结果，而无需关注具体的实现过程

*	Author: GuoSI <memcache@qq.com>
* 	QQ:409416084
*	Github: [form-validation](https://github.com/zip100/form-validation)

基本使用：
---

```
// 数据源
$post = array(
    'name' => 'mike',
    'age' => 28,
    'mobile' => '18610009876',
    'email' => 'memcache@qq.com'
);

// 验证配置
$config = array(
    // 错误语言包
    'error' => array(
        'require' => '为必填项',
        'size' => "长度为%d到%d位",
        'email' => '格式错误',
        'mobile' => '格式错误',
        'tel' => '格式错误',
        'digital' => '格式错误',
        'money' => '格式错误',
        'maxSize' => '长度不能超过%d个字符',
        'minSize' => '长度不能少于%d个字符',
        'max' => '不能大于%d',
        'min' => '不能小于%d'
    ),
    // 是否显示全量错误
    'showAllReport' => true,
    // 自定义业务规则
    'rule' => array(
        'isChild' => array(
            'callable' => function($data,$post,$params){
                return $data < 18;
            },
            'error' => '格式错误'
        ),
        'isDisabled' => array(
            'callable' => function($data,$post,$params){
                throw new \Exception('抛异常弹出错误！');
            },
            'error' => '格式错误'
        )
    )
);

// 验证规则

$rule = array(
    'name' => array(
        'lable' => '名称',
        'rule' => 'require|size[1,32]'
    ),
    'age' => array(
        'lable' => '年纪',
        'rule' => 'require|digital|isChild'
    ),
    'mobile' => array(
        'lable' => '电话',
        'rule' => 'require|mobile'
    ),
    'email' => array(
        'lable' => '邮件',
        'rule' => 'require|email|isDisabled'
    ),
);

$instance = new Validation($config);


$result = $instance->rule($rule)->check($post);
if (! $result) {
    $error = $instance->getReport();
    echo '<pre>';var_dump($error);die('</pre>');
}
echo "pass";
```