
# ম্যাজিক মেথড 

পিএইচপির ক্লাসে কিছু মেথড থাকে যেগুলো দুইটি আন্ডারস্কোর দিয়ে শুরু হয়, এই মেথড গুলোকে সাধারণত ম্যাজিক মেথড বলা হয়। যদিও এই মেথডগুলো আগেথেকে ক্লাসে থাকে না, এই মেথড গুলো সাধারনত প্রোগ্রামাররাই লিখে থাকে। কিন্তু এই মেথড গুলো অন্যান্য মেথডের মত আচরন করে না। 

`__construct()`,` __destruct()`, `__call()`, `__callStatic()`, `__get()`, `__set()`, `__isset()`, `__unset()`, `__sleep()`,` __wakeup()`,` __toString()`, `__invoke()`, `__set_state()`, `__clone()` এবং  `__debugInfo()` এই মেথড গুলোকে ম্যাজিক মেথড বলা হয়ে থাকে।

 __construct(), __destruct() এই মেথড গুলো সম্পর্কে [পূর্বে](http://php.howtocode.com.bd/oop-constructors-and-destructors.html) আলোচনা করা হয়েছে।
 
 
##গেট মেথড
এই মেথড এর মাধ্যমে ক্লাসের প্রোপার্টি এক্সেস করা যায়। নিচের উদাহরণটিতে গেট মেথড ইমপ্লিমেন্ট করে দেখানো হয়েছে। 

```php
public function __get($property)
{
  if (property_exists($this, $property)) {
    return $this->$property;
  }
}
```

ধরা যাক আমাদের ক্লাসের নাম Tweet যার একটি প্রোপার্টি (username) আছে । কেউ যদি username প্রোপার্টি টি এক্সেস করতে চায় তবে সে নিচের উদাহরন এর মত করে প্রোপার্টিটি এক্সেস করতে পারবে।

```php
$tweet = new Tweet();
echo $tweet->username; // এটি username প্রোপার্টিটি রিটার্ন করবে, এমনকি প্রাইভেট প্রোপার্টি হলেও।
```
 
## সেট মেথড
এই মেথড এর মাধ্যমে ক্লাসের প্রোপার্টির ভ্যালু সেট করা যায়। নিচের উদাহরণটিতে সেট মেথড ইমপ্লিমেন্ট করে দেখানো হয়েছে। 

```php
public function __set($property, $value)
{
  if (property_exists($this, $property)) {
    $this->$property = $value;
  }
}
```

Tweet ক্লাসে username এর ভ্যালু সেট করতে চাইলে নিচের উদাহরন অনুসরন করলেই হবে।

```php
$tweet = new Tweet();
$tweet->username = 'saaiful'; // এটি username প্রোপার্টর ভ্যালু সেট করবে।
```

##ইজসেট মেথড
কোন প্রোপার্টি ক্লাসের মধ্যে আছে কিনা তা জানতে এই মেথড ব্যবহার করা হয়। এই মেথড ক্লাসের বাইরে থেকে এক্সেস করা যায় না।

```php
public function  __isset($property)
{
  return isset($this->$property);
}
```

ক্লাসের যেকোন মেথড থেকে নিচের মত করে এই মেথডটি ব্যবহার করা যাবে

```php
isset($tweet->username);
```

##আনসেট মেথড
ক্লাসের কোন প্রোপার্টি সরিয়ে দিতে এই মেথড ব্যবহার করা হয়। এই মেথড ক্লাসের বাইরে থেকে এক্সেস করা যায় না।
```php
public function __unset($property)
{
  unset($this->$property);
}
```

ক্লাসের যেকোন মেথড থেকে নিচের মত করে এই মেথডটি ব্যবহার করা যাবে

```php
unset($tweet->username);
```

##কল মেথড 
যখন কোন মেথড ক্লাসের বাইরে থেকে এক্সেস করা যায় না অথবা যখন কল করা মেথডটি ক্লাসে থাকে না তখন এই মেথড কাজ শুরু করে। 
```php
public function __call($method, $parameters)
{
	var_dump($method);
	var_dump($parameters);
}
```
নিচের উদাহরনে post মেথড ব্যবহার করা হয়েছে , যদিও Tweet ক্লাসে এই মেথডটি নেই। কিন্তু আউটপুট লক্ষ করলে দেখা যাবে মেথডের নাম আর প্যারামিটারের var_dump করা হয়েছে। __call মেথডের মধ্যে প্রয়োজনীয় কোড লিখে এই মেথডের চমৎকার ব্যবহার করা যাবে।
```php
$tweet = new Tweet();
$tweet->post("this is a test");
```
উদাহরনঃ
```php
<?php
class Tweet
{
	
	function __construct()
	{
		$this->username = "saaiful";
		$this->api = "https://api.twitter.com/1.1/";
		$this->param['user_timeline'] = "statuses/user_timeline.json";
		$this->param['home_timeline'] = "statuses/home_timeline.json";
		$this->param['retweets'] = "statuses/retweets";
	}

	public function fetch($url)
	{
		// send get request to $url
		var_dump($url);
	}

	public function __call($method, $parameters='')
	{
		if(array_key_exists($method, $this->param)){
			$url = $this->api . $this->param[$method];
			if(!empty($parameters)){
				$url .= "/".$parameters[0].".json";
			}
			return $this->fetch($url);
		}else{
			return false;
		}
		
	}
}

$tweet = new Tweet();
$tweet->retweets('abc');
$tweet->ppp('abc');
```


...চলমান
