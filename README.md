### Simple create direct pay process via alipay api

This is the tree view of the sample files(php, utf-8).
```
.
|-- alipay.config.php       // basic configuration
|-- alipayapi.php           // alipay api
|-- cacert.pem              // certificate used when verifying via curl whether the notify request is from alipay
|-- index.php               // order page on your site
|-- lib
|   |-- alipay_core.function.php    // alipay public function
|   |-- alipay_md5.function.php     // alipay md5 encryption function
|   |-- alipay_notify.class.php     // aliapy trade status notification class
|   `-- alipay_submit.class.php     // alipay request form builder class
|-- log.txt                         // logging debug info when verifying notification request
|-- notify_url.php                  // asynchronous notification url called by alipay after users pay
|-- readme.txt
|-- return_url.php                  // synchronous return url after users pay
```

### Configuration

In `alipay.config.php` file, where we need to set:

```php
// partner id; starts with 2088; integer; 16 chars
$alipay_config['partner']       = '2088434434372834';

// alipay account
$alipay_config['seller_email']  = 'admin@besticecream.com';

// scurity key; comprised of integers and letters; 32 chars
$alipay_config['key']           = 'dk2938rjd8dkejwisj19ekdjwisk10sj';
```

In `alipayapi.php` file, where we need to set:
```php
// asynchronous request made by alipay after users pay;
$notify_url = "http://www.besticecream.com/.../notify_url.php";

// synchronous request(page redirect) after users pay
// no parameters in url; no `http://localhost/` is allowed
$return_url = "http://www.besticecream.com/.../return_url.php";
```

### Payment process

1. Users open `index.php` file, fill out the form, submit data to `alipayapi.php`; The form data is as below:
```javascript
// required; unique order number on your site
WIDout_trade_no = "12345"

// required; subject of this trade
WIDsubject      = "best icecream"

// required; total fee of this trade
WIDtotal_fee    = "1000"

// optional; body message of this trade
WIDbody         = "vip customer"

// optional; product url
WIDshow_url     = "http://www.besticecream.com/items/134324.php"
```

2. Then we build the request to alipay on `alipayapi.php` page;
    - Get the configuration `alipay.config.php`, get form construction class `lib/aliapy_submit.class.php`;
    - Build the form and trigger a submission via javascript;
3. Users complete payment on alipay, then page jumps to the `return_url.php`;
4. Alipay `POST` following data to `notify_url.php`; `echo` exactly a string `success` to alipay;
```javascript
//  the unique trade number on your site
out_trade_no = "12345"

// alipay trade number
trade_no = "43982574893247382"

// trade status string; `TRADE_FINISHED` or `TRADE_SUCCESS`
trade_status = "TRADE_SUCCESS"
```





