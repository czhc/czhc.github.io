---
title: Fixing mysql2 installation for Homebrew MySQL
description: 
categories: [guides]
tags: [bundler, ruby, homebrew, mysql]
last_modified_at: "2021-01-03"
published: true
---

### Error

[Homebrew](https://brew.sh/) is a popular package manager to install local dependencies on macOS.
[Bundler](https://bundler.io/) is an application application package manager for Ruby and allows you to install different dependency versions per application environment. [`mysql2`](https://rubygems.org/gems/mysql2/versions/0.5.2) is a Ruby package/gem for managing connections to MySQL. 

Dependending on your setup, the default mysql path by Homebrew may not necessarily match the default path used by `bundler`. When running `bundle install`, you will see the following error: 

```
Using devise 4.8.1
Installing mysql2 0.5.3 with native extensions
Gem::Ext::BuildError: ERROR: Failed to build gem native
extension.

current directory:
/Users/foo/.rbenv/versions/3.0.3/lib/ruby/gems/3.0.0/gems/mysql2-0.5.3/ext/mysql2
/Users/foo/.rbenv/versions/3.0.3/bin/ruby -I
/Users/foo/.rbenv/versions/3.0.3/lib/ruby/3.0.0 -r
./siteconf20220103-55703-1aark7.rb extconf.rb
--with-opt-dir\=/usr/local/opt/openssl@3
--with-ldflags\=-L/opt/homebrew/Cellar/zstd/1.5.0/lib
checking for rb_absint_size()... yes
checking for rb_absint_singlebit_p()... yes
checking for rb_wait_for_single_fd()... yes
-----
Using mysql_config at /usr/local/bin/mysql_config
```

The default installation path of homebrew `mysql` libs are not discovered by mysql2: `Don't know how to set rpath on your system, if MySQL libraries are not in path mysql2 may not load`

### How to Fix

1. Check current `mysql` installation path:

    ```sh
    ➜  demo git:(main) which mysql
    /usr/local/bin/mysql

    ➜  demo git:(main) ls -l /usr/local/bin/mysql
    lrwxr-xr-x  1 foo  admin  34 Jan  3 14:56 /usr/local/bin/mysql -> ../Cellar/mysql/8.0.27_1/bin/mysql
    ```
    i.e. `/usr/local/Cellar/mysql/`

2. Set local mysql build paths: 

    ```sh
    bundle config --local build.mysql2 "--with-mysql-lib=/usr/local/Cellar/mysql/8.0.27_1/lib --with-mysql-dir=/usr/local/Cellar/mysql/8.0.27_1 --with-mysql-config=/usr/local/Cellar/mysql/8.0.27_1/bin/mysql_config  --with-mysql-include=/usr/local/Cellar/mysql/8.0.27_1/include"
    ```


3. Re-run `bundle install`: 

    ```sh
    Installing mysql2 0.5.3 with native extensions
    Bundle complete! 21 Gemfile dependencies, 78 gems now installed.
    Use `bundle info [gemname]` to see where a bundled gem is installed.
    ```
