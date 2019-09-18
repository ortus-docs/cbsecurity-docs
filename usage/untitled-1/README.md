# Security Rules

Now that we have seen all the properties of this module, how to configure it and declare it, let's look at how the rules work. All the security rules follows the following format, however, you can append as many columns as you like for your own custom validations and the interceptor will register all the columns you pass to it:

| Property | Type | Description |
| :--- | :--- | :--- |
| `match` | event or URI | Determines if it needs to match the incoming URI or the incoming event. By default it matches the incoming event. |
| `whitelist` | varchar | A comma delimited list of events or patterns to whitelist or to bypass security on |
| `securelist` | varchar | A comma delimited list of events or patterns to secure |
| `roles` | varchar | A comma delimited list of roles that can access these secure events |
| `permissions` | varchar | A comma delimited list of permissions that can access these secure events |
| `redirect` | varchar | An event or route to redirect if the user is not in a role or permission |

You might be asking, why is the element permissions here, if the default ColdFusion security doesn't work with permissions but with roles. Well, in anticipation to customizations and permissions based systems, the permissions element exists. It's there already if you need to use it. However, the default interceptor security does not use it, just the roles element. A few guidelines:

* Test your regular expressions \([http://gskinner.com/blog/archives/2008/03/regexr\_free\_onl.html](http://gskinner.com/blog/archives/2008/03/regexr_free_onl.html)\)
* Determine if you want to secure the incoming URL or event
* Order of declaration of the rules is important as they are fired in order
* Test your `whitelist` events, if not you might be producing endless loops of security redirections
* Make sure implicit events are whitelisted if you are securing the entire application events
* As best practice, create your own validator object for more granular control

> **IMPORTANT** The basic rules table is provided. However, it is very denormalized and for simple applications. If your applications will deal with many permissions and roles, I suggest you build your DB according to your business rules and then just create a method that can join the rules into the above format. You can easily do this in SQL or create a view for your security rules. If you are doing this, then your security implementations are advanced and you know what you are doing \(Hopefully\). So please note that the way you store the rules in the DB is your priority and consideration.

