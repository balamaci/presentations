web-performance-optimization
===
This is a technical lecture about discussing best practices for web site speed optimizations such as:
  - Tools for gathering the metrics about performance and suggesting improvements.
  -
  - Using CDNs. How they work.
  - Http Caching. Apache settings for triggering browser caching for different served resources. HTML5 LocalStorage.
  - JS and CSS merging and minify. Tools for merge&minify.
  - Sending less bytes on the wire: GZipping server response.
  - Javascript tricks.
    - JQuery, CSS selectors optimizations.
    - Async javascript.
    - Deferred image loading.
    - WebWorkers.
  - SPDY protocol


Due to the different topics reached this lecture may be split into two parts.


Talk Time
---------
  about 2.5 hours.


Contents
--------

## Generic rules about improving performance.
   - RTT - Round Trip Time - refers to the overhead needed by a web request(DNS lookup, connection setup) not considering the time for actual data transfer and thus not related to bandwidth. CDNs and **KeepAlive** can help with this.
   - Minimize the number of requests needed: CSS Sprites, Merging of CSS and JS files, Data-URIs.
   - Try to make more simultaneous requests. Browsers have a maximum of simultaneous connections opened "per hostname". Browser dependent see [Browserscope](http://www.browserscope.org/?category=network).
   - "Progressive enhancement" - Try to set a "critical path" that would render the page in a seaming usable state for the user.
   Deffer the rest of the 3rd party scripts and images after this is finished and while the user is probably looking at the products or figuring out what to do next.
   (Putting only core js files that are at the page bottom
   - Caching of course is great because the best performance would be to not need to make a network request which in turn would mean we'd already have the resource locally.

## Tools for measuring the performance and suggesting improvements.
   - Chrome/Firefox Network timeline. The easiest and most at hand tool for seeing what is going on.
   - Browser tools like "YSlow" and "PageSpeed Insights" provide easy hints on improving performance.
   - Sites for checking page loading time:
     - [Web Page Test](http://www.webpagetest.org/)
     - Others : [GMetrix](http://gtmetrix.com/), [Monotis](http://pageload.monitis.com/), [SitePerf](http://site-perf.com/), [Pingdom](http://tools.pingdom.com/), [Gomez](http://www.gomeznetworks.com/custom/instant_test.html)
       Metrics shown:

         - DNS lookup time(time to lookup the host name and match it to an IP)
         - Connection time(time it takes for setting up the connection - handshaking, sending the request headers)
         - Time to First Byte(the time it takes to receive a first byte of data as a response)
         - Content download.(time it takes for the resource to download).
   - [ZoomPF] () - lists in details and clearly the problems with a site.
   They also have a nice video series where they apply their own tool to look at major sites and pinpoint their perf problems.

   - [Cuzzilion]() Tools for delaying a .
   - [DelayMe] it's a server in node.js that a collegue offered to build . I was not content with the fact that Cuzzilion you can start locally and start serving your own page with
   - Great that you can see who contributed

## Using CDNs.
   - Theory behind is that you should distribute and serve the resources need by the users from servers that are "close" to them.
   ![Edge Servers](../img/cdn.png)

### How CDNs work:
  - Think DNS are like a phonebook for web sites. When connecting to a website your computer is first making a query to a DNS Server that may know the entry or .

  A DNS setup may look like:
      ```
      js.web.de.              CNAME   js.web.de.edgekey.net.
      js.web.de.edgekey.net   CNAME   e5416.g.akamaiedge.net.
      e5416.g.akamaiedge.net. A       2.16.109.234
      ```
  Check it out using the "dig" command:

      $ dig js.web.de


  That **CNAME**(Canonical name) acts like an alias and triggers another lookup which queries the DNS of the CDN and it
responds by looking up the closest CDN to the requester.

  - Problem with using Google Public DNS is that the CDN will see the request coming from Google DNS and suggest edge servers close to it instead of you.
    By using your ISP's DNS which probably is geographically close to you you'll also get directed to close CDNs.
  - Some CDN providers: Akamai, Amazon Cloudfront

  - Common misconception is that they only can act like some mirroring FTP servers where content gets refreshed periodically from the origin server.

  In fact they act more like reverse proxy servers something Amazon Cloudfront called [custom origin](http://aws.typepad.com/aws/2010/11/amazon-cloudfront-support-for-custom-origins.html)
    ![CDN as Proxy](http://www.nczonline.net/blog/wp-content/uploads/2011/11/cdn2.png)

    If the resource is not found on the edge server, that server requests it from the 'origin server' and caches it locally on the edge.
    Next request that comes for that resource will get served from the edge server's cache.

### Using multiple CDNs
   - Remember that a browser simultaneous max connections to the same host is limited so for ex. there can only be 6 on Chrome.
But we can further split the resources from cnd.web.de to js.web.de, img.web.de, css.web.de or even more with having img1.web.de imgX.web.de.

   - Problem with this can be that relative reference will no longer work for ex: from http://css.web.de/style.css
referencing a relative image `background-image:url('../img/paper.gif');` will not work.
    Could be solved using placeholders like `background-image:url('${img_cdn}/paper.gif');` which get replaced at build time according to different profiles.

   - Downside that additional hosts need additional DNS lookup to resolve more hosts might outweigh the benefit of parallelization.

### Using most common used libraries with a CDN
   - Most "famous" libraries already have their files on "famous" CDN locations.
   By using those you also have the benefit that the user when arriving on your site he probably already has visited other sites that included the libraries so they are served actually from the cache.
   - On the same line of thinking, you might think that using Github references for those and hoping for caching. Github is great, but it was not meant for this and but doesn't set the cache headers when serving files.
   - Checkout http://cdnjs.com/index.html for a great collection of CDN hosted js libraries. You can also make a request to add what you need.

## Merging and minify JS files and CSS files.
   - Reason behind merging of JS and CSS resources is related to making fewer requests and minimize the RTT.
Older browsers used to serialize requests of JS resources to in order to prevent unnecessary downloading of resources if the script being downloaded could have used document.write to comment out '<!--' on the next.

Image from Firefox 3.0.
   ![Serial requests JS](https://developers.google.com/speed/docs/insights/images/externaljs1.png)
   ![Paralel requests JS](http://balamaci.github.com/presentations/web-perf-optimization/img/apple-waterfall-small.png)
Note that this is no longer true, even for browser like IE8 according to Browserscope, JS resources are downloaded in parallel.

   - In development it makes sense to have the JS logic distributed into separate files according to the function they serve, while in production we'd want

   - Minimization of JS files: removes spaces and comments, renames function and variable names.
        - You'd think that enabling gzip compression would have the same effect: statistics have shown still an additional 5-10% of the size in improvement and since it's an easy way to do it, why not?
   - There are several tools to minimize the JS and CSS files: YUI Compressor, Dojo compressor, Uglify js, Google Closure compiler, Jawr for css, etc.

### Wro4j
  - [Web Resource Optimizer for Java](http://code.google.com/p/wro4j/) has all the tools mentioned above and more.
  - Provides the concept of "groups" of resources. Can combine several js/css files into one entity.
   ![Wro4j groups](http://wro4j.googlecode.com/svn/wiki/img/resourceMerging.png)

  groups can be defined in a **wro4j.xml** file

      <groups xmlns="http://www.isdc.ro/wro">
        <group name="core">
         <js>/js/views/app-view.js</js>
         <js>/js/views/file-browser-view.js</js>
         <js>/js/models/*.js</js>
         <js>classpath:com/mysite/resource/js/1.js</js>
         <css>classpath:com/mysite/resource/css/1.css</css>
         <group-ref>plugins</group-ref>
        </group>

        <group name="plugins">
          <js>/js/plugins/jquery.modal.js</js>
          <js>/js/plugins/jquery.progressbar.js</js>
        </group>
      </groups>

  and we are using the above created groups:

      <html>
       <head>
        <title>Wro Test</title>
        <link rel="stylesheet" type="text/css" href="/wro/core.css" />
        <script type="text/javascript" src="/wro/core.js"></script>
       </head>
       <body>
         //Body
       </body>
      </html>

  - Concept of Pre/Post "Processors": a series of useful "plugins" that alter the file at the time of before or after the merging.
  - Library has a web filter that you can add to **web.xml** and can group and process the resource files on the fly with the request.
  This is useful in development to check on the fly when you change something and you want to refresh the page.

  - Can also be invoked on the command line with a 'java -jar wro4j-runner-1.xxxxx-jar-with-dependencies.jar'.


#### Useful Wro4j Processors
  - **LESS Processor**: You can also include **".less"** files as group resources and by running this processor it will trigger **less.js** to parse those files into .css.

  - **Base64DataUriSupport**:
        - Using DataURI is another performance trick to use to save on making another request to the server.
        By using it you can embed the contents of an image inside css file and thus save the trip to request a small red_dot.png for example.

        <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAUA
          AAAFCAYAAACNbyblAAAAHElEQVQI12P4//8/w38GIAXDIBKE0DHxgljNBAAO
          9TXL0Y4OHwAAAABJRU5ErkJggg==" alt="Red dot">

    -This processor looks for 'background-image' and if the referenced image is less than 32KB(IE8 acceptable) transform it do data-uri.

  - **JS Lint** and **CSS Lint**: "Findbugs" for JS and CSS.

#### Good Maven integration
  - There is a Maven plugin that allows to run wro4j as a build step in your projects lifecycle and create the groups and apply the processors at **build time**.

## Compressing text on the wire

   Browser negotiate the possible communication by sending an **Accept-Encoding** header with the request. This way the browser announces to the server
   what "languages"(encodings) it speaks so that the server knows to respond according:

       Accept-Encoding: gzip, deflate, sdch

   and receiving back:

       Content-Encoding: gzip

   if the content was served up in a gziped form.
  - Checking that you succesfully implemented this is to look for response header **Content-Encoding**.


### What to compress
  - Some resources are already compressed(.png, .jpeg, .pdf) and retrying to compress them would only be a waste of time and CPU resources.
  - As a general rule text compresses best so ideal candidates for gzipping are(html, css, js, xml) but not only those, think also at .ico and web fonts files.
  - Best checkout the .htaccess file

### Other useful performance tips
  - **No 404s** Check the application to not request resources that are no longer available and return 404 status code responses.
  It takes up a slot in the max parallel requests to a host. This is bad especially for not found JS files since it may block the flow until the 404 response comes back.

  - **204 Status code - No Content** - the world's smallest component with a body of 0 bytes can be used for logging and other purposes for which developers usually use a 1x1 GIF tracking pixel.


## Browser Caching
  - There are several Http Headers that control what resources get cached, for how long and trigger cache revalidation:

#### "Expires Sun, 14 Jan 2014 08:23:12 GMT"         /   "Cache-Control: max-age=3600"
  When the browser encounters one of those headers it will cache the resource and for the time specified will
  consider it fresh and will not try to revalidate and download by issuing other GET request to the server.

  -It's redundant to set both **Expires** and **Cache-Control**, because **Cache-Control** always takes precedence over the other.


#### "Last-Modified:Sun, 19 Jun 2011 06:59:22 GMT"   /   "ETag:103212179"
  This headers, are used for validation because the browser may issue GET requests to the server to check if the resource
  has been modified on the server.

  - **Last-Modified** it returns in response the last date when the resource was modified on the server. If the resource's valid period
    had expired then the browser will not issue a GET request but with an added header **"If-Modified-Since"** for the resource and the server might just say the resource
    the browser has is still valid since it didn't change on the server and just return an empty response content body but with **304 Not Modified** status
    and thus save on the download time.
      ![LastModified](http://betterexplained.com/wp-content/uploads/compression/HTTP-caching-last-modified_1.png)

  - **ETag** is a programatically generated value to mean an unique identifier for a resource, a fingerprint or version.
  So to check the validity of the resource on the browser, the server has to respond to a check request if there is a new version of the resource.
  There is the same logic behind as in **Last-Modified** case only that now the browser issues an **"If-None-Match"** header.
      ![ETag](http://betterexplained.com/wp-content/uploads/compression/HTTP_caching_if_none_match.png)
  -
  - **ETag** also can be used for REST entities caching to verify that another client application has the latest version of the entity.


  - Setting **Last-Modified** with no **Expires** or **Cache-Control** triggers an heuristic setting of max-age which for IE9 is max-age = (DownloadTime - LastModified) * 0.1
  - Apache needs the presence of the **mod_expires** module to configure these caching headers. Again see the nice *.htaccess* file of the HTML5Boilerplate.

  - What you need to understand is that Apache only add these Caching Headers to a web response. It does in no way turn the Apache server into a caching machine serving itself cacheable Tomcat resources. Two different users requesting the
same resource will be served from the "backend" Tomcat server. In order to do this we must read more on how to turn the frontend into a *Caching Reverse Proxy* server.


### How to use caching
  - You typically would want to use long future expiration times(months, years) so you eliminate even the checking validity round trips to the server.

  - On the other hand you also want the users get a new version of the css immediately as it is available, so they don't have to wait a month to see the nice way in which you fixed your layout for example.

  - **Solution**: Reference the cacheable resource along with some kind of version token in a file that is not cached
  (most common the .html files). One could use for ex:
   - the file timestamp on disk `<script type="text/javascript" src="/js/site-B0439F858745C84EA46792F9AC42B6CF.js"></script>` - this would rather be ok for a development version not a production env to have to check on the server the status of the file
   - a specific file version `<link rel="stylesheet" type="text/css" href="/css/site.css?ver=1.23" />`
   - application version `<link rel="stylesheet" type="text/css" href="/2.14/css/site.css" />` - what we currently use

### Using an Apache http server as a Caching Reverse Proxy
  - *mod_cache* graduated from experimental in 2.2 version.
  - Using Nginx - the new kid on the block. Takes a different approach as a frontend being an event based server so rather than spawning a new thread for every new request it .
   While some argue about the one thing is certain that it takes less resources than Apache to serve a large number of requests.
   Also since it's new(and does not carry any legacy support baggage) the configuration is less complicated than the Apache.

## JS Optimizations

### Putting JS at the bottom
  - When browsers encounter a <script> tag they stop what they are doing and begin downloading and aftewards executing the script.
  This is a 'synchronous blocking' behaviour. All the browsers can do is to look ahead in the DOM to begin downloading the next scripts/images in the html.
  In older browser versions(pre IE8) they were not even looking and starting the downloads of other scripts because they were 'afraid' the downloading script might
  redirect to another page or comment out the following code after the script and so have wasted the time on the downloads.

  - They cannot begin executing the other downloaded scripts below because those scripts might rely on the blocking script.
  Think of the *JQuery* dependency of other libraries.

  - The idea is about "progressive rendering" to render the page in an no and then progressively add more . or wh
  - See the example of the "White Page of Death".
   We have prepared some examples .



  - domContentLoaded - fired when the 'document' object has been created, jQuery hooks onto this when you're doing **$.ready()**
  - onLoad all files have finished loading.

### Protect your page from SinglePoint of Failures of the 3rd party scripts
  - HTML5 introduces the "async" tag for scripts lie '<script async="async" />.
  - Async scripts when encountered the browser does not block but instead the browser begins the download and when finished it will execute.
  - You can't make any assumptions about .
  - They are great .



### JQuery optimizations
  - Know selector rules:
   - ID & tag element selector are fastest: $('#id, form, input') because backed up by native js. See it here:
   - $('.class') fast backed up by native getElementByClassname(not supported in <IE8).

    - Chaining:

     DONT:
        ```
        $("#cart").addClass("active");
        $("#cart").css("color","#f20");
        $("#cart").height(300);
        ```
     DO:
        ```
        $("#cart").addClass("active").css("color","#f0f").height(300);
        ```

    - Event delegation: Event listeners cost memory and processing power. Imagine the case where you want to add events to all the buttons in a table's row:

        ```
        $('table').find('td').click(function() {
            $(this).toggleClass('active');
        });
        ```

      That means as many events listeners created as the number of table rows. Instead we try to use a single event listener:

         ```
         $('table').on('click','td',function() {
             $(this).toggleClass('active');
         });
         ```


### Deferred image loading:
  - Images take longer to download and some may not really be essential to the first page impression so it would make
  sense to load them at a more appropriate time after other more important resources have loaded.
  For ex. some images might be visible only after scrolling down the page(think Gravatar/Disqus User images, or in an image carousel when a timer triggers the next slide.
  The browser will not know those images are not even visible on the page, it only know that while parsing the DOM and encountering <img src=""> it will request
  the images from the server.

  - Instead of having `<img src="tiger.png"/>` why not have `<img data-src="tiger.png"/>` for the non-critical images.
  - We can still preserve the old way images were loaded for the users who don't have JS enabled by wrapping them in a <noscript> tag:
    ```
     <noscript></noscript>
    ```

  - At the page bottom have a js script that replaces the "data-src" attribute to "src" something like:
    ```
    function deferredImageLoading() {
        $('img[data-src]').each(function() {
             $(this).attr('src', $(this).attr('data-src'));
        });
    }
    ```


Sources of info
----------------
   - The Book of Speed http://www.bookofspeed.com/ - great book for performance tips - sadly unfinished but great
   - Steve Sounders blog - http://www.stevesouders.com/
   - https://webforscher.wordpress.com/
   - JQuery optimizations http://24ways.org/2011/your-jquery-now-with-less-suck
   - Caching headers explained http://www.symkat.com/understanding-http-caching
   - Guypo blog http://www.guypo.com/
   - ZoomPF video series: http://www.guypo.com/