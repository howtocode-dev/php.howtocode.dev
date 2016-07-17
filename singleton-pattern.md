# সিঙ্গেলটোন ডিজাইন প্যাটার্নঃ

সিঙ্গেলটোন ডিজাইন প্যাটার্ন **ক্রিয়েশনাল ডিজাইন প্যাটার্ন** ক্যাটাগরির মধ্যে পরে।
এই প্যাটার্নের মুল উদ্দেশ্য হল প্রতিটি ক্লাসের শুধু মাত্র একটিই **ইন্সট্যান্স/অবজেক্ট** থাকা।

ধরুন, **Singleton** নামে আমাদের একটা ক্লাস আছে তাহলে সিঙ্গেলটোন প্যাটার্নে এই ক্লাসকে এমনভাবে ব্যবহার করতে হবে যেন নতুন কোন **ইন্সট্যান্স/অবজেক্ট** তৈরি না হয়ে একটিই থাকে আর এই কাজটি আমরা নিচের মত করে করতে পারিঃ

```php
<?php

class Singleton
{
    private static $instance;

    public static function getInstance()
    {
        if (null === self::$instance) {
            self::$instance = new self();
        }

        return self::$instance;
    }

    public function __construct()
    {

    }

    public function sayHi()
    {
        echo 'Hi';
    }
}

$singleton = Singleton::getInstance();

$singleton->sayHi();

```

এখানে ক্লাসটি বাইরে থেকে ইন্সট্যান্সিয়েট না করে ```getInstance()``` স্ট্যাটিক মেথডটি ডিক্লেয়ার করা হয়েছে যাতে ক্লাসের ইন্সট্যান্সটা রিটার্ন করে।

অর্থাৎ,
```php
$singleton = new Singleton();
```
এর পরিবর্তে
```php
$singleton = Singleton::getInstance();
```
ব্যবহার করা হয়েছে।

আর ক্লাসের ইন্সট্যান্স ```$instance``` নামে ভ্যারিয়েবল এ রাখা হয়েছে।

যেমনঃ
```php
private static $instance;

public static function getInstance()
{
    if (null === self::$instance) {
        self::$instance = new self();
    }

    return self::$instance;
}
```

[এই লিঙ্ক](https://github.com/sohelamin/php-design-patterns/blob/master/Singleton.php) থেকে কোডটি পাবেন।