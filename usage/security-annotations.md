# Security Annotations

The security module also allows you to secure your events via annotations instead of using security rules.  The setting that controls this security feature is the `handlerAnnotationSecurity` which can see in the [configuration section.](../getting-started/first-chapter.md#annotation-security)

The security module has a tiered approach to annotation security as it will check the handler component first and then the requested action method second.  You can apply different security contexts to each level as you see fit.

{% hint style="warning" %}
Please note that the security rules will be inspected first, annotations second.
{% endhint %}

## `Secure` Annotation

The firewall will inspect handlers for a `secured` annotation. This annotation can be added to the entire handler or to an action method or both. The default value of the `secured` annotation is a Boolean `true`. Which means, we need a user to be **authenticated** in order to access it.

```javascript
// Secure the entire handler
component secured{

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
```

## Authorization Context

You can also give the annotation a value, which can be anything you like: A list of roles, a role, a list of permissions, metadata, JSON, etc. Whatever it is, this is called the **authorization context** and the user validator must be able to not only authenticate but **authorize** the context or an invalid **authorization** will occur.

```javascript
// Secure this handler
component secured="admin,users"{

	function index(event,rc,prc) secured="list"{

	}
	
	function save(event,rc,prc) secured="write"{

	}

}
```

The secured value will be passed to the validator's for authorization.

## Cascading Security

By having the ability to annotate the handler and also the action you create a cascading security model where they need to be able to access the handler first **and only then** will the action be evaluated for access as well.









