# এ্যাবস্ট্রাকশন

কিছু ক্লাসকে আমরা এ্যাবস্ট্রাক্ট হিসেবে ডিক্লেয়ার করতে পারি । এসব ক্লাস থেকে সরাসরি অবজেক্ট ইনস্ট্যান্স তৈরি করা সম্ভব হয় না । কিন্তু এদের কে ইনহেরিট করা সম্ভব । কোন ক্লাসের একটি মেথড এ্যাবস্ট্রাক্ট হলে সেটিকে এ্যাবস্ট্রাক্ট ক্লাস হিসেবে ডিক্লেয়ার করতে হবে ।

এ্যাবস্ট্রাক্ট মেথড গুলোর বেলায় শুধুই মেথড সিগনেচার ডিফাইন করে দিতে হয় । মূল ইম্প্লিমেন্টেশন দেওয়া হয় না । প্যারেন্ট ক্লাসে ডিফাইন করা সকল এ্যাবস্ট্রাক্ট মেথড অবশ্যই চাইল্ড ক্লাসে ইম্প্লিমেন্ট করতে হবে । এসময় ভিজিবিলিটি একই অথবা বেশী ওপেন \(প্রাইভেট থাকলে প্রাইভেট কিংবা প্রটেক্টেড, প্রটেক্টেড থাকলে প্রটেক্টেড কিংবা পাবলিক\) রাখা আবশ্যক । একই সাথে ফাংশনের সিগনেচারও ম্যাচ করতে হবে, আপনি চাইলেই চাইল্ড ক্লাসে কোন মেথডের একটি প্যারামিটার যোগ বা বাদ দিতে পারবেন না ।

এ্যাবস্ট্রাক্ট ক্লাস অনেকটা ইন্টারফেইসের মত শুধু এখানে শুধু মাত্র নির্দিষ্ট মেথড গুলো আমরা এ্যাবস্ট্রাক্ট রেখে বাকি মেথডগুলোর ইম্প্লিমেন্টেশন তৈরি করে দিতে পারি ।

```php
<?php
abstract class AbstractClass
{
    // Force Extending class to define this method
    abstract protected function getValue();
    abstract protected function prefixValue($prefix);

    // Common method
    public function printOut() {
        print $this->getValue() . "\n";
    }
}

class ConcreteClass1 extends AbstractClass
{
    protected function getValue() {
        return "ConcreteClass1";
    }

    public function prefixValue($prefix) {
        return "{$prefix}ConcreteClass1";
    }
}

class ConcreteClass2 extends AbstractClass
{
    public function getValue() {
        return "ConcreteClass2";
    }

    public function prefixValue($prefix) {
        return "{$prefix}ConcreteClass2";
    }
}

$class1 = new ConcreteClass1;
$class1->printOut();
echo $class1->prefixValue('FOO_') ."\n";

$class2 = new ConcreteClass2;
$class2->printOut();
echo $class2->prefixValue('FOO_') ."\n";
?>
```

এখানে একই এ্যাবস্ট্রাক্ট ক্লাস থেকে আমরা দুটি ক্লাস তৈরি করেছি । এবং প্রত্যেকটি সাবক্লাসে আমরা এ্যাবস্ট্রাক্ট মেথডগুলো নিজেদের মত করে ইম্প্লিমেন্ট করেছি । কিন্তু `printOut()` মেথডটি মূল ক্লাসেই ডিফাইন করা ।

