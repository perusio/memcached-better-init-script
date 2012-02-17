# A Better Init script for Memcached

## Introduction

This is an init script for [memcached](http://www.memcached.org/) that uses a **specific**
directory for the memcached configuration: `/etc/memcached`.

It's th erefore much easier to manage your memcache configuration since
it's now restricted to a specific subdirectory of `/etc`. 

## Multiple server layout

Now it's easy to implement a multiserver setup. Just copy each file to
a file named:
    
    /etc/memcached_server1.conf
    /etc/memcached_server2.conf
            .
            .
            .
    /etc/memcached_serverN.conf
   
Now you can either start/stop/restart all of your servers with a
single command like, for example:

    service memcached start 

Or start/stop/restart a specific server, for example server 3, like
this:

    service memcached start server3

Of course the numeric naming of the files is a mere convenience you
can **name** them as you wish as long as you maintain the pattern of
the underscore for separating the server name. For example, server 2
works caching [drupal](http://drupal.org) menus. Providing a service
before provided by the `cache_menu` database table. You could name the
specific memcached server like this:

    memcached_server_cache_menu.conf

for example.

## Installation alongside the Debian Wheezy Memcached package

 1. Create the `/etc/memcached` directory.
 
 2. Clone the repo 
    
        git clone git://github.com/perusio/memcached-better-init-script.git
        
 3. Move your memcached configuration file(s) to `/etc/memcached`. 
 
 4. Copy this script to `/etc/init.d`.
 
 5. Issue `service memcached force-reload`
 
 6. Done.

## For installations alongside the Debian Squeeze Memcached package you'll need to edit another file

    /usr/share/memcached/scripts/start-memcached
    
    Around line 27, once the $pidfile variable has been initially set you'll need to implement the below 4 lines
    
    if (scalar(@ARGV) == 2) {
        $etcfile = shift(@ARGV);
        $pidfile = shift(@ARGV);
    }



## Credits

The original init script is part of the debian testing distribution
[`wheezy`](http://packages.debian.org/wheezy/memcached). YMMV on other distrubutions.
