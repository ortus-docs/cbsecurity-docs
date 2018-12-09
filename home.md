# How It Works

The basics of the security validation is that you define a set of rules, much like how you define a firewall. Each rule is composed of several elements: **securelist**, **whitelist**, **roles**, **permissions**, **match**, and **redirect**. However, each rule can be expanded by the developer as needed with custom elements, etc. Each rule will be evaluated in the order that it is declared and the follow validation via our flow diagram below.

1. An incoming request or internal event reaches the first rule and the type of matching is determined: event or URI matching
2. The incoming event or URI is matched against the _whitelist_ element
   1. If matched, then the event is whitelisted so it continues to the next rule
   2. Else, continue
3. The incoming event or URI is matched against the _securelist_ element
   1. If not matched, then continue to next rule
   2. Else, continue validation
4. Do we have a custom validator or not?
   1. Yes, validate against the custom validator
   2. No, validate against ColdFusion's logged in user roles or logged in credentials
5. If validation fails, redirect the user via the _redirect_ element

