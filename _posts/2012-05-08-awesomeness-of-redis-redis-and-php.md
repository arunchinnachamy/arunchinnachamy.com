---
layout: post
title: Awesomeness of Redis - Redis and PHP
excerpts: How to use Predis to connect redis and PHP explaining the connection and all the commands in Predis which lets you connect php with Redis.
comments: true
---
In my previous post ([Awesomeness of Redis – Redis Installation and Configuration](http://www.arunchinnachamy.com/awesomeness-of-redis-redis-installation-and-configuration/ "Redis Installation and Configuration")), I explained how to install and configure Redis server, a ultra fast Key-Value pair engine. As a follow up, this post (Awesomeness of Redis - redis and php) will focus on how to use PHP to connect to Redis server using Predis library.

### Redis and PHP Connection:

First, you need to download the Predis library from the [download page](https://github.com/nrk/predis/downloads "Predis Download Page"). Once downloaded, extract the contents and you can discard everything other than the Predis folder inside the lib directory. This is the core library files which you will be using from your PHP files to connect to Redis. Copy the directory Predis (which is inside the lib directory) in to the folder where your PHP script lies. Alternatively, you can copy the directory in
<pre>/usr/share/php/</pre>
for Ubuntu/Debian systems which is defined by default in php.ini as a place to place library files. By placing the directory in the above location, the Predis library will be accessible to all PHP scripts. Now that you have got the library files, It is time to load the library in your PHP code and connect to the Redis server.

The Predis comes with the auto-load function which will pretty much do everything and ease your work. Include the following line at the top of your PHP file.

{% highlight php %}
require 'Predis/Autoloader.php';
Predis\Autoloader::register();
{% endhighlight %}

These lines include and loads the file Autoloader.php inside the Predis directory which we copied in the last step. Now connect to your redis database from PHP using

{% highlight php %}
$redis = new Predis\Client(array(
    'scheme' => 'tcp',
    'host'   => '10.0.0.1',
    'port'   => 6379,
));
{% endhighlight %}

Change the Parameter depending on the IP Address and the port in which your installed Redis. If you have set a passkey to Redis Instance, you need to use below snippet. Scheme is by default tcp.

{% highlight php %}
$redis = new Predis\Client(array(
   'host' => '10.0.0.1',
   'port' =>  6379,
   'password' => {PASS_KEY}, 
   'database' => 0
));
{% endhighlight %}


I have introduced a new parameter, _database_ here. This lets you connect to the database you like, by default, it connects to 0. Now that you are connected, lets look into how to execute commands from PHP in your redis server. It is easy to add, modify, delete Keys from Redis server. I have compiled and listed out few basic and commonly used redis commands and its Predis equivalence below as I did not find the information to be readily available.

<table id="wp-table-reloaded-id-1-no-1" class="wp-table-reloaded wp-table-reloaded-id-1">
<caption style="caption-side:bottom;text-align:left;border:none;background:none;">
<a href="http://www.arunchinnachamy.com/wp-admin/tools.php?page=wp-table-reloaded&amp;action=edit&amp;table_id=1" title="Edit">Edit</a></caption>
<thead>
    <tr class="row-1 odd"><th class="column-1 sorting_disabled" rowspan="1" colspan="1" style="width: 133px;">Redis Command</th><th class="column-2 sorting_disabled" rowspan="1" colspan="1" style="width: 176px;">PHP Predis Command</th><th class="column-3 sorting_disabled" rowspan="1" colspan="1" style="width: 275px;">Comment</th></tr>
</thead>

<tbody class="row-hover"><tr class="row-2 even">
        <td class="column-1">SELECT index</td><td class="column-2">$redis-&gt;select(index)</td><td class="column-3"><em>Change the selected database for the current connection</em></td>
    </tr><tr class="row-3 odd">
        <td class="column-1">FLUSHDB</td><td class="column-2">$redis-&gt;flushdb();</td><td class="column-3"><em>Remove all keys from the current database</em></td>
    </tr><tr class="row-4 even">
        <td class="column-1">FLUSHALL</td><td class="column-2">$redis-&gt;flushall();</td><td class="column-3"><em>Remove all keys from all databases</em></td>
    </tr><tr class="row-5 odd">
        <td class="column-1">SAVE</td><td class="column-2">$redis-&gt;save();</td><td class="column-3"><em>Synchronously save the dataset to disk</em></td>
    </tr><tr class="row-6 even">
        <td class="column-1">QUIT</td><td class="column-2">$redis-&gt;quit();</td><td class="column-3"><em>Close the connection</em></td>
    </tr><tr class="row-7 odd">
        <td class="column-1">PING</td><td class="column-2">$redis-&gt;ping();</td><td class="column-3"><em>Ping the server</em></td>
    </tr><tr class="row-8 even">
        <td class="column-1">ECHO message</td><td class="column-2">$redis-&gt;do_echo('ECHO test');</td><td class="column-3"><em>Echo the given string</em></td>
    </tr><tr class="row-9 odd">
        <td class="column-1">SET key value<br>
</td><td class="column-2">$redis-&gt;set('aaa', 'bbb')</td><td class="column-3"><em>Set the string value of a key</em></td>
    </tr><tr class="row-10 even">
        <td class="column-1">SETNX key value<br>
</td><td class="column-2">$redis-&gt;set('aaa', 'ccc', true);</td><td class="column-3"><em>Set the value of a key, only if the key does not exist</em></td>
    </tr><tr class="row-11 odd">
        <td class="column-1">GET key</td><td class="column-2">$redis-&gt;get('aaa');</td><td class="column-3"><em>Get the value of a key</em></td>
    </tr><tr class="row-12 even">
        <td class="column-1">INCR key</td><td class="column-2">$redis-&gt;incr('aaa');</td><td class="column-3"><em>Increment the integer value of a key by one</em></td>
    </tr><tr class="row-13 odd">
        <td class="column-1">INCRBY key increment</td><td class="column-2">$redis-&gt;incr('aaa', 2);</td><td class="column-3"><em>Increment the integer value of a key by the given amount</em></td>
    </tr><tr class="row-14 even">
        <td class="column-1">DECR key</td><td class="column-2">$redis-&gt;decr('aaa');</td><td class="column-3"><em>Decrement the integer value of a key by one</em></td>
    </tr><tr class="row-15 odd">
        <td class="column-1">DECRBY key decrement</td><td class="column-2">$redis-&gt;decr('aaa', 2);</td><td class="column-3"><em>Decrement the integer value of a key by the given number</em></td>
    </tr><tr class="row-16 even">
        <td class="column-1">EXISTS key</td><td class="column-2">$redis-&gt;exists('aaa');</td><td class="column-3"><em>Determine if a key exists</em></td>
    </tr><tr class="row-17 odd">
        <td class="column-1">DEL key [key ...]</td><td class="column-2">$redis-&gt;delete('aaa');</td><td class="column-3"><em>Delete a key</em></td>
    </tr><tr class="row-18 even">
        <td class="column-1">KEYS pattern</td><td class="column-2">print_r($redis-&gt;keys('*'), true);</td><td class="column-3"><em>Find all keys matching the given pattern. In this case, all the keys</em></td>
    </tr><tr class="row-19 odd">
        <td class="column-1">RANDOMKEY</td><td class="column-2">$redis-&gt;randomkey('a*')</td><td class="column-3"><em>Return a random key from the keyspace matching the pattern.</em></td>
    </tr><tr class="row-20 even">
        <td class="column-1">RENAME key newkey</td><td class="column-2">$redis-&gt;rename('a1', 'a0');</td><td class="column-3"><em>Rename a key</em></td>
    </tr><tr class="row-21 odd">
        <td class="column-1">RENAMENX key newkey</td><td class="column-2">$redis-&gt;rename('a0', 'a2', true);</td><td class="column-3"><em>Rename a key, only if the new key does not exist</em></td>
    </tr><tr class="row-22 even">
        <td class="column-1">LPUSH key value [value ...]<br>
</td><td class="column-2">$redis-&gt;push('a0', 'aaa');</td><td class="column-3"><em>Prepend one or multiple values to a list</em></td>
    </tr><tr class="row-23 odd">
        <td class="column-1">RPUSH key value [value ...]</td><td class="column-2">$redis-&gt;push('a0', 'ccc', false);</td><td class="column-3"><em>Append one or multiple values to a list</em></td>
    </tr><tr class="row-24 even">
        <td class="column-1">LLEN key</td><td class="column-2">$redis-&gt;llen('a0');</td><td class="column-3"><em>Get the length of a list</em></td>
    </tr><tr class="row-25 odd">
        <td class="column-1">LRANGE key start stop</td><td class="column-2">$redis-&gt;lrange('sdkjhfskdjfh', 0, 100), true)</td><td class="column-3"><em>Get a range of elements from a list</em></td>
    </tr><tr class="row-26 even">
        <td class="column-1">LTRIM key start stop</td><td class="column-2">$redis-&gt;ltrim('a0', 0, 1);</td><td class="column-3"><em>Trim a list to the specified range</em></td>
    </tr><tr class="row-27 odd">
        <td class="column-1">LINDEX key index</td><td class="column-2">$redis-&gt;lindex('a0', 0);</td><td class="column-3"><em>Get an element from a list by its index</em></td>
    </tr><tr class="row-28 even">
        <td class="column-1">RPOP key</td><td class="column-2">$redis-&gt;pop('a0');</td><td class="column-3"><em>Remove and get the last element in a list</em></td>
    </tr><tr class="row-29 odd">
        <td class="column-1">LPOP key</td><td class="column-2">$redis-&gt;pop('a0', false);</td><td class="column-3"><em>Remove and get the first element in a list</em></td>
    </tr><tr class="row-30 even">
        <td class="column-1">LSET key index value</td><td class="column-2">$redis-&gt;lset('a0', 'ccc', 0);</td><td class="column-3"><em>Set the value of an element in a list by its index</em></td>
    </tr><tr class="row-31 odd">
        <td class="column-1">SADD key member [member ...]</td><td class="column-2">$redis-&gt;sadd('s0', 'aaa');</td><td class="column-3"><em>Add one or more members to a set</em></td>
    </tr><tr class="row-32 even">
        <td class="column-1">SREM key member [member ...]</td><td class="column-2">$redis-&gt;srem('s0', 'bbb');</td><td class="column-3"><em>Remove one or more members from a set</em></td>
    </tr><tr class="row-33 odd">
        <td class="column-1">SISMEMBER key member</td><td class="column-2">$redis-&gt;sismember('s0', 'aaa');</td><td class="column-3"><em>Determine if a given value is a member of a set</em></td>
    </tr><tr class="row-34 even">
        <td class="column-1">SINTER key [key ...]</td><td class="column-2">$redis-&gt;sinter(array('s0', 's1')), true)</td><td class="column-3"><em>Intersect multiple sets</em></td>
    </tr><tr class="row-35 odd">
        <td class="column-1">SMEMBERS key</td><td class="column-2">$redis-&gt;smembers('s1'), true);</td><td class="column-3"><em>Get all the members in a set</em></td>
    </tr><tr class="row-36 even">
        <td class="column-1">MOVE key db</td><td class="column-2">$redis-&gt;move('s1', 1);</td><td class="column-3"><em>Move a key to another database</em></td>
    </tr><tr class="row-37 odd">
        <td class="column-1">BGSAVE</td><td class="column-2">$redis-&gt;save(true);</td><td class="column-3"><em>Asynchronously save the dataset to disk</em></td>
    </tr><tr class="row-38 even">
        <td class="column-1">LASTSAVE</td><td class="column-2">$redis-&gt;lastsave();</td><td class="column-3"><em>Get the UNIX time stamp of the last successful save to disk</em></td>
    </tr><tr class="row-39 odd">
        <td class="column-1">HSET key field value</td><td class="column-2">$redis-&gt;hset('a', "field","value');<br>
$redis-&gt;hset('a', "field2","value");</td><td class="column-3"><em>Set the string value of a hash field</em></td>
    </tr><tr class="row-40 even">
        <td class="column-1">HGET key field<br>
</td><td class="column-2">$redis-&gt;hget('a','field')</td><td class="column-3"><em>Get the value of a hash field</em></td>
    </tr><tr class="row-41 odd">
        <td class="column-1">HGETALL key</td><td class="column-2">$redis-&gt;hgetall('a');</td><td class="column-3"><em>Get all the fields and values in a hash</em></td>
    </tr></tbody></table>

The above table covers pretty much all the commands in Redis. For more information on Commands available in Redis visit the [Command Reference](http://redis.io/commands "Redis Command Reference").

This concludes our tutorial on How to Connect to Redis from PHP using client Predis. I would like to extent my thanks to **Daniele Alessandri** for his excellent work in developing Predis library.

Update:
Table updated on May 28, 2012 with Predis Commands to work with Hashes Data Type of Redis.
