# অবজার্ভার ডিজাইন প্যাটার্নঃ

অবজার্ভার ডিজাইন প্যাটার্ন বিহাভিওরাল টাইপের মধ্যে পরে।
এটা **pub/sub** এর নিয়মে কাজ করে অর্থাৎ কোন অবজেক্ট কিংবা সাবজেক্ট এ পরিবর্তন হলে সেটা **Publisher** তৎক্ষন্যাত **Subscriber** দেরকে জানায় দিবে কিংবা নটিফাই করবে।

পিএইসপিতে অবজার্ভার প্যাটার্নটি প্রয়োগ করতে হলে যথাক্রমে ```SplSubject``` ও ```SplObserver``` ইন্টারফেইস ইমপ্লিমেন্ট করে সাবজেক্ট ও অবজার্ভার ২ টা ক্লাস লিখতে হয়। আর সাবস্ক্রাইব করা অবজার্ভারদেরকে স্টোর করে রাখার জন্য ```SplObjectStorage``` এই ক্লাসটিকে ব্যাবহার করা যেতে পারে।

উপরে উল্লেখিত **SplSubject, SplObserver, SplObjectStorage** হল পিএইসপির **Standard PHP Library (SPL)**

নিচে একটি ```Model``` নামক ক্লাস ও দুইটি অবজার্ভার ক্লাসের উধাহরন দেয়া হলঃ

```php

<?php

class Model implements SplSubject
{
    protected $observers;

    public function __construct()
    {
        $this->observers = new SplObjectStorage();
    }

    public function attach(SplObserver $observer)
    {
        $this->observers->attach($observer);
    }

    public function detach(SplObserver $observer)
    {
        $this->observers->detach($observer);
    }

    public function notify()
    {
        foreach ($this->observers as $observer) {
            $observer->update($this);
        }
    }

    public function __set($name, $value)
    {
        $this->data[$name] = $value;
        // notify the observers, that model has been updated
        $this->notify();
    }
}

class ModelObserver implements SplObserver
{
    public function update(SplSubject $subject)
    {
        echo get_class($subject) . ' has been updated' . '<br>';
    }
}

class Observer2 implements SplObserver
{
    public function update(SplSubject $subject)
    {
        echo get_class($subject) . ' has been updated' . '<br>';
    }
}

// Instantiate the model class for 2 different objects
$model1 = new Model();
$model2 = new Model();

// Instantiate the observers
$modelObserver = new ModelObserver();
$observer2 = new Observer2();

// Attach the observers to $model1
$model1->attach($modelObserver);
$model1->attach($observer2);

// Attach the observers to $model2
$model2->attach($observer2);

// Changing the subject properties
$model1->title = 'Hello World';
$model2->body = 'Lorem ipsum............';

```

উপরে ```Model``` ক্লাসটি হল সাবজেক্ট ```ModelObserver``` ও ```Observer2``` হল অবজার্ভার।

```Model``` ক্লাসটি যেহেতু ```SplSubject``` ইন্টারফেইস ইমপ্লিমেন্ট করে লেখা হয়েছে কাজেই ```attach()```, ```detach()``` ও ```notify()``` মেথডগুলা অবশ্যই থাকতে হবে।

অপরদিকে যেহেতু ```ModelObserver``` ও ```Observer2``` ক্লাসগুলা ```SplObserver``` ইন্টারফেইস ইমপ্লিমেন্ট করে লেখা হয়েছে সেহেতু ```update()``` মেথডটি ক্লাসগুলাতে থাকতে হবে।

এবার আপনারা যদি ```SplSubject``` ও ```SplObserver``` ইন্টারফেইস ব্যাবহার না করে অবজার্ভার ডিজাইন প্যাটার্ন এর প্রয়োগ করতে চান সেটাও করতে পারবেন শুধুমাত্র আপনার বিষয় বস্তু ঠিক থাকলেই হল।

নিচে একটা উধাহরন দেয়া হলঃ

```php

<?php

class Model
{
    protected $observers;

    public function __construct()
    {
        $this->observers = new SplObjectStorage();
    }

    public function notify()
    {
        foreach ($this->observers as $observer) {
            $observer->update($this);
        }
    }

    public function setObservers($observers = [])
    {
        foreach ($observers as $observer) {
            $this->observers->attach($observer);
        }
    }

    public function __set($name, $value)
    {
        $this->data[$name] = $value;
        // notify the observers, that model has been updated
        $this->notify();
    }
}

class Post extends Model
{
    public function insert($data)
    {
        // Store the data
        // Notify to observers
        $this->notify();
    }

    public function update($data)
    {
        // Update the model
        // Notify to observers
        $this->notify();
    }

    public function delete($id)
    {
        // Delete the model
        // Notify to observers
        $this->notify();
    }
}

class PostModelObserver
{
    public function update($subject)
    {
        echo get_class($subject) . ' has been updated' . '<br>';
    }
}

class Observer2
{
    public function update($subject)
    {
        echo get_class($subject) . ' has been updated' . '<br>';
    }
}

$post = new Post();

$post->setObservers([new PostModelObserver, new Observer2]);

$post->title = 'Hello World';

```

[এই লিঙ্ক](https://github.com/sohelamin/php-design-patterns/blob/master/Observer.php) থেকে আরও ধারনা পেতে পারেন।