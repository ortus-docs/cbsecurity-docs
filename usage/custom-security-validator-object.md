# Custom Security Validator Object

A security validator object is a simple CFC that implements the following function:

```text
boolean userValidator( rule:struct, controller:coldbox.system.web.Controller )
```

This function must return a `boolean` variable and it must validate a user according to the rule that just ran by testing the fields that get sent in as a rule. Where this method exists is up to you. It will also receive a reference to the current ColdBox controller. You can use the controller to call other plugins, persist keys, or anything you like \(please see the controller API\). The important note here is that the rule structure contains all the elements/columns you defined in your xml or query. Below is a real life example:

```text
<!---  User Validator for security --->
<cffunction name="userValidator" access="public" returntype="boolean" output="false" hint="Verifies that the user is in any permission">
    <!---************************************************************** --->
    <cfargument name="rule"     required="true" type="struct"   hint="The rule to verify">
    <cfargument name="controller" type="any" required="true" hint="The coldbox controller" />
    <!---************************************************************** --->
    <!---  Local call to get the user object from the session --->
    <cfset var oUser = getUserSession()>
    <!---  The results boolean variable I will return --->
    <cfset var results = false>
    <!---  The permission I am checkin --->
    <cfset var thisPermission = "">
            
    <!---  Authorized Check, if true, then see if user is valid. This column is an additional column in my query --->
    <cfif arguments.rule['authorize_check'] and oUser.getisAuthorized()>
        <!---  I first check if the user is authorized or not if set in the db rules --->
        <cfset results = true>
    </cfif>
    
    <!---  Loop Over Permissions to see if my user is in any of them. --->
    <cfloop list="#arguments.rule['permissions']#" index="thisPermission">
    
        <!---  My user object has a method called check permission that I call with a permission to validate --->
        <cfif oUser.checkPermission( thisPermission ) >
            <!---  This permission existed, I only need one to match as per my business logic, so let's return and move on --->
            <cfset results = true>
            <cfbreak>
        </cfif>
    </cfloop>
    
    <!---  I now return whether the user can view the incoming rule or not --->
    <cfreturn results>
</cffunction>
```

Or, in `cfscript`:

```javascript
/** 
 * User Validator for security
 * 
 * @hint Verifies that the user is in any permission
 * @rule.hint The rule to verify
 * @controller.hint The ColdBox controller
 */
public boolean function userValidator( required struct rule, required any controller ) {
    // Local call to get the user object from the session
    var user = getUserSession();

    // Authorized Check, if true, then see if user is valid. This column is an additional column in my query
    if ( arguments.rule['authorize_check'] and user.getIsAuthorized() ) {
        return true;
    }
    
    // Loop Over Permissions to see if my user is in any of them.
    var permissionsArray = ListToArray(arguments.rule['permissions']);
    for (var permission in permissionsArray) {
        // My user object has a method called check permission that I call with a permission to validate
        if ( user.checkPermission( permission ) ) {
            // This permission existed, I only need one to match as per my business logic, so let's return and move on
            return true;
        }
    }

    // If we got here, the user does not have permission
    return false;
}
```

