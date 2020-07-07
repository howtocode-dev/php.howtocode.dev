# ডেকোরেটর

`Decorator` ডিজাইন প্যাটার্ন স্ট্রাকচারাল ডিজাইন প্যাটার্নের মধ্যে পরে। `Decorator` শব্দটি শুনলেই আমরা বুঝতে পারছি যে এটি কোন কিছুর প্রসাধক হিসেবে কাজ করে থাকে।

অবজেক্ট ওরিয়েন্টেডের ক্ষেত্রে ডেকোরেটর একটি নির্দিষ্ট অবজেক্টকে স্টাটিক্যালি অথবা ডায়নামিক্যালি সংযুক্তি বা পরিবর্তন করে থাকে।

এখন প্রশ্ন আসতে পারে আমরা কোন ক্লাসকে ইনহেরিট করেই তো এই কাজটি করতে পারি তাহলে কেন ডেকোরেটর ব্যাবহার করবো? ইনহেরিট্যান্স এর মাধ্যমে আমরা একটা ক্লাসকে পরিবর্তন করে থাকি তার মানে সাবক্লাস দিয়ে আমরা যতগুলো অবজেক্ট তৈরি করবো সবগুলাই সেইম হবে। অন্যদিকে ডেকোরেটর আমাদেরকে এই ক্ষেত্রে শুধুমাত্র কোন নির্দিষ্ট অবজেক্টে পরিবর্তন করতে ফ্লেক্সিবিলিটি দিয়ে থাকে।

এবার চলুন একটা উদাহরণ দেখা যাক।

```php
interface EmailInterface
{
    public function body();
}

class Email implements EmailInterface
{
    public function body()
    {
        return 'Simple email body.';
    }
}

abstract class EmailDecorator implements EmailInterface
{
    public $email;

    public function __construct(EmailInterface $email)
    {
        $this->email = $email;
    }

    abstract public function body();
}

class NewYearEmailDecorator extends EmailDecorator
{
    public function body()
    {
        return $this->email->body() . ' Additional text from deocorator.';
    }
}
```

উপরের কোডে খেয়াল করলে দেখতে পাবেন ইমেইল পাঠানোর জন্য একটি মুল ইন্টারফেইস `EmailInterface` আর এর কনক্রিট ক্লাস `Email` ইমপ্লিমেন্ট করা হয়েছে যা দিয়ে আমরা সিম্পল ইমেইল করতে পারি।

এবার ডেকোরেটর এর জন্য `EmailDecorator` অ্যাবস্ট্রাক্ট ক্লাস ডিফাইন করেছি আর এইটা কে ইনহেরিট করে `NewYearEmailDecorator` কনক্রিট ক্লাস ডিফাইন করেছি যার মাধ্যমে আমরা খুব সহজেই ইমেইলের অবজেক্টকে পরিবর্তন/সংযুক্তি করতে পারবো।

আমরা চাইলে `EmailDecorator` অ্যাবস্ট্রাক্ট ক্লাসটি ইনহেরিট করে আরও ক্লাস ডিফাইন করতে পারি।

এবার নিচের কোডটি দেখলে বুঝতে পারবেন কিভাবে অবজেক্টকে ডেকোরেট করা হয়েছে।

```php
// Simple Email
$email = new Email();
var_dump($email->body());

// Decorated Email
$emailNewYearDecorator = new NewYearEmailDecorator($email);
var_dump($emailNewYearDecorator->body());
```

এই চ্যাপ্টারের সোর্স কোডটি [এই লিঙ্ক](https://github.com/sohelamin/php-design-patterns) থেকে পাবেন।

