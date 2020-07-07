# স্ট্রাটেজি

`Strategy` ডিজাইন প্যাটার্ন বিহেভিওরাল ডিজাইন প্যাটার্নের মধ্যে পরে। `Strategy` এর অর্থ হল কৌশল, কোন কিছু করতে গেলে তার জন্য কৌশল কিংবা এক গুচ্ছ পদক্ষেপ গ্রহণ করাই হল স্ট্রাটেজি।

প্রোগ্রামিং এর পরিভাষায়, একটি নির্দিষ্ট কাজ সম্পন্ন করতে ভিন্ন ভিন্ন অ্যালগরিদম নির্ধারণ করার স্বাধীনতা থাকাই স্ট্রাটেজি প্যাটার্ন। এই প্যটার্নকে আবার পলিসি প্যাটার্নও বলা হয়ে থাকে।

ধরুন, আপনি ঢাকা থেকে চট্টগ্রাম যাইতে চাচ্ছেন এরজন্য আপনি চাইলে বাস, ট্রেন কিংবা প্লেন এ করে যাইতে পারেন। এইক্ষেত্রে গন্তব্যস্থল একটিই কিন্তু এটা সম্পন্ন করতে ভিন্ন ভিন্ন স্ট্রাটেজি অনুসরণ করা যায়।

এবার চলুন একটা বাস্তব ভিত্তিক উদাহরণের মাধ্যমে প্যাটার্নটি বুঝা যাক। প্রথমে চলুন একটা ইন্টারফেইস বানিয়ে ভিন্ন ভিন্ন স্ট্রাটেজি ইমপ্লিমেন্ট করি নিচের মত করে।

```php
interface TravelStrategy
{
    public function travel();
}

class BusTravelStrategy implements TravelStrategy
{
    public function travel()
    {
        // Bus travel strategy will goes here
    }
}

class TrainTravelStrategy implements TravelStrategy
{
    public function travel()
    {
        // Train travel strategy will goes here
    }
}

class PlaneTravelStrategy implements TravelStrategy
{
    public function travel()
    {
        // Plane travel strategy will goes here
    }
}
```

এবার মেইন কনটেক্সট ক্লাস হিসেবে `Traveler` নামক একটা ক্লাস ডিফাইন করি।

```php
class Traveler
{
    protected $traveler;

    public function __construct(TravelStrategy $traveler)
    {
        $this->traveler = $traveler;
    }

    public function travel()
    {
        $this->traveler->travel();
    }
}
```

পরিশেষে, স্ট্রাটেজি পরিবর্তন করে সহজে আমরা আমাদের কার্য সম্পন্ন করতে পারি।

```php
$traveler = new Traveler(new BusTravelStrategy());
$traveler->travel();

$traveler1 = new Traveler(new PlaneTravelStrategy());
$traveler1->travel();
```

এই চ্যাপ্টারের সোর্স কোডটি [এই লিঙ্ক](https://github.com/sohelamin/php-design-patterns) থেকে পাবেন।

