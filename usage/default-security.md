# Default Security

This module will try to use ColdFusion's `cflogin + cfloginuser`authentication by default. However, if you are using your own authentication mechanisms you can still use this module by implementing a Security Validator Object \(See next section\). Below we can see a sample on how to use the `cflogin` tag:

Example:

```text
<cflogin>
    
    Your login logic here
    
    <---  Log in the user with appropriate credentials --->
    <cfloginuser name="name" password="password" roles="ROLES HERE">
</cflogin>

<---  Some Real sample --->
<cflogin>
    <cfif getUserService().authenticate(rc.username,rc.password)>
        <cfloginuser name="#rc.username#" password="#rc.password#" roles="#getUserService().getRoles(rc.username)#" />
    </cfif>
</cflogin>
```

For more information about `cflogin, cfloginuser and cflogout`, please visit the docs [http://cfdocs.org/security-functions](http://cfdocs.org/security-functions)

