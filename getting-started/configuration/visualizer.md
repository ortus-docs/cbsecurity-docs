# Visualizer



## Security Visualizer

ColdBox security comes with a nice graphical visualizer for all the registered security rules and settings in your global firewall. You can enable it by using the enableSecurityVisualizer setting.

```javascript
enableSecurityVisualizer :  true
```

You can then visit the `/cbsecurity` URL and you will be presented with this magical tool:

![](https://raw.githubusercontent.com/coldbox-modules/cbsecurity/development/test-harness/visualizer.png)

{% hint style="danger" %}
**Important** The visualizer is **disabled** by default and if it detects an environment of production, it will disable itself.
{% endhint %}
