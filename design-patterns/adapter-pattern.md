# অ্যাডাপ্টার

সফটওয়ার ইঞ্জিনিয়ারিং এ আরেকটি বহুল প্রচলিত ডিজাইন প্যাটার্ন হল অ্যাডাপ্টার ডিজাইন প্যাটার্ন। এটি **স্ট্রাকচারাল** প্যাটার্নের মধ্যে পরে।

আমরা বাস্তব জীবনে সবাই অ্যাডাপ্টার শব্দটির সাথে পরিচিত। যেমনঃ মোবাইলের চার্জিং অ্যাডাপ্টার, কম্পিউটারের গ্রাফিক্স অ্যাডাপ্টার।

আর অ্যাডাপ্টার ডিজাইন প্যাটার্ন অনেকটা এই অ্যাডাপ্টারের ন্যায় কাজ করে অর্থাৎ আমরা যদি কম্পিউটারের গ্রাফিক্স কিংবা ভিজিএ অ্যাডাপ্টারের কথা চিন্তা করি তাহলে বলা যায় আমরা গেইম খেলার জন্য এক বিশেষ ধরনের অ্যাডাপ্টার ব্যবহার করি আবার সাধারণ কোন কাজের জন্য সাধারণ অ্যাডাপ্টার হলেই চলে কিন্তু বিষয়বস্তু দুইটারি সমান ভিডিও আউটপুট করা দুইটিই একটা কমন প্যাটার্নে তৈরি। আর মজার বিষয় হল এই অ্যাডাপ্টার গুলো আমাদের খুশি মত আমরা পরিবর্তন করতে পারি।

এবার ইমপ্লিমেন্টেশনের পরিভাষায়, ধরুন আমরা একটা পিএইচপি প্রজেক্ট কিংবা অ্যাপ্লিকেশন বানাবো যেখানে আমরা ডাটাবেস অ্যাডাপ্টার হিসেবে **MySQL Adapter** আর **PDO Adapter** ব্যাবহার করব যাতে করে ক্লাইন্ট সহজেই তার পছন্দের অ্যাডাপ্টারটি ব্যাবহার করতে পারে **Database** নামে আরেকটি অ্যাডাপ্টারের মাধ্যমে। এতে করে **MySQL Adapter** আর **PDO Adapter** গুলো খুব সহজেই পরিবর্তন করা যাবে।

নিচে একটা সম্পূর্ণ উদাহরণ দেয়া হলঃ

```php
<?php

interface AdapterInterface
{
    public function query($sql);

    public function result();
}

class MySQLAdapter implements AdapterInterface
{
    protected $connection;

    protected $result;

    public function __construct($host, $username, $password, $dbname)
    {
        $this->connection = new mysqli($host, $username, $password, $dbname);
    }

    public function query($sql)
    {
        $this->result = $this->connection->query($sql);

        return $this;
    }

    public function result()
    {
        if (gettype($this->result) === 'boolean') {
            return $this->result;
        } elseif ($this->result->num_rows > 0) {
            $result = [];

            while ($row = $this->result->fetch_assoc()) {
                $result[] = $row;
            }

            return $result;
        } else {
            return [];
        }
    }
}

class PDOAdapter implements AdapterInterface
{
    protected $connection;

    protected $result;

    public function __construct($host, $username, $password, $dbname)
    {
        $this->connection = new PDO("mysql:host=$host;dbname=$dbname", $username, $password);
    }

    public function query($sql)
    {
        $query = $this->connection->prepare($sql);
        $exec = $query->execute();

        if ($query->columnCount() == 0) {
            $this->result = $exec;
        } else {
            $this->result = $query;
        }

        return $this;
    }

    public function result()
    {
        if (gettype($this->result) === 'boolean') {
            return $this->result;
        } else {
            $data = [];

            while ($row = $this->result->fetch(PDO::FETCH_ASSOC)) {
                $data[] = $row;
            }

            return $data;
        }
    }
}

class Database
{
    protected $adapter;

    public function __construct(AdapterInterface $adapter)
    {
        $this->adapter = $adapter;
    }

    public function query($sql)
    {
        return $this->adapter->query($sql);
    }

    public function result()
    {
        return $this->adapter->result();
    }
}

$mysql = new MySQLAdapter('localhost', 'root', '1234', 'demo');
$db = new Database($mysql);

$query = $db->query("SELECT * FROM users");
$result = $query->result();
var_dump($result);
```

এখানে `AdapterInterface` ইন্টারফেইস ব্যবহার করা হয়েছে যেটিকে ইমপ্লিমেন্ট করে যথাক্রমে `MySQLAdapter` ও `PDOAdapter` ডিক্লেয়ার করা হয়েছে যাতে দুইটারি ন্যাচার কিংবা কোডবেইস একই থাকে।

আবার ডাটাবেসকে অ্যাকসেস করার জন্য ও অ্যাডাপ্টারগুলাকে ব্যবহার করার জন্য `Database` নামে একটা ক্লাস ডিফাইন করা হয়েছে। আর এর ডিপেন্ডেন্সি ইনজেকশন হিসেবে `AdapterInterface` ব্যবহার করা হয়েছে যাতে করে কেবল মাত্র `AdapterInterface` ইমপ্লিমেন্ট করা ক্লাসের ইন্সটান্সই কন্সটারক্টরে পাস করা যায়।

এখানে আমরা `MySQLAdapter` কে ব্যবহার করেছি।

```php
$mysql = new MySQLAdapter('localhost', 'root', '1234', 'demo');
$db = new Database($mysql);
```

আমরা চাইলে PDOAdapter ও ব্যবহার করতে পারি নিচের মত করে।

```php
$pdo = new PDOAdapter('localhost', 'root', '1234', 'demo');
$db = new Database($pdo);
```

এতে করে অ্যাডাপ্টার গুলা **Loosly Coupled/Highly Decoupled** থাকে আর বর্তমানে এই টার্মটাকে খুবই প্রাধান্য দেয়া হয় বড় কোন অ্যাপ্লিকেশন কিংবা ফ্রেমওয়ার্ক তৈরি করতে গেলে।

নিচের [লিঙ্ক](https://github.com/sohelamin/php-design-patterns) থেকে সোর্স কোডটি পাবেন।

