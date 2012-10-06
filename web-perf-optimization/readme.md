web-performance-optimization
===
This is a technical lecture about discussing best practices for web site speed optimizations such as:
  - Tools for gathering the metrics about performance and suggesting improvements.
  -
  - Using CDNs. How they work.
  - Http Caching. Apache settings for triggering browser caching by looking at content type.GZipping responses.
  - JS and CSS merging and minify. Tools for merge&minify.
  - Javascript tricks.
    - Async javascript.
    - Deferred image loading.
    - JQuery, CSS selectors optimizations.
    - WebWorkers.
  - SPDY protocol


Due to the different topics reached this lecture may be split into two parts.


Talk Time
---------
  about 2 hours.


Contents
--------

## Generic rules about improving performance.
   - RTT - Round Trip Time - refers to the overhead needed by a web request(DNS lookup, connection setup) not considering the time for actual data transfer and thus not related to bandwidth.
   - Minimize the number of requests needed: CSS Sprites, Merging of CSS and JS files.
   - Try to make more simultaneous requests. Browsers have a maximum of simultaneous connection that is "per hostname". Browser dependent see [[Browserscope]http://www.browserscope.org/?category=network].

   - Try not block use the
   - Caching of course is great because the best performance would be 0 time which in turn would be to not download the resource but instead retrieve it from somewhere local.

## Tools for measuring the performance and suggesting improvements.
   - Google Chrome network timeline. Reading response headers for the caching info.
   - Browser tools like "YSlow" and "PageSpeed Insights" provide easy hints on improving performance.
   - [Web Page Test](http://www.webpagetest.org/), [Monotis](http://pageload.monitis.com/) or [Gomez](http://www.gomeznetworks.com/custom/instant_test.html)
        Metrics shown:
            - DNS lookup time(time to lookup the host name and match it to an IP)
            - Connection time(time it takes for setting up the connection - handshaking, sending the request headers)
            - Time to First Byte(the time it takes to receive a first byte of data as a response)
            - Content download.(time it takes for the resource to download).


## Using CDNs.
   - Theory behind is that you should distribute and serve the resources need by the users from servers that are "close" to them.
   ![Edge Servers](../img/cdn.png)

### How CDNs work:
   - Think DNS are like a phonebook for web sites. When connecting to a website your computer is first making a query to a DNS Server.
   - A DNS setup may look like:
    js.web.de.              CNAME   js.web.de.edgekey.net.
    js.web.de.edgekey.net   CNAME   e5416.g.akamaiedge.net.
    e5416.g.akamaiedge.net. A       2.16.109.234

   - Check it out using the "dig" command:

    $ dig js.web.de


    That CNAME(Canonical name) acts like an alias and triggers another another lookup .
   - Most common is Akamai
    Problem with

   - Common misconception is that they only can act like FTP servers where content gets uploaded when in fact they act more like proxy servers.

    ![CDN as Proxy](http://www.nczonline.net/blog/wp-content/uploads/2011/11/cdn2.png)
   -

### Using multiple CDNs
   - Remember that a browser simultaneous connections to the same host is limited so for ex. there can only be 6 on Chrome.
But we can further split the resources from cnd.web.de to js.web.de, img.web.de, css.web.de.
   - Problem with this can be that relative reference will no longer work for ex: from http://css.web.de/style.css
referencing a relative image 'background-image:url('../img/paper.gif');'. Could be solved using placeholders like ''background-image:url('${img_cdn}/paper.gif');' which get replaced at build time according to different profiles.
   - Downside that needs additional DNS lookup to resolve more hosts might outweigh the benefit of parallelization.

### Using most common used libraries with a CDN
   - Most "famous" libraries already have their files on "famous" CDN locations.
   By using those you also have the benefit that the user when arriving on your site he probably already has visited other sites that included the libraries so they are served actually from the cache.
   - On the same line of thinking, you might think that using Github references for those and hoping for caching. Github is great, but it was not meant for this and but doesn't set the cache headers when serving files.
   - Look for . Some CDNs offered space on hosting


## Merging JS files and CSS files.
   - Reason behind merging of JS and CSS resources is related to making fewer requests.
   - In development it makes sense to have the JS logic distributed into separate files according to the .
   -
   - wro4j invokes the through java.

