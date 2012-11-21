---
title: Nodejs - wiki from the battlefront
layout: wikistyle
---

### Patterns

Getting POST vars
--------------------

The data event is emitted when a req contains data, however this is
chunked (and so should not be used until the `end` event has been
emitted).

So, do something like this:

{% highlight js %}

  function doResponse (req, res) {
    //other handling code
    query.post = req.content;

    res.writeHead(200, {
      'Content-Type' : 'application/x-javascript',
    });
    res.end();
  }


  function mainReqHandler(req, res) {
    req.content = '';
    req.setEncoding("utf8");

    if (req.method == 'POST') {
      req.addListener("data", function(chunk) {
        // post request can be multi-part so we have to wait for end
        req.content += chunk;
      });

      req.addListener("end", function() {
        // query.post = req.content;
        doResponse(req, res);
      });
    } else {
      // standard non POST request is done immediately
      doResponse(req, res);
    }	
  }

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
