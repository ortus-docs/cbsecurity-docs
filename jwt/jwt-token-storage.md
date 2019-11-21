# Token Storage

You can enable token storage in cbsecurity via the `tokenStorage` setting. By default it is **enabled** and leverages CacheBox's `default` cache using a key prefix of `cbjwt_` + the token's unique identifier claim of `jti`.

{% hint style="danger" %}
We recommend that you create a separate provider for the cache.
{% endhint %}

## Why use a storage?

The storage of keys are great in order to visualize in your application all the registered keys in the system. You can also invalidate keys, as by default if the token does not exist in the storage, it is considered invalid.

You can retrieve the token storage by injection or the helper method:

```javascript
property name="tokenStorage" inject="DBTokenStorage@cbsecurity";
property name="tokenStorage" inject="CacheTokenStorage@cbsecurity";

jwtAuth().getTokenStorage()
```

## Storage Drivers

We ship with two drivers:

* `cachebox` : Leverages any cache registered in CacheBox
* `db` : Leverages a database table to store the keys

### **CacheBox Driver Properties**

* `cacheName` : The cache to use

### **DB Driver Properties**

* `table`   : The table to use for storage
* `schema`  : A schema to use if the database supports it, else empty
* `dns`     : The datasource to use, defaults to the one set in `Application.cfc`
* `autoCreate:true` : Autocreate the table if not found
* `rotationDays:7` : How many days should the expiration be before removal
* `rotationFrequency:60` : How many minutes should pass before issuing a rotation check

The columns it will create are:

* `id` - identifier
* `cacheKey` - The unique cacke key, indexed
* `token` - The encrypted token
* `expiration` - The expiration
* `issued` - The issue date
* `subject` - The subject identifier

## Custom Token Storage

If you would like to create your own token storage, just add your own WireBox ID to the `driver`, `properties` and implement the following interface: `cbsecurity.interfaces.jwt.IJwtStorage`

{% code title="cbsecurity.interfaces.jwt.IJwtStorage.cfc" %}
```javascript
interface{

    /**
     * Configure the storage by passing in the properties
     * 
     * @return JWTStorage
     */
    any function configure( required properties );

    /**
     * Set a token in the storage
     * 
     * @key The cache key
     * @token The token to store
     * @expiration The token expiration
     * 
     * @return JWTStorage
     */
    any function set( required key, required token, required expiration );

    /**
     * Verify if the passed in token key exists
     * 
     * @key The cache key
     */
    boolean function exists( required key );

    /**
     * Retrieve the token via the cache key, if the key doesn't exist a TokenNotFoundException will be thrown
     * 
     * @key The cache key
     * @defaultValue If not found, return a default value
     *
     * @throws TokenNotFoundException
     */
    any function get( required key, defaultValue );

    /**
     * Invalidate/delete one or more keys from the storage
     *
     * @key A cache key or an array of keys to clear
     * 
     * @return JWTStorage
     */
    any function clear( required any key );

    /**
     * Clear all the keys in the storage
     *
     * @async Run in a separate thread
     * 
     * @return JWTStorage
     */
    any function clearAll( boolean async=false );

    /**
     * Retrieve all the jwt keys stored in the storage
     */
    array function keys();

    /**
     * The size of the storage
     */
    numeric function size();

}
```
{% endcode %}

