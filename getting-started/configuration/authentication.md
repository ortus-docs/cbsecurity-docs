# Authentication

## Authentication Services

cbsecurity ships with the [cbauth](https://github.com/elpete/cbauth) module that can provide you with a nice interface for authentication services. If you use the default `authenticationService` authenticationService@cbauth, you have to define the UserServiceClass in the cbauth module.\
However, you can plug in any WireBox ID and select your own authentication services.

{% hint style="warning" %}
If you are using cbauth as your `authenticationService` (the default), you also need to [configure cbauth.](https://cbauth.ortusbooks.com/installation-and-usage)
{% endhint %}

## User Services

cbsecurity will also require a user service if you will be dealing with any JWT security tokens. Just add your WireBox ID to the user service of your choice. If you are using cbauth, you have to define the UserServiceClass in the cbauth module.
