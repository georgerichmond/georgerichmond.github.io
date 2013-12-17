---
layout: default
title:  "The slow death of XML"
date:   2013-12-12 09:54:06
categories: xml
permalink: xml-slow-death-of
---

XML came along as the answer to everybody's data problems. Well it isn't. The industry is finally moving away from this rigid and verbose data structure, but I'm sure it will hang on a long time. a bit like VGA connectors in conference rooms.

XSLT - a little programming language for turning some XML data into some other XML data, most commonly an HTML document for reading. Generally for example for each element in an array of elements you would output a table row element with the nicely formatted data inside. 

This sounds great, until you actually try writing these things and testing them. I did quite a lot of XSLT a few years ago and it's a great way to burn lots of time. Debugging is a bit of a nightmare and you usually have to examine every character of your code to see where you went wrong. If I have to deal with XML now I'd far rather build a simple Ruby/Sinatra app and parse the XML using something like Nokogiri.

I saw a great presentation at QCon 2012 called [XML Is Great, But I'd Rather Be Coding...](http://qconlondon.com/london-2012/presentation/XML%20Is%20Great,%20But%20I'd%20Rather%20Be%20Coding...) which highlighted some of the flaws of XML, particularly how it's verbose nature makes it totally unsuitable for high volume transactions, leading banks to use their own proprietary data formats.

I used to work on Java applications that called XML based services and then turned the response into a Java object graph, based on an XML schema document (XSD). These services were often consumed by multiple applications and the desire for order and rigidity by specifying a schema resulted in increased risk, as a small change required by one application (such as adding a field) meant that everyone had to update their XSDs and release, turning what could have been a simple change by one team into a massive co-ordinated affair. People thought they were being enormously productive by generating all the getters and setters from their IDE but really they were just creating more stuff to maintain and hiding away the true meaning of what their code was trying to do. I've worked quite hard to get rid of this madness and push my department towards JSON/REST, whilst fighting off attempts to over-engineer these services by inventing JSON schemas.

And don't get me started on SOAP. It amazes me that the more enterprisey vendors still choose SOAP/XML over the far simpler and more elegant JSON/REST option. The funny thing its it has so many rules which makes engineers feel warm and protected, but in practice these rules are actually nonsense and completely broken as [this article](http://wanderingbarque.com/nonintersecting/2006/11/15/the-s-stands-for-simple/) illustrates. I've rarely found a case where the WSDL actually works and isn't just completely ignored. If you have to work with SOAP, I'd recommend wrapping it as quickly as possible, and for Ruby developers the [Savon gem](http://savonrb.com/) goes some way to hiding the madness.