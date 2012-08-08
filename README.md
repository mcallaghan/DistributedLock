python-distributed-lock
=======================

Python distributed lock (now only with memcached)

How to configure
------------------------

In your setup file (in django, settings.py), configure locking:

```python

    import distributed_lock
    distributed_lock.DEFAULT_MEMCACHED_CLIENT = memcache.Client(['127.0.0.1:11211'])
    distributed_lock.DEFAULT_TIMEOUT=60
    distributed_lock.DEFAULT_BLOCKING=False
```

If you setting up memcached in Django, you can use it abstraction of memcached.

```python

    from django.core.cache import cache
    distributed_lock.DEFAULT_MEMCACHED_CLIENT = cache
```

You can configure this settings in each lock, as parameter.


How to use
------------------------

Using minimal configuration, as decorator:

```python

    from distributed_lock import syncronized
    @syncronized()
    def hello_world():
        print 'running'
```

Or as `with` block:

```python

    from distributed_lock import syncronized_block
    ... my code before
    with syncronized_block('hello'):
        print 'running'
    ... my code after
```

You can use with conventional threading.Lock (only in process locking)

```python
    from distributed_lock import syncronized_block
    import threading
    with syncronized_block('hello', lock=threading.Lock())
        print 'running'
```

Arguments
------------------------

    def syncronized_block(key, lock=None, blocking=None)
    def syncronized(lock=None, blocking=None)

  * key: name of key in memcached. Avoid long names, because memcached supports only 255 characters in key. Mandatory. Using decorator
  key name will be class name + method name.
  * lock: If you desire use another lock strategy, like `threading.Lock()` or `threading.RLock()`. defaults to `distributed_lock.memcachedlock.MemcachedLock`
  * blocking: If another process has lock, wait until have lock or abort immediately.
  


