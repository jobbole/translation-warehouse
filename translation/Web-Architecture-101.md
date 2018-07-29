---
editor: transadmin
published: false
title: Web Architecture 101
---
The basic architecture concepts I wish I knew when I was getting started as a web developer  



![Modern web application architecture overview](https://cdn-images-1.medium.com/max/800/1*K6M-x-6e39jMq_c-2xqZIQ.png)


> The above diagram is a fairly good representation of our architecture at Storyblocks. If you’re not an experienced web developer, you’ll likely find it complicated. The walk through below should make it more approachable before we dive into the details of each component.


> A user searches on Google for “Strong Beautiful Fog And Sunbeams In The Forest”. The first result happens to be from Storyblocks, our leading stock photo and vectors site. The user clicks the result which redirects their browser to the image details page. Underneath the hood the user’s browser sends a request to a DNS server to lookup how to contact Storyblocks, and then sends the request.
The request hits our load balancer, which randomly chooses one of the 10 or so web servers we have running the site at the time to process the request. The web server looks up some information about the image from our caching service and fetches the remaining data about it from the database. We notice that the color profile for the image has not been computed yet, so we send a “color profile” job to our job queue, which our job servers will process asynchronously, updating the database appropriately with the results.
Next, we attempt to find similar photos by sending a request to our full text search service using the title of the photo as input. The user happens to be a logged into Storyblocks as a member so we look up his account information from our account service. Finally, we fire off a page view event to our data firehose to be recorded on our cloud storage system and eventually loaded into our data warehouse, which analysts use to help answer questions about the business.
The server now renders the view as HTML and sends it back to the user’s browser, passing first through the load balancer. The page contains Javascript and CSS assets that we load into our cloud storage system, which is connected to our CDN, so the user’s browser contacts the CDN to retrieve the content. Lastly, the browser visibly renders the page for the user to see.

