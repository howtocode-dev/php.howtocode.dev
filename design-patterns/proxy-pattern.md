# প্রক্সি

`Proxy` ডিজাইন প্যাটার্ন স্ট্রাকচারাল ডিজাইন প্যাটার্নের মধ্যে পরে। এই প্যাটার্ন শুরুর আগে আসুন আমরা “প্রক্সি” শব্দের অর্থ জেনে নেই। প্রক্সি এমন একটি প্রতিনিধি বা বস্তু যা অন্য বিষয় বস্তুর হয়ে কাজ করে।

অবজেক্ট অরিয়েন্টেড প্রোগ্রামিং এ প্রক্সি হলঃ একটি অবজেক্ট অন্য কোন অবজেক্টের হয়ে কাজ করা বা তাকে কন্ট্রোল করা।

প্রক্সি সাধারণত ৩ প্রকারেরঃ 1. Virtual Proxy: এই প্রক্সি মুল অবজেক্টকে ইন্সটানশিয়েট বা ইনিশিয়ালাইজ করতে বিলম্ব করে যতক্ষণ না দরকার পরে। 2. Remote Proxy: এই প্রক্সি কোন রিমুট লোকেশনে অবস্থিত কোন অবজেক্টকে রিপ্রেজেন্ট করে। যেমনঃ সার্ভার থেকে কোন অবজেক্টকে অ্যাকসেস করা। 3. Protection Proxy: এই প্রক্সি মুল অবজেক্টকে অ্যাকসেস করার আগে সেকুরিটি চেক করে। 4. Smart Proxy: এই প্রক্সি মুল অবজেক্টের রেফারেন্স নাম্বার ট্রাক করে এবং প্রয়োজন মত মেমোরি থেকে লোডিং অথবা ফ্রি করতে সহয়তা করে।

এখানে আমরা `Virtual Proxy` এর একটি উদাহরণ দেখব।

```php
interface FileInterface
{
    public function content();
}

class RealFile implements FileInterface
{
    private $fileName;

    private $fileContent;

    public function __construct($fileName)
    {
        $this->fileName = $fileName;

        $this->readFile();
    }

    private function readFile()
    {
        $this->fileContent = file_get_contents($this->fileName);
    }

    public function content()
    {
        return $this->fileContent;
    }
}

class ProxyFile implements FileInterface
{
    private $fileName;

    private $realFileObject;

    public function __construct($fileName)
    {
        $this->fileName = $fileName;
    }

    public function content()
    {
        // Lazy load the file using the RealFile class
        if (!$this->realFileObject) {
            $this->realFileObject = new RealFile($this->fileName);
        }

        return $this->realFileObject->content();
    }
}
```

উপরের কোডটি খেয়াল করলে আমরা দেখতে পাব একই ইন্টারফেইস `FileInterface` ব্যাবহার করে রিয়েল অবজেক্ট এর জন্য `RealFile` ও প্রক্সি অবজেক্টের জন্য `ProxyFile` নামক ক্লাস ইমপ্লিমেন্ট করা হয়েছে।

`ProxyFile` এর `content()` মেথডটি দেখলে বুঝতে পাব যে এর মাধ্যমে মুল `RealFile` ক্লাস এর ইন্সটানশিয়েট করা হয়েছে লেজিলোডিং পদ্ধতির মাধ্যমে যাতে অ্যাকসেস না করা পর্যন্ত ইন্সটানশিয়েট না করা হয়।

```php
public function content()
{
    // Lazy load the file using the RealFile class
    if (!$this->realFileObject) {
        $this->realFileObject = new RealFile($this->fileName);
    }

    return $this->realFileObject->content();
}
```

এবার নিচের মত করে উভয় ক্লাসকে ইন্সটানশিয়েট করে কল করা হলে প্রথমে ভিন্ন ভিন্ন মেমোরি দখল করবে।

```php
$realFile = new RealFile('/path/to/file.jpg');
var_dump(memory_get_usage()); // ~5Mb
$realFile->content();
var_dump(memory_get_usage()); // ~5Mb

$realFile->content();
var_dump(memory_get_usage()); // ~5Mb

$proxyFile = new ProxyFile('/path/to/file.jpg');
var_dump(memory_get_usage()); // ~350Kb
$proxyFile->content();
var_dump(memory_get_usage()); // ~5Mb

$proxyFile->content();
var_dump(memory_get_usage()); // ~5Mb
```

এই চ্যাপ্টারের সোর্স কোডটি [এই লিঙ্ক](https://github.com/sohelamin/php-design-patterns) থেকে পাবেন।

