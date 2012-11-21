---
title: Kafka - running Apache kafka on EC2
layout: wikistyle
---

### Hostnames / remotely accessing your cluster

Kafka brokers register in Zookeeper using their internal ip addresses by default
--------------------

This can be a bit of a pain if you want to be able to access kafka streams using the consumer from outside ec2 (where the private ip addresses wont work).
One simple solution to this is to specify hostnames in the broker config.

This will allow consumers to tranparently connect to the brokers. The only other thing you need to change is a tiny fix to the ConsumerOffsetChecker tool (if this is something you use). 

Here is the commit for that to work [https://github.com/jibs/kafka/commit/9f8b57bc11c3092f1ee2d7387e392b758ac5ed03](https://github.com/jibs/kafka/commit/9f8b57bc11c3092f1ee2d7387e392b758ac5ed03)

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