# সিঙ্গেলটোন ডিজাইন প্যাটার্নঃ

সিঙ্গেলটোন ডিজাইন প্যাটার্ন **ক্রিয়েশনাল ডিজাইন প্যাটার্ন** ক্যাটাগরির মধ্যে পরে।
এই প্যাটার্নের মুল উদ্দেশ্য হল প্রতিটি ক্লাসের শুধু মাত্র একটিই **ইন্সট্যান্স/অবজেক্ট** থাকা।

ধরুন, <strong>Singleton</strong> নামে আমাদের একটা ফাইনাল ক্লাস আছে তাহলে সিঙ্গেলটোন প্যাটার্নে এই ক্লাসকে এমনভাবে ব্যবহার করতে হবে যেন নতুন কোন <strong>ইন্সট্যান্স/অবজেক্ট</strong> তৈরি না হয়ে একটিই থাকে আর ক্লাসটিকে ইনহেরিট ও করা না যায়, যা আমরা নিচের মত করে করতে পারিঃ

```php
<?php

final class Singleton
{
    private static $instance;

    public static function getInstance()
    {
        if (null === self::$instance) {
            self::$instance = new self();
        }

        return self::$instance;
    }

    private function __construct()
    {

    }

    private function __clone()
    {

    }

    private function __wakeup()
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

আবার ক্লাসের একাধিক ইন্সস্ট্যান্স তৈরিতে বাধা দিতে আমরা ```__clone()``` ও ```__wakeup()``` ম্যাজিক মেথডগুলি ব্যাবহার করেছি।

[এই লিঙ্ক](https://github.com/sohelamin/php-design-patterns) থেকে কোডটি পাবেন।