<img src="/assets/tik_tok_framed.png" width="300" align="right">

# [InAppBrowser-csp.netlify.app](https://InAppBrowser-csp.netlify.app)

## What is this project?

This is a small test to check how In-app browsers responds to Content Security Policies set as a meta tag in a statically hosted html file.
The idea (and code) comes from the good work of Krausefx, and you should probably read [this article](https://krausefx.com/blog/announcing-inappbrowsercom-see-what-javascript-commands-get-executed-in-an-in-app-browser) first.

## How does it work?

By setting a Content Security Policy(CSP) on a page, we can prevent injectet script, css and unwanted requests to be performed on a page.
Usually it is advised to set a policy in the header of the response, but in my case I cannot do that since the page is statically hosted.
So I put this in the head of the html:

```html
<meta
  http-equiv="Content-Security-Policy"
  content="default-src 'self'; img-src 'none'; child-src 'none'; script-src 'nonce-Ilovescript'; style-src 'nonce-Ilovecss';"
/>
```

I then add a matching `nonce` to the script and style tags on the page.

```javascript
<script type="text/javascript" nonce="Ilovescript">
  const errorList = document.getElementById('errors');

  const addToLog = (logmessage) => {
    errorList.innerHTML += logmessage;
  };

  document.addEventListener('securitypolicyviolation', (e) => {
    addToLog(`<li>${e.violatedDirective}:  ${e.blockedURI}</li>`);
  });
</script>
```

This code listens to the `securitypolicyviolation` event which is triggered whenever something violates the CSP set for the page. It then adds the violation to the html so we can see it on the page.

## How to use

Open [InAppBrowser-csp.netlify.app](https://InAppBrowser-csp.netlify.app) through the iOS/Android app of your choice. For a social media app post the link, or for messengers send the link to yourself, and try opening the page as part of their in-app web browser.
