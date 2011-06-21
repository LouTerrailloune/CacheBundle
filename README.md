winzouCacheBundle
============

What's that?
--------------
winzouCacheBundle provides a simple cache management. Now you can use a cache system without reinventing it.
It supports Apc, XCache, File, ZendData and Array.

Usage
-----
In your controller:

    $cache = $this->get('winzou_cache.apc');
    // or
    $cache = $this->get('winzou_cache.file');
    // or
    $cache = $this->get('winzou_cache.memcache');
    // or
    $cache = $this->get('winzou_cache.array');
    // or
    $cache = $this->get('winzou_cache.xcache');
    // or
    $cache = $this->get('winzou_cache.zenddata');
    // or
    $cache = $this->get('winzou_cache'); // in that case, it will use the default driver defined in config.yml, see below

    $cache->save('bar', array('foo', 'bar'));
    $bar = $cache->fetch('bar');

See Cache\AbstractCache for all the available methods.

Configuration
-------------
When using FileCache, if you don't want to store your cache files in `CACHE_DIR/winzou_cache` (default value), then define the absolute path in your config.yml:

    winzou_cache:
        options:
            cache_dir: %kernel.cache_dir%/MyAppCache
    # or    cache_dir: /tmp/MyAppCache/%kernel.environment%

If you want to define in only one place the driver you want to use, you will like the default_driver option:

    winzou_cache:
        options:
            default_driver: apc # default is "file"

You can now access the FileCache with the `winzou_cache` service. And if you want to change the driver, you have to modify only one value in your config.yml.

Raw access
----------
You can overwrite any option just by using the factory service. See these two very similar methods:

    $factory = $this->get('winzou_cache.factory');
    $cache = $factory->get('file', array('cache_dir' => '/tmp/cache'));

Or by defining your own service:

    your_cache:
        factory_service: winzou_cache.factory
        factory_method:  get
        class:           %winzou_cache.driver.file%
        arguments:
            - file
            - [ {'cache_dir': /tmp/cache} ]
    
    # and then $cache = $this->get('your_cache')
