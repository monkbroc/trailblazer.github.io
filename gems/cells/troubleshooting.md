---
layout: default
---

# Troubleshooting

## Helper Inclusion Order

One of the many problems with Rails helper implementation is that the inclusion order matters. This can lead to problems with the following exception.

{% highlight ruby %}
undefined method `output_buffer=' for #<Comment::Cell:0xb518d8cc>
{% endhighlight %}

Usually, the problem arises when you have initializers to setup your application cell. When including helpers here, they might be included before the `cells` gem has a chance to include its fixes.

Please include your template engine module explicitly then, _after_ your standard helper inclusions.

{% highlight ruby %}
# config/initializers/trailblazer.rb

Cell::Concept.class_eval do
  include ActionView::Helpers::TranslationHelpers # include your helpers here.
  include Cell::Erb # or Cell::Slim, or Cell::Haml after that
end
{% endhighlight %}