# ডিপেন্ডেন্সি ইনজেকশন ডিজাইন প্যাটার্নঃ

ডিপেন্ডেন্সি ইনজেকশন স্ট্রাকচারাল ডিজাইন প্যাটার্নের মধ্যে পরে। এইটা এমন একটা ডিজাইন প্যাটার্ন যা কোন ক্লাসের ডিপেন্ডেন্সি অর্থাৎ প্রয়োজনীয় অবজেক্ট গুলোকে রান টাইম কিংবা কম্পাইল টাইমে সহজে পরিবর্তনে সহায়তা করে।

এই প্যাটার্নের মূল উদ্দেশ্যই হল লুজলি কাপল আর্কিটেকচার ইমপ্লিমেন্ট করা যাতে করে একটা ভাল মানের অ্যাপ তৈরি করা যায়।

ডিপেন্ডেন্সি ইনজেকশন ডিজাইন প্যাটার্ন হল ```S.O.L.I.D Principle``` এর ```D``` যার পূর্ণ অর্থ ```Dependency Inversion Principle (DIP)``` যেটি ```Inversion of Control (IoC)``` কে অনুসরণ করে।

এখানে ```Dependency Inversion Principle``` বলতে ```Decoupling``` করাকে বুঝানো হয় আর ```Inversion of Control``` বলতে কিভাবে ডিপেন্ডেন্সি রিজল্ভ করা হবে সেটিকে বুঝায়। ডিপেন্ডেন্সি রিজল্ভ করতে ```Dependency Injection (DI) Container``` বা ```Inversion of Control (IoC) Container``` ব্যাবহৃত হয়ে থাকে।

আমরা সাধারণত একটি ক্লাসে অন্য ক্লাসের অবজেক্ট ব্যাবহার করলে নিচের মত করে হার্ডকোড করি যা হাইলি কাপল্ড থাকে।

```php
class Database
{
    protected $adapter;

    public function __construct()
    {
        $this->adapter = new MySqlAdapter;
    }
}

class MysqlAdapter
{

}
```

আর ডিপেন্ডেন্সি ইনজেকশন ডিজাইন প্যাটার্নে কোন ক্লাস কিংবা ইন্টারফেইসকে টাইপ হিন্ট করে কনস্ট্রাক্টর কিংবা মেথডে ইঞ্জেক্ট করতে হয় নিচের মত করে।

```php
class Database
{
    protected $adapter;

    public function __construct(MySqlAdapter $adapter)
    {
        $this->adapter = $adapter;
    }
}

class MysqlAdapter
{

}
```

উপরে ```MySqlAdapter``` ক্লাসকে ডিপেন্ডেন্সি হিসেবে রাখা হয়েছে। আর এই ডিপেন্ডেন্সি রিজল্ভ করতে হলে অবশ্যই ```MySqlAdapter``` এর অবজেক্ট কন্সট্রাক্টরের প্যারামিটারে দিতে হবে।

যেমনঃ
```php
$mysqlAdapter = new MysqlAdapter;
$database = new Database($mysqlAdapter);
```

ডিপেন্ডেন্সি প্রধানত তিন ভাবে ইনজেক্ট করা যায়।

## ১. কন্সট্রাক্টর ইনজেকশনঃ
যা কনস্ট্রাক্টরের মাধ্যমে ইঞ্জেক্ট করা হয়।

```php
public function __construct(MySqlAdapter $adapter)
{
    $this->adapter = $adapter;
}
```

## ২. সেটার ইনজেকশনঃ
যা কোন মেথডের প্যারামিটারে ইঞ্জেক্ট করা হয়।

```php
public function setterMethod(MySqlAdapter $adapter)
{
    $this->adapter = $adapter;
}
```

## ৩. ইন্টারফেইস ইনজেকশনঃ
ইন্টারফেইসকে কোন কনস্ট্রাক্টরে অথবা সেটার মেথডে ইঞ্জেক্ট করা হয়।

```php
public function __construct(AdapterInterface $adapter)
{
    $this->adapter = $adapter;
}
```

আমরা আমাদের প্রজেক্টে ডিপেন্ডেন্সি গুলোকে স্বয়ংক্রিয় ভাবে ইঞ্জেক্ট কিংবা রিজল্ভ করতে ডিপেন্ডেন্সি ইনজেকশন কন্টেইনার ব্যাবহার করব যা আগেই উল্লেখ করেছি।
অনেক ফ্রেমওয়ার্কে এই কন্টেইনার সাধারণত বিল্ট-ইন দেয়া থাকে যেমনঃ ```Symfony```, ```Laravel```, ```Yii```

আমরা সাধারণ প্রজেক্টের ক্ষেত্রে ```Pimple``` নামে কন্টেইনারটি ব্যাবহার করতে পারি।
আবার আমি আমার কাজের জন্য খুব সহজ এবং অপ্টিমাইজ একটা কন্টেইনার বানিয়েছিলাম আপনারা চাইলে সেটি দেখতে পারেন [এই লিঙ্ক](https://github.com/appzcoder/container) থেকে। আশাকরি সোর্স কোড ও ডকুমেন্টেশন থেকে আপনারা ভাল ধারণা পাবেন।

এই চ্যাপ্টারের সোর্স কোডটি [এই লিঙ্ক](https://github.com/sohelamin/php-design-patterns) থেকে পাবেন।