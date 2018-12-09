# \_securedURL key

When the security module is about to relocate _redirect_ element, it will save the incoming URL that was requested in a flash RAM variable called: `_securedURL`. This key will be persisted in the flash memory of the framework and when the user get's relocated to the `redirect` element, this key will exist in the request collection. You can then use this to store where they came from and do proper redirections. So always remember to use this key if you want to provide a seamless login experience to your users. You can easily place it in the login form:

```markup
#html.startForm(action=prc.xehDoLogin,name="loginForm",novalidate="novalidate")#
    <---< Secured URL --->
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

