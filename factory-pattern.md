# ফ্যাক্টরী ডিজাইন প্যাটার্নঃ

ফ্যাক্টরী প্যাটার্ন এমন একটি প্যাটার্ন যা কম বেশি সব ধরনের অ্যাপ্লিকেশনে ব্যাবহৃত হয়ে থাকে এইটা ক্রিয়েশনাল প্যাটার্ন ক্যাটাগরীর মধ্যে পরে।

ফ্যাক্টরী প্যাটার্নের মূল উদ্দেশ্যই হল এর প্রোডাক্ট কিংবা চাইল্ড ক্লাসের অবজেক্ট তৈরি করে দেয়া। যেমন বাস্তব জীবনে যেভাবে ফ্যাক্টরীতে প্রোডাক্ট তৈরি হয়ে থাকে।

এই প্যাটার্ন ক্লাইন্টের কাছে অবজেক্ট ইনস্টানশিয়েট করার লজিক অদৃশ্যমান রাখে। আর অবজেক্টের ক্লাস গুলা একটা কমন ইন্টারফেইস কে ফলো করে বানানো থাকে।

এই প্যাটার্ন সাধারণত ৩ প্রকারেরঃ
১. সিম্পল ফ্যাক্টরী।
২. ফ্যাক্টরী মেথড।
৩. অ্যাবস্ট্রাক্ট ফ্যাক্টরী।

## ১. সিম্পল ফ্যাক্টরীঃ
ফ্যাক্টরী প্যাটার্নের মধ্যে সিম্পল ফ্যাক্টরী হচ্ছে সবচেয়ে সহজ প্যাটার্ন যদিও অফিশিয়ালি এই প্যাটার্ন ডিজাইন প্যাটার্ন হিসেবে স্বীকৃত না।

এই প্যাটার্নের নিয়ম মতে এর একটি ফ্যাক্টরী থাকবে আর একটি ফ্যাক্টরী একই সময় শুধুমাত্র একটাই প্রোডাক্ট তৈরি করবে অর্থাৎ একটিই ইনস্টান্স কিংবা অবজেক্ট রিটার্ন করবে।

নিচে একটা উধাহরন দেয়া হলঃ

```php
class CarFactory
{
    protected $brands = [];

    public function __construct()
    {
        $this->brands = [
            'mercedes' => 'MercedesCar',
            'toyota' => 'ToyotaCar',
        ];
    }

    public function make($brand)
    {
        if (!array_key_exists($brand, $this->brands)) {
            return new Exception('Not available this car');
        }

        $className = $this->brands[$brand];

        return new $className();
    }
}

interface CarInterface
{
    public function design();
    public function assemble();
    public function paint();
}

class MercedesCar implements CarInterface
{
    public function design()
    {
        return 'Designing Mercedes Car';
    }

    public function assemble()
    {
        return 'Assembling Mercedes Car';
    }

    public function paint()
    {
        return 'Painting Mercedes Car';
    }
}

class ToyotaCar implements CarInterface
{
    public function design()
    {
        return 'Designing Toyota Car';
    }

    public function assemble()
    {
        return 'Assembling Toyota Car';
    }

    public function paint()
    {
        return 'Painting Toyota Car';
    }
}

$carFactory = new CarFactory;

$mercedes = $carFactory->make('mercedes');
echo $mercedes->design() . '<br/>';
echo $mercedes->assemble() . '<br/>';
echo $mercedes->paint() . '<br/>';

echo '<br/>';

$toyota = $carFactory->make('toyota');
echo $toyota->design() . '<br/>';
echo $toyota->assemble() . '<br/>';
echo $toyota->paint() . '<br/>';
```

এখানে ```CarFactory``` নামে মূল ফ্যাক্টরী ক্লাস ডিফাইন করা হয়েছে যেটির মাধ্যমে একটা ```Car``` ইনস্টান্স তৈরি করা হবে।
```Car``` এর জন্য ২ টি ক্লাস যথাক্রমে ```MercedesCar``` ও ```ToyotaCar``` ডিফাইন করা হয়েছে যেগুলো ```CarInterface``` কে ফলো করেছে।

এবার চলুন ```CarFactory``` ক্লাসটিকে ইনস্টানশিয়েট করি।

```php
$carFactory = new CarFactory;
```

এরপর ধরুন ```MercedesCar``` ক্লাসকে ফ্যাক্টরির মাধ্যমে ইনস্টানশিয়েট করব তাহলে টাইপ/প্যারামিটার হিসেবে ```mercedes``` দিতে হবে নিচের মত করে।

```php
$mercedes = $carFactory->make('mercedes');
echo $mercedes->design() . '<br/>';
echo $mercedes->assemble() . '<br/>';
echo $mercedes->paint() . '<br/>';
```

অনুরূপ ভাবে ```ToyotaCar``` ক্লাসকে ইনস্টানশিয়েট করতে হলে

```php
$toyota = $carFactory->make('toyota');
echo $toyota->design() . '<br/>';
echo $toyota->assemble() . '<br/>';
echo $toyota->paint() . ‘<br/>';
```

আর ডিফাইন না করা কোন ক্লাসের টাইপ দিলে সেটি এরর দেখাবে।

## ২. ফ্যাক্টরী মেথডঃ

ফ্যাক্টরী মেথড প্যাটার্ন অনেকখানি সিম্পল ফ্যাক্টরী প্যাটার্নের মতই শুধুমাত্র এর মূল পার্থক্য হল এটি তার সাব ক্লাস গুলোকে ক্লাস ইনস্টানশিয়েট করার স্বাধীনতা দিয়ে দেয়। আর এর একাধিক ফ্যাক্টরী থাকতে পারে।

নিচে একটা উদাহরণ দেয়া হলঃ

```php
abstract class VehicleFactoryMethod
{
    abstract public function make($brand);
}

class CarFactory extends VehicleFactoryMethod
{
    public function make($brand)
    {
        $car = null;

        switch ($brand) {
            case "mercedes":
                $car = new MercedesCar;
                break;
            case "toyota":
                $car = new ToyotaCar;
                break;
        }

        return $car;
    }
}

class BikeFactory extends VehicleFactoryMethod
{
    public function make($brand)
    {
        $bike = null;

        switch ($brand) {
            case "yamaha":
                $bike = new YamahaBike;
                break;
            case "ducati":
                $bike = new DucatiBike;
                break;
        }

        return $bike;
    }
}

interface CarInterface
{
    public function design();

    public function assemble();

    public function paint();
}

interface BikeInterface
{
    public function design();

    public function assemble();

    public function paint();
}

class MercedesCar implements CarInterface
{
    public function design()
    {
        return 'Designing Mercedes Car';
    }

    public function assemble()
    {
        return 'Assembling Mercedes Car';
    }

    public function paint()
    {
        return 'Painting Mercedes Car';
    }
}

class ToyotaCar implements CarInterface
{
    public function design()
    {
        return 'Designing Toyota Car';
    }

    public function assemble()
    {
        return 'Assembling Toyota Car';
    }

    public function paint()
    {
        return 'Painting Toyota Car';
    }
}

class YamahaBike implements BikeInterface
{
    public function design()
    {
        return 'Designing Yamaha Bike';
    }

    public function assemble()
    {
        return 'Assembling Yamaha Bike';
    }

    public function paint()
    {
        return 'Painting Yamaha Bike';
    }
}

class DucatiBike implements BikeInterface
{
    public function design()
    {
        return 'Designing Ducati Bike';
    }

    public function assemble()
    {
        return 'Assembling Ducati Bike';
    }

    public function paint()
    {
        return 'Painting Ducati Bike';
    }
}

$carFactoryInstance = new CarFactory;

$mercedes = $carFactoryInstance->make('mercedes');
echo $mercedes->design() . '<br/>';
echo $mercedes->assemble() . '<br/>';
echo $mercedes->paint() . '<br/>';

echo '<br/>';

$toyota = $carFactoryInstance->make('toyota');
echo $toyota->design() . '<br/>';
echo $toyota->assemble() . '<br/>';
echo $toyota->paint() . '<br/>';

echo '<br/>';

$bikeFactoryInstance = new BikeFactory;

$yamaha = $bikeFactoryInstance->make('yamaha');
echo $yamaha->design() . '<br/>';
echo $yamaha->assemble() . '<br/>';
echo $yamaha->paint() . '<br/>';

echo '<br/>';

$ducati = $bikeFactoryInstance->make('ducati');
echo $ducati->design() . '<br/>';
echo $ducati->assemble() . '<br/>';
echo $ducati->paint() . '<br/>';
```

এখানে ফ্যাক্টরী মেথডের জন্য ```VehicleFactoryMethod``` নামে একটা অ্যাবস্ট্রাক্ট ক্লাস ডিফাইন করা হয়েছে যেটির সাব ক্লাস যথাক্রমে ```CarFactory``` ও ```BikeFactory``` আছে যেগুলা ভিন্ন ভিন্ন একক ফ্যাক্টরী।
আবার প্রতিটি ফ্যাক্টরীর জন্য সিম্পল ফ্যাক্টরী প্যাটার্নের ন্যায় ইন্টারফেইস ```CarInterface``` ও ```BikeInterface``` ডিফাইন করা হয়েছে যেগুলোকে ইমপ্লিমেন্ট করে কংক্রিট ক্লাস অর্থাৎ ইনস্টানশিয়েট যোগ্য ক্লাস যথাক্রমে ```CarFactory``` এর আওতায় ```MercedesCar``` ও ```ToyotaCar``` এবং ```BikeFactory``` এর আওতায় ```YamahaBike``` ও ```DucatiBike``` ডিফাইন করা হয়েছে।

সুতরাং ```CarFactory``` ও ```BikeFactory``` ক্লাসগুলো নির্ধারণ করতে পারবে সে কোন ক্লাসকে ইনস্টানশিয়েট করবে।

নিচের কোডটি খেয়াল করলে বুঝতে পারবেন ২ টি আলাদা ফ্যাক্টরীর মাধ্যমে প্যারামিটার কিংবা ```Car``` এর ব্র্যান্ড পাস করে কাঙ্ক্ষিত অবজেক্ট কে পাওয়া যায়।

```php
$carFactoryInstance = new CarFactory;

$mercedes = $carFactoryInstance->make('mercedes');
echo $mercedes->design() . '<br/>';
echo $mercedes->assemble() . '<br/>';
echo $mercedes->paint() . '<br/>';

echo '<br/>';

$toyota = $carFactoryInstance->make('toyota');
echo $toyota->design() . '<br/>';
echo $toyota->assemble() . '<br/>';
echo $toyota->paint() . '<br/>';

echo '<br/>';

$bikeFactoryInstance = new BikeFactory;

$yamaha = $bikeFactoryInstance->make('yamaha');
echo $yamaha->design() . '<br/>';
echo $yamaha->assemble() . '<br/>';
echo $yamaha->paint() . '<br/>';

echo '<br/>';

$ducati = $bikeFactoryInstance->make('ducati');
echo $ducati->design() . '<br/>';
echo $ducati->assemble() . '<br/>';
echo $ducati->paint() . ‘<br/>';
```

## ৩. অ্যাবস্ট্রাক্ট ফ্যাক্টরীঃ

অ্যাবস্ট্রাক্ট ফ্যাক্টরী এমন একটি পদ্ধতি প্রদান করে যেখানে একটি মূল (অ্যাবস্ট্রাক্ট) ফ্যাক্টরী অনেকগুলো একক ফ্যাক্টরীকে একত্রিত করে রাখে।

এক কথায়, প্রথমে একটি অ্যাবস্ট্রাক্ট ফ্যাক্টরী অনেকগুলো প্রোডাক্ট ফ্যাক্টরী তৈরি করে এরপর প্রতিটি ফ্যাক্টরী একাধিক প্রোডাক্ট কিংবা অবজেক্ট তৈরি করে।

উল্লেখ্য, প্রতিটি প্রোডাক্ট ফ্যাক্টরী ক্লাসকে একটা কমন অ্যাবস্ট্রাক্ট ক্লাসকে এক্সটেন্ড করতে হবে অথবা একটা কমন ইন্টারফেইসকে ইমপ্লিমেন্ট করতে হবে।
আবার প্রতিটি প্রোডাক্ট ফ্যাক্টরী ক্লাসে একাধিক আর একই মেথড থাকতে হবে।

নিচে একটা উধাহরণ দেয়া হলঃ

```php
abstract class AbstractVehicleFactory
{
    abstract public function makeCar();
    abstract public function makeBike();
}

class BangladeshiFactory extends AbstractVehicleFactory
{
    public function makeCar()
    {
        return new ToyotaCar();
    }

    public function makeBike()
    {
        return new YamahaBike();
    }
}

class USAFactory extends AbstractVehicleFactory
{
    public function makeCar()
    {
        return new MercedesCar();
    }

    public function makeBike()
    {
        return new DucatiBike();
    }
}

abstract class AbstractVehicle
{
    abstract public function design();
    abstract public function assemble();
    abstract public function paint();
}

abstract class AbstractCarVehicle extends AbstractVehicle
{

}

abstract class AbstractBikeVehicle extends AbstractVehicle
{

}

class MercedesCar extends AbstractCarVehicle
{
    public function design()
    {
        return 'Designing Mercedes Car';
    }

    public function assemble()
    {
        return 'Assembling Mercedes Car';
    }

    public function paint()
    {
        return 'Painting Mercedes Car';
    }
}

class ToyotaCar extends AbstractCarVehicle
{
    public function design()
    {
        return 'Designing Toyota Car';
    }

    public function assemble()
    {
        return 'Assembling Toyota Car';
    }

    public function paint()
    {
        return 'Painting Toyota Car';
    }
}

class YamahaBike extends AbstractBikeVehicle
{
    public function design()
    {
        return 'Designing Yamaha Bike';
    }

    public function assemble()
    {
        return 'Assembling Yamaha Bike';
    }

    public function paint()
    {
        return 'Painting Yamaha Bike';
    }
}

class DucatiBike extends AbstractBikeVehicle
{
    public function design()
    {
        return 'Designing Ducati Bike';
    }

    public function assemble()
    {
        return 'Assembling Ducati Bike';
    }

    public function paint()
    {
        return 'Painting Ducati Bike';
    }
}

$bangladeshiFactoryInstance = new BangladeshiFactory;
$car = $bangladeshiFactoryInstance->makeCar();
echo $car->design() . '<br/>';
echo $car->assemble() . '<br/>';
echo $car->paint() . '<br/>';

echo '<br/>';

$bike = $bangladeshiFactoryInstance->makeBike();
echo $bike->design() . '<br/>';
echo $bike->assemble() . '<br/>';
echo $bike->paint() . '<br/>';

echo '<br/>';

$usaFactoryInstance = new USAFactory;
$car = $usaFactoryInstance->makeCar();
echo $car->design() . '<br/>';
echo $car->assemble() . '<br/>';
echo $car->paint() . '<br/>';

echo '<br/>';

$bike = $usaFactoryInstance->makeBike();
echo $bike->design() . '<br/>';
echo $bike->assemble() . '<br/>';
echo $bike->paint() . ‘<br/>';
```

উপরের কোডে ```AbstractVehicleFactory``` নামে একটা অ্যাবস্ট্রাক্ট ফ্যাক্টরী ক্লাস ডিফাইন করা হয়েছে যেখানে ```makeCar()``` ও ```makeBike()``` ২টা মেথড দেয়া আছে যাতে সাব ফ্যাক্টরী গুলো ওই মেথড গুলো ডিফাইন করে।

এখানে একটি মেথড ```Car``` অবজেক্ট তৈরি করতে আরেকটি ```Bike``` অবজেক্ট  তৈরি করতে ব্যবহৃত হয়েছে যা সব গুলা ফ্যাক্টরীকেই করতে হবে।
আর মূল বিষয় হল একেক ফ্যাক্টরী একেক ব্র্যান্ডের ```Car``` ও ```Bike``` অবজেক্ট তৈরি করবে।

যেমন এখানে আমরা ```BangladeshiFactory``` ফ্যাক্টরী ব্যাবহার করেছি যেটি ```ToyotaCar``` ও ```YamahaBike``` ক্লাসের অবজেক্ট তৈরি করবে। অনুরূপ ভাবে, ```USAFactory``` ফ্যাক্টরী ```MercedesCar``` ও ```DucatiBike``` ক্লাসের অবজেক্ট তৈরি করবে।

আবার আপনি চাইলে একটা ফ্যাক্টরীতে একাধিক ব্রান্ডের ```Car``` কিংবা ```Bike``` এর অবজেক্ট তৈরি করতে পারেন সেক্ষেত্রে র‍্যান্ডমলি কিংবা লজিক্যালি করতে হবে।

এই [লিঙ্ক](https://github.com/sohelamin/php-design-patterns) থেকে সোর্স কোডটি পাবেন।