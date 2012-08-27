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

- Url session parameter in the simplest case can be copy pasted by mistake by the users thus.

- Servlet3 session specifications let us specify some

## Session stealing


### HttpOnly cookies
- Explain added benefit of using HttpOnly cookies.


### XSS attacks

### NoScript explained
