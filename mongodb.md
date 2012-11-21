---
title: Mongodb - random snippets
layout: wikistyle
---

### Patterns

Getting non-empty arrays
--------------------

A quick google turned up a suggestion that when checking to see if a
array / list is empty in mongo, the `$exists` operator is the way to go.

Because of the nature of how this construct is usually used (i imagine), it may not be apparent, but
this doesnt quite appear to do the trick. I.e. the returned objects also
include documents with empty (but existing, i.e. []) lists.

So how do you, in fact check for an empty list in mongo?

in python (py mongo):

{% highlight python %}
results = db.mycollection.find({'a_variable_on_the_object': { "$not" : {"$size" : 0}}})
{% endhighlight %}

<script type="text/javascript">

  var _gaq = _gaq || [];
  _gaq.push(['_setAccount', 'UA-36497876-1']);
  _gaq.push(['_setDomainName', 'github.com']);
  _gaq.push(['_setAllowLinker', true]);
  _gaq.push(['_trackPageview']);

  (function() {
    var ga = document.createElement('script'); ga.type = 'text/javascript'; ga.async = true;
    ga.src = ('https:' == document.location.protocol ? 'https://ssl' : 'http://www') + '.google-analytics.com/ga.js';
    var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ga, s);
  })();

</script>
