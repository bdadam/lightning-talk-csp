# Content Security Policy

Adam Beres-Deak

--

> Content Security Policy (CSP) is an added layer of security that helps to detect and mitigate certain types of attacks, including Cross Site Scripting (XSS) and data injection attacks. These attacks are used for everything from data theft to site defacement or distribution of malware.

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

# Examples

--

Starter Policy
```
default-src 'none'; script-src 'self'; connect-src 'self'; img-src 'self'; style-src 'self';
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

# Report Only

Directives are monitored but not enforeced.

```
Content-Security-Policy-Report-Only:
	default-src https:; report-uri /csp-violation-report
```