---
description: >-
  CBSecurity stores the secured incoming url so you can relocate the user to it
  after authenticating.
---

# Secured URL

The security module has the concept of a secured URL which is the actual URL that got intercepted and relocated because of a security exception.  This is stored in the request collection as `rc._securedURL` and in the ColdBox flash memory as `_securedURL`.

So always remember to use this variable to provide a seamless login experience to your users. You can easily place it in the login form as a hidden field:

```html
#html.startForm( action=prc.xehDoLogin, name="loginForm" )#
    
    <!--- Store the _securedURL so we can use it to relocate -->
    #html.hiddenField( name="_securedURL", value=event.getValue('_securedURL','') )#

    #html.textfield(name="username",label="Username: ",size="40",required="required",class="textfield",value=prc.rememberMe)#
    #html.passwordField(name="password",label="Password: ",size="40",required="required",class="textfield")#

    <div id="loginButtonbar">
        #html.checkBox(name="rememberMe",value=true,checked=(len(prc.rememberMe)))# 
        #html.label(field="rememberMe",content="Remember Me  ",class="inline")#
        #html.submitButton(value="  Log In  ",class="buttonred")#
    </div>

    <br/>
    <img src="#prc.cbRoot#/includes/images/lock.png" alt="lostPassword" />
    <a href="#event.buildLink( prc.xehLostPassword )#">Lost your password?</a> 

#html.endForm()#
```

In your login action you can use the secured URL and relocate appropriately:

```javascript
function doLogin( event, rc, prc ){

   if( cbSecure().authenticate( rc.username, rc.password ) ){
      
      rc._securedURL.len() ? relocate( url : rc._securedURL ) : relocate( "admin.dashboard" )
      
   }

}
```
