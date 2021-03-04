---
title: How does the Internet work? (from a web dev point of view)
date: "2021-02-28T22:40:32.169Z"
description: Exploring how the internet works - specifically, how does the browser load a website or web application? I'm using this question to get an in-depth understanding of web development.
---

# How does the internet work?

### (from a web development point of view)

When you plug into the internet, your machine is assigned an IP address.

(Qs: How do you plug into the internet? What does the ISP have access to that needs me to pay them for access? Why can't I just plug into the internet myself)

In a nutshell: your browswer is making a request to a server and getting a response back. 

The server is just another machine connected to the internet. When you open a browser and type in an address (mysite.com), your ISP does a DNS (domain name service) lookup. This looks up the IP address associated with the domain name (mysite.com).

Once the browser knows which IP address is configured to the desired website, the browser makes a request that that IP address. The request reaches the server with that particular IP address and it gives you a response. Normally, the response will contain a file and the file's content type. If the response is a file (index.html) it will come with the content type 'text/html'. With this information, the browser will know how to treat the html document. It will begin to parse through the html document.

(Qs: How does the browser parse through a document?)

A request will include a few headers. You can see the request headers by opening your browsers dev tools and looking under the 'Network' tab. The headers are: Host, Method, Path, Cookies, user-agent, Content Type. If posting data, a Post Body is also included.

The resoponse will include the following headers: Content Type, Status/Code. It will also include a Response Body: your file (html, css, js, image) or your results (json, xml, etc).

## What does the server world look like?

### For web sites

When a request comes into the server, it is in control of what to respond with. The most popular is LAMP (Linux, Apache, MySQL, PHP)

If you sign up for web hosting, you will most likely get a cpanel to manage files on the server. Apache is set up to listen to port 80, the basic http port. The reason someone can't access your files via your IP address is because you don't have a web server software running.

(Qs: What are ports?)

If the server gets a request with no path (mysite.com) then it will respond with the index. If it gets a request with a path (mysite.com/img/logo2.jpg) then it will respond with the file.

### For web application

Let's assume the web application is also listening to port 80. For a web application like twitter.com, a unique landing page with unique information needs to be constructed before the browser can load it. The server will first go into the database (MySQL or MongoDB) and retrieve user information and tweets. Then it will load this information into an index template. The server will generate html on the fly and send it back to the browser.