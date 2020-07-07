# ইনহেরিট্যান্স

আমরা যেমন আমাদের বাবা-মার গুনাবলী বংশানুক্রমিকভাবে পাই, তেমনি ভাবে পিএইচপিতে ও একটি ক্লাস অন্য আরেকটি ক্লাস কে এক্সটেন্ড করে তার সব প্রোপার্টি বা মেথড ব্যবহার করতে পারে । এটাই ইনহেরিট্যান্স । একটি সহজ উদাহরন দেখি:

```php
<?php

class ParentClass
{
    public $name;

    public function getName()
    {
        return $this->name;
    }
}


class ChildClass extends ParentClass
{

}

$child = new ChildClass();
$child->name = "Abul";

var_dump($child->getName());
```

এখানে লক্ষ্য করুন `ChildClass` টি `ParentClass` কে এক্সটেন্ড করেছে । এর ফলে `ChildClass` এ আমরা `name` বা `getName()` ডিফাইন না করলেও `ParentClass` থেকে সে এই প্রোপার্টি এবং মেথড এ্যাক্সেস করতে পারছে । এটাই সহজ ভাষায় ইনহেরিট্যান্স । এক্ষেত্রে আমরা বলতে পারি, `ChildClass` টি `ParentClass` কে ইনহেরিট করেছে । এখানে আমরা `extends` কিওয়ার্ডটি ব্যবহার করে বলে দেই কোন ক্লাসটি এক্সটেন্ড করছে আর কোনটি থেকে এক্সটেন্ড করা হচ্ছে । যেই ক্লাস টি এক্সটেন্ড করে, সেটিকে চাইল্ড ক্লাস এবং যেটি থেকে এক্সটেন্ড করা হয় সেটিকে প্যারেন্ট ক্লাস বলি আমরা । একটি ক্লাস যখন আরেকটি ক্লাস কে এক্সটেন্ড করে তখন প্যারেন্ট ক্লাস এর সব প্রোপার্টি এবং মেথডই চাইল্ড ক্লাস না ডিফাইন করলেও এ্যাক্সেস করতে পারবে ।

এখানে `ChildClass` এর `name` এবং `getName()` যে `ParentClass` থেকেই এসেছে তা এই উদহরনটি থেকে আরও পরিস্কারভাবে বোঝা যাবে:

```php
<?php

class ParentClass
{
    public $name = "Name of The ParentClass";

    public function getName()
    {
        return $this->name;
    }
}


class ChildClass extends ParentClass
{

}

$child = new ChildClass();
var_dump($child->getName());
```

এখানে দেখুন, আমরা `$name` এর ভ্যালু `ParentClass` এ ইনিশিয়ালাইজ করেছি । `ChildClass` হুবহু সেই ভ্যালুই গ্রহন করেছে । সুতরাং কোন সন্দেহ নেই যে এটি ইনহেরিটেন্স এরই ফল!

