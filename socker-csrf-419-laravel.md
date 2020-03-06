- Error 419 authentication 
- Check port 443
--------------------------------add CSRF token to header----------------------

Add to boostrap.js file:
```
window.Echo = new Echo({
  broadcaster: "pusher",
  key: pusher_app_key,
  cluster: pusher_cluster,
  encrypted: pusher_encrypted,
  auth: {
    headers: {
      'X-CSRF-TOKEN': <token string here>,
    },
  },
})
```

For pusher directly same thing but different you need to do auth endpoint as well like this:

```
window.pusher = new Pusher(pusher_app_key, {
  cluster: pusher_cluster,
  forceTLS: pusher_encrypted,
  authEndpoint: '/pusher_auth.php',
  auth: {
    headers: {
      'X-CSRF-TOKEN': <token string here>,
    },
  },
})
```

Therefor you can remove '/broadcasting/auth', from middleware.


------------fix with add to middle ware---------------

You probably can fix it by adding in VerifyCsrfToken middleware:

    protected $except = [
        '/broadcasting/auth',
    ];

Also right above it I also have:

protected $addHttpCookie = true;

Ref: https://github.com/beyondcode/laravel-websockets/issues/258
