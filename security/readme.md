practical-web-security-presentation
===
This is a technical lecture about practical . This is addressed to the web developers and security testers.


Talk Time
---------
  about 1 hour.


Contents
--------

## Anatomy of a web request and response
### The analysis tools explained.
    - Looking at the web request through Chrome network panel.
    - Burp proxy.

### Web request/response Headers:
    - Discussing Set-Cookie.


## Session attacks
### The web session explained.
- Why the need the http session? Http protocol is stateless, the web session is the most basic way of making a relation between
- How is it created? We analise the response coming from the server that sets the JSESSION cookie.


### Benefits of Cookie vs URL parameter sessions

- URLs with their parameters are recorded in various log files(ISPs, proxy-s).

- The simplest case of session disclosure can be the user copy-pasting the url and sharing it by posting to a site(say Facebook) or by submitting .

- Servlet3 session specifications let us specify some

## Session stealing
- The idea of using an additional attribute for checking the fact that is the same user. User-Agent

### HttpOnly cookies
- Explain added benefit of using HttpOnly cookies.


### XSS attacks

### NoScript explained
