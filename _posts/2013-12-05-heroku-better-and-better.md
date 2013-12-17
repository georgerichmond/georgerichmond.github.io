---
layout: default
title:  "Heroku just keeps getting better and better"
date:   2013-12-05 09:54:06
categories: heroku cloud
permalink: heroku-better-and-better
---

#Heroku just keeps getting better and better

It's been a couple of years since we put our first app on Heroku and I still think it's brilliant.

The big problem we had before platform as a service came along was getting new server instances provisioned for new apps. This meant we tended to 'wang in' functionality into existing applications that pretty much crapped all over the 'low coupling, high cohesion' pattern. This was also contributing to our excessively long builds.

Another team started work on a small standalone shop, an ideal candidate for an experimental cloud deployment as it didn't require any customer data service calls. Heroku was the first choice but unfortunately came a cropper when we found that the goods supplier insisted on requests coming from a fixed IP, something that wasn't possible on Heroku at the time (there is now an addon that lets you do this). So they went with Engine Yard. I've used Engine Yard a few times and whilst you do get a high degree of control, it's almost too much control, requiring you to think about details you don't really care about.

Fast forward a few months and we were building our first application on Heroku. We've learnt some lessons along the way - here they are.

Naturally you want your dev, test and staging environments to be as similar to production as possible. So we set up a heroku-test-APPNAME, stage-APPNAME, pre-prod-APPNAME and prod-APPNAME environment. I can't empasise enough how important that pre-prod environment is, and that it must be exactly the same as your production environment so you can be confident your production configuration settings are correct before you deploy to prod and find it blows up. Sometimes you don't need the staging environment - we only use it when we have staging services that we want to connect to. 

We initially always ran all our tests after deploying to a Heroku test environment, but found the deployment time before being able to run the first test annoying. A new guy got us into running Rack::Test tests rather than Selenium, which are a lot quicker, but these tests need to run against a local server rather than a remote one. We now generally run our first suite of tests against a local instance of the application as the latency is reduced, you can run very fast Rack::Test tests when Javascript isn't required and there is no deployment time to Heroku. I did take some convincing however as I was very attached to the idea of similar environments, but so far the trade-off has been worth it. However we continue to run our staging and pre-production test against Heroku of course.

A really handy beta feature called Heroku pipeline saves you time promoting your build through multiple Heroku environments. You tell Heroku what the upstream application is and then when you call pipeline:promote, it just pushes the same slug (sort of like the compiled app with all gems installed) straight up to the next environment. It is very quick and has shaved a few valuable minutes off our build.

I love the way that it 'just works' for the common addons like Postgresql for Rails. You generally don't need to configure your production database settings as these are done for you by Heroku during deployment.

You can now create an app in the European region, meaning reduced latency for your European users.

Like a lot of these services, Heroku and it's addons are pretty much free when you are in dev and cheap when you have a few users, but the prices (especially for the addons) shoot up when you get bigger. That's a champagne problem however and we've found that we can handle a very high load with lots of concurrent requests by being careful about blocking and using multi-threaded server middleware like Rainbows.

