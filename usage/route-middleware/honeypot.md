---
description: Catch form spam bots with the Honeypot middleware.
---

# Honeypot

Bots fill in every field they find. `Honeypot@cbsecurity` adds a trap field that people never see. If it arrives filled in, the request is a bot and your handler does not run.

```javascript
route( "/contact" ).middleware( "Honeypot@cbsecurity" ).to( "contact.send" )
```

Add the field to your form and hide it with CSS. Do not use `type="hidden"`, many bots skip those.

```html
<div style="position:absolute;left:-9999px" aria-hidden="true">
    <input type="text" name="website_url" tabindex="-1" autocomplete="off">
</div>
```

## What Bots Get

By default the bot gets a silent `200 OK`, so it believes it succeeded and does not adapt. Set `silent` to `false` to answer `422` instead.

| Route meta key | Setting | Default |
| --- | --- | --- |
| `honeypotField` | `middleware.honeypot.field` | `website_url` |
| `honeypotSilent` | `middleware.honeypot.silent` | `true` |

A honeypot stops simple bots only. Pair it with [Throttle](throttle.md) and use a CAPTCHA where the stakes are high.
