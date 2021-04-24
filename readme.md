<p align="center">
    <img src="https://payjs.cn/static/images/logo.png" width=80 />
</p>
<h2 align="center">PAYJS Wechat Payment Laravel Package</h2>

## 简介
本项目是基于 PAYJS 的 API 开发的 Laravel Package，可直接用于生产环境

PAYJS 针对个人主体提供微信/支付宝支付接入能力，是经过检验的正规、安全、可靠的微信支付个人开发接口

其它版本: [PAYJS 通用开发包](https://github.com/nonfu/payjs)

支持Laravel 5.x、Laravel 6.x、Laravel 7.x、Laravel 8.x


## 安装

通过 Composer 安装

```bash
$ composer require nonfu/payjs-laravel
```

## 使用方法

### 一、发布并修改配置文件

- 发布配置文件
```shell
php artisan vendor:publish --provider="Nonfu\Payjs\PayjsServiceProvider"
```
- 编辑配置文件 `config/payjs.php` 配置商户号和通信密钥
```php
return [
    'wechat' => [
        'mchid' => '', // 填写商户号
        'key'   => '', // 填写通信KEY
    ],
    'alipay' => [
        'mchid' => '', // 填写商户号
        'key'   => '', // 填写通信KEY
    ]
];
```

### 二、在业务中使用

首先在业务模块中引入门面

```php
use Nonfu\Payjs\Facades\Payjs;
```

以扫码支付为例：

```php
// 构造订单基础信息
$data = [
    'body' => '订单测试',                                // 订单标题
    'total_fee' => 2,                                   // 订单标题
    'out_trade_no' => time(),                           // 订单号
    'attach' => 'test_order_attach',                    // 订单附加信息(可选参数)
    'notify_url' => 'https://www.baidu.com/notify',     // 异步通知地址(可选参数)
];
Payjs::setPayType('wechat');
return Payjs::native($data);

// 支付宝
$data = [
    'body' => '订单测试',   
    'type' => 'alipay',                                 // 支付方式
    'total_fee' => 2,                                   // 订单标题
    'out_trade_no' => time(),                           // 订单号
    'attach' => 'test_order_attach',                    // 订单附加信息(可选参数)
    'notify_url' => 'https://www.baidu.com/notify',     // 异步通知地址(可选参数)
];
Payjs::setPayType('alipay');
return Payjs::native($data);
```