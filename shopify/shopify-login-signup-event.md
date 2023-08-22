# Login & Signup Event

```
if (window.location.pathname == 'account/login') window.localStorage.setItem('login_signup', 'login');
if (window.location.pathname == 'account/register') window.localStorage.setItem('login_signup', 'signup');
if (__st.cid) {
    if (window.localStorage.getItem('login_signup') == 'login') window.gtag('event', 'login');      
    if (window.localStorage.getItem('login_signup') == 'signup') window.gtag('event', 'sign_up');
    window.localStorage.removeItem('login_signup');
}
```