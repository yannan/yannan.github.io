---
layout: post
title: 【译文】使用PhantomCSS进行CSS的回归测试(Using PhantomCSS for Regression Testing Your CSS)
description: "UI自动化测试"
modified: 2016-09-01
tags: [sample post]
image:
  feature: abstract-1.jpg
---

{% highlight ruby linenos %}
module Jekyll
  class TagIndex < Page
    def initialize(site, base, dir, tag)
      @site = site
      @base = base
      @dir = dir
      @name = 'index.html'
      self.process(@name)
      self.read_yaml(File.join(base, '_layouts'), 'tag_index.html')
      self.data['tag'] = tag
      tag_title_prefix = site.config['tag_title_prefix'] || 'Tagged: '
      tag_title_suffix = site.config['tag_title_suffix'] || '&#8211;'
      self.data['title'] = "#{tag_title_prefix}#{tag}"
      self.data['description'] = "An archive of posts tagged #{tag}."
    end
  end
end
{% endhighlight %}



  

