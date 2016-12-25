# ফ্যাসাড ডিজাইন প্যাটার্নঃ

```Facade``` ডিজাইন প্যাটার্ন স্ট্রাকচারাল ডিজাইন প্যাটার্নের মধ্যে পরে।
ফ্যাসাড ডিজাইন প্যাটার্ন এর কাজ হল ক্লায়েন্ট এর কাছে একটা কমপ্লেক্স সিস্টেম বা ইন্টারফেইস হতে একটা সহজ ইন্টারফেইস প্রদান করা যাতে কমপ্লেক্স কিংবা আগলি কোড গুলো হিডেন অবস্থায় থাকে।

লেগাসি কিংবা কমপ্লেক্স কোন সিস্টেম এর কোডকে সহজ ভাবে উপস্থাপন করার দরকার হলে এই প্যাটার্ন ব্যাবহার করা হয়।

ধরুন আপনার সিস্টেম কিংবা অ্যাপ্লিকেশনে একটা কমপ্লেক্স লাইব্রেরি ব্যাবহার করার দরকার পরতেছে আর আপনি উক্ত কমপ্লেক্স পার্টকে সহজ করে তোলার জন্য একটা ```Wrapper``` বানিয়ে সেইটা করতে পারেন।

এবার নিচে একটা উধাহরনের মাধ্যমে প্যাটার্নটি বোঝানোর চেষ্টা করা হলঃ

```php
<?php

class Cart
{
    public function addProducts($products)
    {
        // Product adding codes goes here
    }

    public function getProducts()
    {
        // Product retrieval codes goes here
    }
}

class Order
{
    public function process($products)
    {
        // Order processing codes goes here
    }
}

class Payment
{
    public function charge($charge)
    {
        // Additional charge codes goes here
    }

    public function makePayment()
    {
        // Payment method verify & payment codes goes here
    }
}

class Shipping
{
    public function calculateCharge()
    {
        // Calculation codes goes here
    }

    public function shipProducts()
    {
        // Ship process codes goes here
    }
}

class CustomerFacade
{
    public function __construct()
    {
        $this->cart = new Cart;
        $this->order = new Order;
        $this->payment = new Payment;
        $this->shipping = new Shipping;
    }

    public function addToCart($products)
    {
        $this->cart->addProducts($products);
    }

    public function checkout()
    {
        $products = $this->cart->getProducts();

        $this->totalAmount = $this->order->process($products);
    }

    public function makePayment()
    {
        $charge = $this->shipping->calculateCharge();
        $this->payment->charge($charge);

        $isCompleted = $this->payment->makePayment();

        if ($isCompleted) {
            $this->shipping->shipProducts();
        }
    }
}

$customer = new CustomerFacade;

$products = [
    [
        'name' => 'Polo T-Shirt',
        'price' => 40,
    ],
    [
        'name' => 'Smart Watch',
        'price' => 400,
    ],
];

$customer->addToCart($products);
$customer->checkout();
$customer->makePayment();
```

উপরের কোডটি খেয়াল করলে দেখতে পারবেন এখানে একটা ই-কমার্স অ্যাপ্লিকেশনের প্রসেস দেখানো হয়েছে।
এর জন্য আমরা যথাক্রমে ```Cart```, ```Order```, ```Payment```, ```Shipping``` ক্লাসগুলো ব্যাবহার করেছি আর ফ্যাসাড হিসেবে ```CustomerFacade``` ক্লাস ব্যাবহার করেছি। 
এখানে কোন প্রোডাক্টকে কার্টে যুক্ত করার জন্য ```Cart``` ক্লাসটি, অর্ডার প্রসেস করার জন্য ```Order``` ক্লাসটি, পেমেন্ট প্রসেস করার জন্য ```Payment``` ক্লাসটি আর প্রডাক্ট এর শিপিং হ্যান্ডল করার জন্য ```Shipping``` ক্লাসটি ব্যাবহার করেছি।

এখন মুল কথা হল আমরা যদি এসব কাজের জন্য প্রতিবার উক্ত ক্লাস গুলোকে বার বার কল করি তাহলে অনেক সময় সাপেক্ষ বেপার হয়ে পরবে আর স্ট্রাকচারটিও ভাল হবেনা। আর তাই এখানে ফ্যাসাড প্যাটার্নটি ব্যাবহার করা হয়েছে। যাতে ডেভেলপার কিংবা ক্লায়েন্ট হিসেবে সুধু মাত্র ```CustomerFacade``` ক্লাসটিকে ব্যাবহার করে উপরে উল্লেখিত সবগুলো কাজ অনায়াসে করা সম্ভব।


## অতিরিক্ত বিষয় (লারাভেল ফ্যাসাড) ঃ
আমরা যারা লারাভেল ব্যাবহার করি তারা কম বেশি সবাই জানি লারাভেল এ অনেকগুলো বিল্ড-ইন ফ্যাসাড আছে কিংবা ব্যাবহার হয়। যেমনঃ ```DB```, ```View```, ```Event```, ```Queue```, ```Mail``` ইত্যাদি।

আমরা মূলত যেটা জানি তা হল লারাভেল এ ফ্যাসাড ```Statically``` কোন ক্লাসকে কল করার জন্য ব্যাবহার হয়। আসলে বিষয়টি ঠিক তেমন নয়।
এখানে অনেক কমপ্লেক্স সিস্টেমকে হাইড করে আমাদের কাছে সহজ ভাবে উপস্থাপন করা হয়েছে তার সাথে লারাভেল ফ্যাসাড প্যাটার্নের সাথে ```__callStatic``` ম্যাজিক মেথডটি ব্যাবহার করা হয়েছে।
যাতে ডেভেলপার কিংবা ক্লায়েন্টকে আলাদাভাবে ক্লাস ইন্সট্যানশিয়েট করতে না হয়।

নিচে একটা উধাহরন দেয়া হলঃ

```php
<?php

class Person
{
    public function getFullName()
    {
        return 'Sohel Amin';
    }
}

class PersonFacade
{
    static $instance;

    public static function __callStatic($method, $args)
    {
        if (null === static::$instance) {
            static::$instance = new Person;
        }

        $instance = static::$instance;

        switch (count($args)) {
            case 0:
                return $instance->$method();
            case 1:
                return $instance->$method($args[0]);
            case 2:
                return $instance->$method($args[0], $args[1]);
            case 3:
                return $instance->$method($args[0], $args[1], $args[2]);
            case 4:
                return $instance->$method($args[0], $args[1], $args[2], $args[3]);
            default:
                return call_user_func_array([$instance, $method], $args);
        }

    }
}

var_dump(PersonFacade::getFullName());
```

এই চ্যাপ্টারের সোর্স কোডটি [এই লিঙ্ক](https://github.com/sohelamin/php-design-patterns) থেকে পাবেন।
