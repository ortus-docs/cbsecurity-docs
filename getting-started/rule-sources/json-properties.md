# JSON Rules

If you have already a JSON file with your rules, then all you need to do is add the path \(relative or absolute\) to that file in the `rules` configuration key.  However, the path MUST include the keyword `json` in it.

{% code title="config/Coldbox.cfc" %}
```javascript
moduleSettings = {
	// CB Security
	cbSecurity : {
		"rules" : "config/security.json.cfm"
};
```
{% endcode %}

  
Then your file can be something like this:

{% code title="config/security.json.cfm" %}
```javascript
[
    {
        "whitelist": "user\\.login,user\\.logout,^main.*",
        "securelist": "^user\\.*, ^admin",
        "match": "event",
        "roles": "admin",
        "permissions": "",
        "redirect": "user.login",
        "useSSL": false
    },
    {
        "whitelist": "",
        "securelist": "^shopping",
        "match": "url",
        "roles": "",
        "permissions": "shop,checkout",
        "redirect": "user.login",
        "useSSL": true
    }
]
```
{% endcode %}

