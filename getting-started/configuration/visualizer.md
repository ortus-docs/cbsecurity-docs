---
description: Configuring the CBSecurity Visualizer
---

# 🔬 Visualizer

The CBSecurity visualizer is a tool that will allow you to visualize all of your configuration settings, firewall logs, and much more.  By default, the visualizer is **disabled**.

<figure><img src="../../.gitbook/assets/cbsecurity-3-visualizer (1).png" alt=""><figcaption><p>Visualizer</p></figcaption></figure>

{% hint style="danger" %}
If you enable the visualizer, we highly suggest you **secure** it.
{% endhint %}

If enabled, you can visit the `/cbsecurity` entry point, and you will get the visualizer rendered. &#x20;

## Configuration

Here are the configuration settings for the visualizer:

```javascript
/**
* --------------------------------------------------------------------------
* Security Visualizer
* --------------------------------------------------------------------------
* This is a debugging panel that when active, a developer can visualize security settings and more.
* You can use the `securityRule` to define what rule you want to use to secure the visualizer but make sure the `secured` flag is turned to true.
* You don't have to specify the `secureList` key, we will do that for you.
*/
visualizer : {
	"enabled"      : false,
	"secured"      : false,
	"securityRule" : {}
},
```

### enabled

If `false` then no visualizer, if `true` then you get a visualizer :tada:

### secured

We highly encourage you to ensure the visualizer is ONLY accessible if you have authenticated into your system.  By using a `secured=true` then CBSecurity will incorporate a rule to secure the visualizer for ONLY authenticated users.  If you want to be picky, use the `securityRule` setting.

### securityRule

We also recommend that ONLY certain users have access to the visualizer. You can accomplish this by adding the keys to the security rule created for the visualizer.  For example, I only want `admins` or users with the `cbsecurity-visualizer` permission to access it.

```javascript
visualizer : {
	"enabled"      : true,
	"secured"      : true,
	"securityRule" : {
		"roles" : "admins",
		"permissions" : "cbsecurity-visualizer"
	}
}
```

## Requirements

Please note that the security visualizer can ONLY visualize if you have [firewall logs enabled](firewall/#logs).  If no logs are enabled or configured, then the visualizer WILL NOT WORK.  Here is a simple logs configuration in the firewall

```javascript
firewall : {

    "logs" : {
        "enabled"    : true,
        "dsn"        : "myapp",
        "schema"     : "",
        "table"      : "cbsecurity_logs",
        "autoCreate" : true
    }
    
}
```

{% hint style="warning" %}
The `dsn` key is optional, and CBSecurity will inspect the Application.cfc settings for a default datasource: `this.datasource`
{% endhint %}
