# Rule Sources

The security firewall can be configured with rules that can come from many different sources:

* Declared inline in your `config/Coldbox.cfc`
* A JSON file
* An XML file
* From a model object via a method call
* From a database
* Declared inline in ANY module's `ModuleConfig.cfc`

{% hint style="warning" %}
When defining your rules source, you **ALWAYS** have to define the `rules` property. You specify an array of rules for inline, or \
`rules = "(db|json|xml|model)"`\
if you define your rules externally.\
If you have external rules you probably have to specify additional properties as explained in the next pages.
{% endhint %}

Let's start exploring these sources.
