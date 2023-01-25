---
description: Security annotations are used to secure your handler and/or handler actions
---

# Security Annotations

CBSecurity also allows you to secure your events via annotations instead of security rules.  The setting that controls this security feature is called `handlerAnnotationSecurity` , which can be set in the [configuration section.](../getting-started/configuration/#annotation-security)

The security module has a tiered approach to annotation security as it will check the handler component first and then the requested action method second.  You can apply different security contexts to each level as you see fit.

{% hint style="warning" %}
Please note that the security rules will be run first, and annotations second.
{% endhint %}

See the diagram below for inspecting security based on annotations:

![Annotation based security](../.gitbook/assets/AnnotationProcess.png)

## `Secure` Annotation

The firewall will inspect handlers for a `secured` annotation. This annotation can be added to the entire handler or to an action method, or both. The default value of the `secured` annotation is a Boolean `true`. This means we need a user to be **authenticated** to access the action.

<pre class="language-javascript"><code class="lang-javascript">// Secure the entire handler
component <a data-footnote-ref href="#user-content-fn-1">secured</a>{

	function index(event,rc,prc){}
	function list(event,rc,prc){}

}
// Same as this
component secured=true{
}

// Do NOT secure the handler
component secured=false{
}
// Same as this, no annotation!
component{

	function index(event,rc,prc) secured{
	}

	function list(event,rc,prc) secured="list"{

	}
	
}
</code></pre>

## Authorization Context

You can also give the annotation a value, which can be anything you like: A list of roles, a role, a list of permissions, metadata, JSON, etc. Whatever it is, this is called the **authorization context,** and the user validator must be able to authenticate and **authorize** the context, or an invalid **authorization** will occur.

```javascript
// Secure this handler
component secured="admin,users"{

	function index(event,rc,prc) secured="list"{

	}
	
	function save(event,rc,prc) secured="write"{

	}

}
```

The **secured** value will be passed to the validator for authorization.

## Cascading Security

By having the ability to annotate the handler and also the action, you create a cascading security model where they need to be able to access the handler first, **and only then** will the action be evaluated for access as well.









[^1]: The security annotation
