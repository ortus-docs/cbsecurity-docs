# Secured URL

## `_securedURL`

The security module has the concept of a secured URL which is the actual URL that got intercepted and relocated because of a security exception. If the module detects an invalid authentication or authorization and an action must be issued, then the firewall will store this URL in the `RC` scope and flash it so it can be available in the next request (if a relocation occurs).

The flash RAM variable is called: `_securedURL`. This key will be persisted in the flash memory of the framework and when the user gets relocated to the `redirect` element, this key will be populated in the request collection automatically for you.

So always remember to use this key if you want to provide a seamless login experience to your users. You can easily place it in the login form:

```markup
#html.startForm(action=prc.xehDoLogin,name="loginForm")#

    #html.hiddenField(name="_securedURL",value=event.getValue('_securedURL',''))#

    #html.textfield(name="username",label="Username: ",size="40",required="required",class="textfield",value=prc.rememberMe)#
    #html.passwordField(name="password",label="Password: ",size="40",required="required",class="textfield")#

    <div id="loginButtonbar">
        #html.checkBox(name="rememberMe",value=true,checked=(len(prc.rememberMe)))# 
        #html.label(field="rememberMe",content="Remember Me  ",class="inline")#
        #html.submitButton(value="  Log In  ",class="buttonred")#
    </div>

    <br/>
    <img src="#prc.cbRoot#/includes/images/lock.png" alt="lostPassword" />
    <a href="#event.buildLink(prc.xehLostPassword)#">Lost your password?</a> 

#html.endForm()#
```
