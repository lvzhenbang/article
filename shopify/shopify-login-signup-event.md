# Login & Signup Event

Certainly! To track Shopify login and sign-up events with GA4, you can use the following code snippets:

For login event:

```
gtag('event', 'login', {
  'method': 'Shopify',
  'event_category': 'Authentication',
  'event_label': 'Login'
});
```

For sign-up event:
```
gtag('event', 'sign_up', {
  'method': 'Shopify',
  'event_category': 'Authentication',
  'event_label': 'Sign Up'
});
```

Make sure to replace 'Shopify' with the actual method used for authentication in your Shopify store.

Please note that you need to have the GA4 tracking code installed on your Shopify store for these events to be tracked correctly. Let me know if you need any further assistance!

demo:

```
if (window.location.pathname == 'account/login') window.localStorage.setItem('login_signup', 'login');
if (window.location.pathname == 'account/register') window.localStorage.setItem('login_signup', 'signup');
if (__st.cid) {
    if (window.localStorage.getItem('login_signup') == 'login') window.gtag('event', 'login');      
    if (window.localStorage.getItem('login_signup') == 'signup') window.gtag('event', 'sign_up');
    window.localStorage.removeItem('login_signup');
}
```