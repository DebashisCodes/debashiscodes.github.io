---
date: 2024-08-02 12:00:00
layout: post
title: Define An Endless Method In Ruby 3
subtitle:
description:
image: "/assets/img/code.png"
optimized_image:
category: Ruby
tags:
  - ruby
author: debashisbiswal
---

Prior to Ruby 3, methods could be defined as follows:

```ruby
def do_something
  puts "Something!"
end
```

or

```ruby
def do_something; puts "Something!"; end
```

With Ruby 3, methods can be defined without the need for the 'end' keyword.

```ruby
def do_something = puts "Something!"
```
Endless method definitions are well-suited for one-line methods.
