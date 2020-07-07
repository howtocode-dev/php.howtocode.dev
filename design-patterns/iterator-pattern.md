# ইটারেটর

ইটারেটর ডিজাইন প্যাটার্ন বিহেভিওরাল টাইপের মধ্যে পরে। এই প্যাটার্ন এর মুল উদ্দেশ্যই হচ্ছে ইটারেটরের ব্যাবহার করা। ইটারেটর একটা কন্টেইনার কিংবা অবজেক্ট এর ইলিমেন্টকে ট্রাভার্স করার জন্য সহায়তা করে আর এতে ভিতরের লজিক গুলো লুকানো অবস্থায় থাকে। যারফলে, আমরা কন্টেইনারে আমাদের পছন্দের মত ডাটা স্ট্রাকচার ব্যাবহার করতে পারি।

এবার চলুন আমরা কিভাবে এই প্যাটার্নটি ইমপ্লিমেন্ট করতে পারি। পিএইচপির একটা বিল্ড-ইন `Iterator` ইন্টারফেইস আছে আমরা সেটি ব্যাবহার করব।

সর্বপ্রথমে, আমরা ইলিমেন্ট বা আইটেম এর জন্য `Book` নামে একটা ক্লাস ডিফাইন করব।

```php
class Book
{
    private $title;

    public function __construct($title)
    {
        $this->title = $title;
    }

    public function getTitle()
    {
        return $this->title;
    }
}
```

এবার কন্টেইনার এর জন্য `BookList` নামে একটা ক্লাস ডিফাইন করব।

```php
class BookList implements Iterator, Countable
{
    private $books = [];

    private $currentIndex = 0;

    public function current()
    {
        return $this->books[$this->currentIndex];
    }

    public function key()
    {
        return $this->currentIndex;
    }

    public function next()
    {
        $this->currentIndex++;
    }

    public function rewind()
    {
        $this->currentIndex = 0;
    }

    public function valid()
    {
        return isset($this->books[$this->currentIndex]);
    }

    public function count()
    {
        return count($this->books);
    }

    public function addBook(Book $book)
    {
        $this->books[] = $book;
    }

    public function removeBook(Book $bookToRemove)
    {
        foreach ($this->books as $key => $book) {
            if ($book->getTitle() === $bookToRemove->getTitle()) {
                unset($this->books[$key]);
            }
        }

        $this->books = array_values($this->books);
    }
}
```

এখানে `Iterator` ইন্টারফেসের জন্য যথাক্রমে `current()`, `key()`, `next()`, `rewind()` ও `valid()` মেথডগুলি ইমপ্লিমেন্ট করা হয়েছে আর `Countable` ইন্টারফেইসের এর জন্য `count()` মেথডটি ইমপ্লিমেন্ট করা হয়েছে যা ইলেমেন্ট কাউন্ট করতে সাহায্য করবে। আর ইলিমেন্ট অ্যাড আর রিমুভ করার জন্য `addBook()` ও `removeBook()` কাস্টম মেথডগুলি ব্যাবহার করা হয়েছে।

এবার কন্টেইনার ক্লাসটি ইন্সটানশিয়েট করে কিছু ইলিমেন্ট অ্যাড করে আমরা নিচের ন্যায় লুপের মাধ্যমে ইলিমেন্ট ট্রাভার্স করে অ্যাকসেস করতে পারি।

```php
$bookList = new BookList();
$bookList->addBook(new Book('Design Pattern'));
$bookList->addBook(new Book('Head First Design Pattern'));
$bookList->addBook(new Book('Clean Code'));
$bookList->addBook(new Book('The Pragmatic Programmer'));

$bookList->removeBook(new Book('Design Pattern'));

foreach ($bookList as $book) {
    echo $book->getTitle() . PHP_EOL;
}
```

এই চ্যাপ্টারের সোর্স কোডটি [এই লিঙ্ক](https://github.com/sohelamin/php-design-patterns) থেকে পাবেন।

