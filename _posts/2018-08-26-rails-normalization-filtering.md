---
layout: post
title: 'Rails 特殊字元過濾(白名單)'
date: 2018-08-26 17:21
comments: true
categories:
description: '使用 Rails 過濾特殊字元方法'
tags: Rails
---
[Rails HTML Sanitizers](https://apidock.com/rails/ActionView/Helpers/SanitizeHelper/sanitize)
```rb
filter_words = "Greg'\bs works" # \b: 表示退格鍵
ActionController::Base.helpers.sanitize(filter_words
=> "Greg's works"

ActionController::Base.helpers.sanitize("<script>I'am script</script>")
=> "I'am script"
```
{% capture string_with_newlines %}
[Ruby 正規則表示式](http://www.runoob.com/ruby/ruby-regular-expressions.html)
[rails sanitize](https://stackoverflow.com/questions/32553330/sanitize-helper-in-rails)
{% endcapture %}
{{ string_with_newlines | newline_to_br }}