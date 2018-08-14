---
layout: post
title: How to create a blog with Github and Jekyll
categories:
- tech
tags: [github,jekyll,blog]
---

Steps that I used for setting up this blog.

---

# Create a blog with Github and Jekyll

## New Respository

1. create new respository named "{user name of github}.github.io"

2. git clone to your local disk

3. add a index.html and git push to your respository

4. visit your blog by "{user name of github}.github.io"

## Install Jekyll(in your respository folder)

1. install Ruby

    ```shell
    sudo apt get install ruby
    ```

2. install Jekyll

    ```shell
    sudo gem install jekyll
    ```

3. find a theme or create your own

    - [jekylltheme](http://jekyllthemes.org/)

4. download theme and copy to your local respository folder

5. install bundle

    ```shell
    sudo apt install ruby-bundler
    ```

6. update

    ```shell
    bundle install --path vendor/bundle
    ```

7. set up jekyll environment

    ```shell
    bundle exec jekyll serve
    ```

8. push to github

## invalid date '0000-00-00' from template

change in __config.yml

    exlude:
        - vendor

## IF "Gem::Ext::BuildError: ERROR: Failed to build gem native extension."
```shell
sudo apt-get install ruby-dev
bundle install --path vendor/bundle
```

## “Fail to install nakogiri"
[install libraries that nokogiri need](http://www.nokogiri.org/tutorials/installing_nokogiri.html)
 ```shell
sudo apt-get install build-essential patch ruby-dev zlib1g-dev liblzma-dev
```

## Fail to install ffi
```shell
sudo apt install libffi-dev
```
## Error: Address already in use - bind(2) for 127.0.0.1:4000

1. find out the PID or using program

    ```shell
    lsof -wni tcp:4000
    ```

2. kill that program

    ```shell
    kill -9 PID
    ```

## create local debug environment for Jekyll

in your blog folder:

    ```shell
    vim Gemfile
    ```

inside Gemfile, add these two lines

    ```
    source 'https://rubygems.org'      
    gem 'github-pages'
    ```

And then in your blog folder:

    ```shell
    bundle install --path vendor/bundle
    bundle exec jekyll serve
    ```
