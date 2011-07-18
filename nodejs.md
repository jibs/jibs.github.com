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


{% highlight javascript %}
  console.log('will this work?');
{% endhighlight %}


