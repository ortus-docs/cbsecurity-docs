# Overview

The topic of security is always an interesting and essential one. However, most MVC frameworks offer very loose security when it comes down to an event-oriented architecture. ColdBox Interceptors change this behavior as we have the ability to intercept requests in any point in time during our application executions. With this feature we introduce our Security Module that implements what we call **ColdBox Security**. With the ColdBox security module you will be able to secure all your public and private ColdBox events from execution and incoming URL patterns. However, as we all know, every application has different requirements and since we are keen on extensibility, the module can be configured to work with whatever security authentications and permissions you might have.

![](https://github.com/ColdBox/cbox-security/wiki/ColdBoxSecurity.jpg)

The module wraps itself around the `preProcess` execution of your request and also \(if configured\) on any \`runEvent\(\)\`\` methods that get executed internally. The module is based on a custom rules engine that will validate a request against a set of rules that you define and then if a rule is matched, it will try to see if the user is authenticated and in a specific criteria \(as specified by you\). The two available authentication algorithms are the following:

1. \(**default**\) By using `cflogin, cfloginuser, cflogout`
2. Security Validation Object that you create and implement.

The default security is based on what ColdFusion gives you for basic security using their security tags. You basically use `cfloginuser` to log in a user and set their appropriate roles in the system. The module can then match to these roles via the security rules you have created.

The second method of authentication is based on your custom security logic. You will be able to register a validation object with the module. Once a rule is matched, the module will call your validation object, send in the rule and ask if the user can access it or not. It will be up to your logic to determine if the rule is satisfied or not.

### Features

* Secures incoming events and URIs
* If enabled, secure internal event executions via `preEvent()` interceptions
* Security rules can exist in:
  * XML File
  * JSON File
  * Database
  * Model/WireBox Object
  * IoC Module
  * ColdBox Cache \(Placing a query of rules into the cache\)
* The rules can be configured to use regular expressions or simple snippets
* Can use ColdFusion authentication security
* Can Use your own Security Validation by creating a security validation object.

### Resources

* `cflogin` - [http://help.adobe.com/en\_US/ColdFusion/9.0/CFMLRef/WSc3ff6d0ea77859461172e0811cbec22c24-7db9.html](http://help.adobe.com/en_US/ColdFusion/9.0/CFMLRef/WSc3ff6d0ea77859461172e0811cbec22c24-7db9.html)
* `cfloginuser` - [http://help.adobe.com/en\_US/ColdFusion/9.0/CFMLRef/WSc3ff6d0ea77859461172e0811cbec22c24-7c5c.html](http://help.adobe.com/en_US/ColdFusion/9.0/CFMLRef/WSc3ff6d0ea77859461172e0811cbec22c24-7c5c.html)
* `cflogout` - [http://help.adobe.com/en\_US/ColdFusion/9.0/CFMLRef/WSc3ff6d0ea77859461172e0811cbec22c24-7c5b.html](http://help.adobe.com/en_US/ColdFusion/9.0/CFMLRef/WSc3ff6d0ea77859461172e0811cbec22c24-7c5b.html)
* security tags - [http://help.adobe.com/en\_US/ColdFusion/9.0/CFMLRef/WSc3ff6d0ea77859461172e0811cbec17576-7ffc.html\#WSc3ff6d0ea77859461172e0811cbec22c24-7705](http://help.adobe.com/en_US/ColdFusion/9.0/CFMLRef/WSc3ff6d0ea77859461172e0811cbec17576-7ffc.html#WSc3ff6d0ea77859461172e0811cbec22c24-7705)

