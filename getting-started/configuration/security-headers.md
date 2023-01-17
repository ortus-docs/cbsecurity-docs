---
description: Configuring the security response headers and features
---

# ☢ Security Headers

CBSecurity comes bundled with tons of security response features to help developers be secure-minded about their applications.  Here are the defaults for configuring the security headers in CBSecurity.

<pre class="language-javascript" data-line-numbers><code class="lang-javascript">/**
 * --------------------------------------------------------------------------
 * Security Headers
 * --------------------------------------------------------------------------
 * This section is the way to configure cbsecurity for header detection, inspection and setting for common
 * security exploits like XSS, ClickJacking, Host Spoofing, IP Spoofing, Non SSL usage, HSTS and much more.
 */
securityHeaders : {
<strong>	// If you trust the upstream then we will check the upstream first for specific headers
</strong>	"trustUpstream"         : false,
	// Content Security Policy
	// Content Security Policy (CSP) is an added layer of security that helps to detect and mitigate certain types of attacks,
	// including Cross-Site Scripting (XSS) and data injection attacks. These attacks are used for everything from data theft, to
	// site defacement, to malware distribution.
	// https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP
	"contentSecurityPolicy" : {
		// Disabled by defautl as it is totally customizable
		"enabled" : false,
		// The custom policy to use, by default we don't include any
		"policy"  : ""
	},
	// The X-Content-Type-Options response HTTP header is a marker used by the server to indicate that the MIME types advertised in
	// the Content-Type headers should be followed and not be changed => X-Content-Type-Options: nosniff
	// https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Content-Type-Options
	"contentTypeOptions" : { "enabled" : true },
	"customHeaders"      : {
		 // Name : value pairs as you see fit.
	},
	// Disable Click jacking: X-Frame-Options: DENY OR SAMEORIGIN
	// https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options
	"frameOptions" : { "enabled" : true, "value" : "SAMEORIGIN" },
	// HTTP Strict Transport Security (HSTS)
	// The HTTP Strict-Transport-Security response header (often abbreviated as HSTS)
	// informs browsers that the site should only be accessed using HTTPS, and that any future attempts to access it
	// using HTTP should automatically be converted to HTTPS.
	// https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Strict-Transport-Security,
	"hsts"         : {
		"enabled"           : true,
		// The time, in seconds, that the browser should remember that a site is only to be accessed using HTTPS, 1 year is the default
		"max-age"           : "31536000",
		// See Preloading Strict Transport Security for details. Not part of the specification.
		"preload"           : false,
		// If this optional parameter is specified, this rule applies to all of the site's subdomains as well.
		"includeSubDomains" : false
	},
	// Validates the host or x-forwarded-host to an allowed list of valid hosts
	"hostHeaderValidation" : {
		"enabled"      : false,
		// Allowed hosts list
		"allowedHosts" : ""
	},
	// Validates the ip address of the incoming request
	"ipValidation" : {
		"enabled"    : false,
		// Allowed IP list
		"allowedIPs" : ""
	},
	// The Referrer-Policy HTTP header controls how much referrer information (sent with the Referer header) should be included with requests.
	// Aside from the HTTP header, you can set this policy in HTML.
	// https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Referrer-Policy
	"referrerPolicy"     : { "enabled" : true, "policy" : "same-origin" },
	// Detect if the incoming requests are NON-SSL and if enabled, redirect with SSL
	"secureSSLRedirects" : { "enabled" : false },
	// Some browsers have built in support for filtering out reflected XSS attacks. Not foolproof, but it assists in XSS protection.
	// https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-XSS-Protection,
	// X-XSS-Protection: 1; mode=block
	"xssProtection"      : { "enabled" : true, "mode" : "block" }
}
</code></pre>

### TrustUpstream

This boolean flag tells CBSecurity whether to inspect `x-forwarded-` headers FIRST instead of traditional host/IP headers.  If you trust your proxies, then turn this setting to `true`.

### ContentSecurityPolicy

The Content Security Policy (CSP) is an added layer of security that helps to detect and mitigate certain types of attacks, including Cross-Site Scripting (XSS) and data injection attacks. These attacks are used for data theft, site defacement, and malware distribution. &#x20;

By default, this policy is disabled as it requires a custom policy to be written according to your needs.  Once you have a policy available, you can add it to the configuration.

{% embed url="https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP" %}
Read more about content security policies
{% endembed %}

```javascript
"contentSecurityPolicy" : {
    // Disabled by defautl as it is totally customizable
    "enabled" : true,
    // The custom policy to use, by default we don't include any
    "policy"  : "default-src 'self' *.example.com; img-src *"
},
```

### ContentTypeOptions

The `X-Content-Type-Options` response HTTP header is a marker used by the server to indicate that the MIME types advertised in the `Content-Type` headers should be followed and not be changed.  This produces the following header => `X-Content-Type-Options: nosniff` &#x20;

{% embed url="https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Content-Type-Options" %}
Read more about content type options
{% endembed %}

```javascript
"contentTypeOptions" : { "enabled" : true },
```

### CustomHeaders

You can fill out this struct with the custom headers you would like to send out on EVERY request.  The header value can be a simple value to return always or a closure/lambda that will be executed at runtime and the value sent on every request.

```javascript
customHeaders : {
    "x-mvc" : "ColdBox",
    "x-runtime-timestamp" : (event,rc,prc) => now()
}
```

Please note that the closure accepts the incoming `event, rc, and prc` variables.

### FrameOptions

The **`X-Frame-Options`** [HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP) response header can be used to indicate whether or not a browser should be allowed to render a page in a [`<frame>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/frame), [`<iframe>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe), [`<embed>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/embed) or [`<object>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/object). Sites can use this to avoid [click-jacking](https://developer.mozilla.org/en-US/docs/Web/Security/Types\_of\_attacks#click-jacking) attacks by ensuring that their content is not embedded into other sites.  The default value ColdBox uses is `SAMEORIGIN` which allows iframes and embeds from the same origin.  The available values are: `SAMEORIGIN OR DENY`.

{% embed url="https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options" %}
Read more about frame options
{% endembed %}

```javascript
"frameOptions" : { "enabled" : true, "value" : "SAMEORIGIN" },
```

### HSTS - HTTP Strict Transport Security

The HTTP Strict-Transport-Security response header (often abbreviated as HSTS) informs browsers that the site should only be accessed using HTTPS and that any future attempts to access it using HTTP should automatically be converted to HTTPS.  Here are the defaults we use in CBSecurity:

{% embed url="https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Strict-Transport-Security" %}
Read more about HSTS
{% endembed %}

```javascript
"hsts" : { 
    "enabled" : true, 
    // The time, in seconds, that the browser should remember that a site is only to 
    // be accessed using HTTPS, 1 year is the default 
    "max-age" : "31536000", 
    // See Preloading Strict Transport Security for details. Not part of the specification. 
    "preload" : false, 
    // If this optional parameter is specified, this rule applies to all of the site's subdomains as well. 
    "includeSubDomains" : false 
},
```

### HostHeaderValidation

This configuration setting can restrict access to your application for ONLY a specific list of hosts.  This prevents host spoofing.  If an invalid host is detected, a 401 Not Authorized response will be sent back to the user.  This setting is **disabled** by default.

```javascript
// Validates the host or x-forwarded-host to an allowed list of valid hosts
"hostHeaderValidation" : {
	"enabled"      : true,
	// Allowed hosts list
	"allowedHosts" : "www.coldbox.org,coldbox.org"
},
```

### IPValidation

This configuration setting can restrict access to your application for ONLY a specific list of IP addresses.  This prevents IP spoofing.  If an invalid IP is detected, then a 401 Not Authorized response will be sent back to the user.  This setting is **disabled** by default.

{% hint style="warning" %}
Please note that as of now, a full IP address must be used.
{% endhint %}

```javascript
// Validates the host or x-forwarded-host to an allowed list of valid hosts
"ipValidation" : {
	"enabled"      : true,
	// Allowed hosts list
	"allowedHosts" : "127.0.0.1,98.98.98.98"
},
```

### ReferrerPolicy

The Referrer-Policy HTTP header controls how much referrer information (sent with the Referer header) should be included with requests.  Aside from the HTTP header, you can set this policy in HTML.  This setting is **enabled** by default with a policy of `same-origin`.

{% embed url="https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Referrer-Policy" %}
Read more about referrer policies
{% endembed %}

```javascript
"referrerPolicy"     : { 
    "enabled" : true, 
    "policy" : "same-origin" 
},
```

Here are some available policies:

```
Policy: no-referrer
Policy: no-referrer-when-downgrade
Policy: origin
Policy: origin-when-cross-origin
Policy: same-origin
Policy: strict-origin
Policy: strict-origin-when-cross-origin
Policy: unsafe-url
```

### SecureSSLRedirects

Detect if the incoming requests are NON-SSL and redirect with SSL alongside any incoming query strings and host information if enabled.  By default, this setting is **disabled**.

```javascript
"secureSSLRedirects" : { "enabled" : true },
```

### XSSProtection

Some browsers have built-in support for filtering out reflected XSS attacks. Not foolproof, but it assists in XSS protection.  By default, it is **enabled** and a `block` mode is produced.

{% embed url="https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-XSS-Protection" %}
Read more about XSS protection
{% endembed %}

```javascript
"xssProtection"      : { 
    "enabled" : true, 
    "mode" : "block" 
}
```
