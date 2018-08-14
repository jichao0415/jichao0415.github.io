---
layout: post
title: Add Search Funtion to jekyll blog
category: tech
tags: [github,jekyll,blog,search]
---

Because I really like this [Lagom theme](https://github.com/swanson/lagom), but there is no searching function in it. I found this simple-jekyll-search in the Internet, and it is really really light, cool, very very simple and quick to install it(Although I encountered some problems while adding them). I will write down the steps that I used for adding searching function to my blog.

---

# Add Search Funtion to jekyll blog

My installation follows:

1. Chinese version from [blog of 琯琯](https://www.jianshu.com/p/064e2422f7c7)

2. Offitial manual of [simple jekyll search](https://github.com/christian-fei/Simple-Jekyll-Search), you can also see some examples here.

## Step 1: Install simple-jekyll-search

Install simple-jekyll-search from npm:
```shell
npm install simple-jekyll-search
```
## Step 2: add .json file

create a search.json file in your root folder, contents of this file are:

>How to include this kind of excution code(as Liquid templating engine in jekyll) in markdown, see [answer](https://stackoverflow.com/questions/22044488/jekyll-code-in-jekyll)

{% raw %}
```json
---
layout: null
---
[
  {% for post in site.posts %}
    {
      "title"    : "{{ post.title | escape }}",
      "category" : "{{ post.category }}",
      "tags"     : "{{ post.tags | join: ', ' }}",
      "url"      : "{{ site.baseurl }}{{ post.url }}",
      "date"     : "{{ post.date }}"
    } {% unless forloop.last %},{% endunless %}
  {% endfor %}
]
```
{% endraw %}

## Step 3: add HTML code

What you need for this search function are two containers. **input field** and **result field**. By default, you can easily copy the code in your html file, where you want to put your search function in. Normally, you may want to integrate it in the index.html in root folder.

```html
<div id="search-container">
  <!--input field-->
  <input type="text" id="search-input" placeholder="search...">
  <!--result field-->
  <ul id="results-container"></ul>
</div>
```

## Step 4: add CSS code

To make your input box more beautiful, I just copied the code from [blog of 琯琯](https://www.jianshu.com/p/064e2422f7c7). You can make a new search.css file and type in what you want for input styling.

```css
#search-input {
    width: 90%;
    height: 35px;
    color: #333;
    background-color: rgba(227,231,236,.2);
    line-height: 35px;
    padding:0px 16px;
    border: 1px solid #c0c0c0;
    font-size: 16px;
    font-weight: bold;
    border-radius: 17px;
    outline: none;
    box-sizing: border-box;
    box-shadow: inset 0 1px 1px rgba(0,0,0,.075), 0 0 8px rgba(102,175,233,.6);
}
#search-input:focus {
    outline: none;
    border-color: rgb(102, 175, 233);
    background-color: #fff;
    box-shadow: inset 0 1px 1px rgba(0,0,0,.075), 0 0 8px #007fff;
}
```

## Step 5: add javascript code

Now, it's time to make your searching function work! You must add several javascript code to listen and really do searching.

>Important: js code must be read later than html code, and html code later than css code.

### 5.1: import source code of simple-jekyll-search

You may use the source code of simple-jekyll-search.min.js online by using:

```html
<script src="https://cdn.rawgit.com/christian-fei/Simple-Jekyll-Search/master/dest/simple-jekyll-search.min.js"></script>
```
Or download it in your js folder and input it:

```html
  <script src="{{ site.baseurl }}/js/simple-jekyll-search.min.js"></script>
```

### 5.2 add your searching configuration

You can also copy the following code in your html file, or you can make a new search_setttings.js to do it.
```html
  <script>
    SimpleJekyllSearch({
      //3 fields you must have
      searchInput: document.getElementById('search-input'),
      resultsContainer: document.getElementById('results-container'),
      json: '/search.json',
      //optional field
      searchResultTemplate: '<li><a href="{{ site.url }}{url}">{title}</a></li>'
    })
  </script>
```
Other searching options can be found in [wiki](https://github.com/christian-fei/Simple-Jekyll-Search/wiki#options)

Thanks [Christian Fei](https://christianfei.com/), the developer of simplr-jekyll-search!