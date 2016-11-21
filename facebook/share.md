How Facebook share is working :

The objective is to allow user to customize is message from my website, and not from the Fb dialog


I want to try this : https://developers.facebook.com/docs/sharing/reference/share-dialog

It's look like a good solution because I can add a quote, wich can be compare to a description

And it's don't need facebook login or API Key

First i have to include the Facebook SDK (javacript)

I need to add this part of code :

```js
<script>
  window.fbAsyncInit = function() {
    FB.init({
      appId      : 'your-app-id',
      xfbml      : true,
      version    : 'v2.8'
    });
    FB.AppEvents.logPageView();
  };

  (function(d, s, id){
     var js, fjs = d.getElementsByTagName(s)[0];
     if (d.getElementById(id)) {return;}
     js = d.createElement(s); js.id = id;
     js.src = "//connect.facebook.net/en_US/sdk.js";
     fjs.parentNode.insertBefore(js, fjs);
   }(document, 'script', 'facebook-jssdk'));
</script>
```
In this code you have to replace `your-app-id` by the FB id of my app. I can change the language, by changing this part `en_US` in the url (`js.src`) <br>
The list of local code can be find in this xml file : https://www.facebook.com/translations/FacebookLocales.xml


From the doc of facebook I just need to add this code : <br>
```js
FB.ui({
  method: 'share',
  href: 'https://developers.facebook.com/docs/',
}, function(response){});
```

The href is the url wich i want to share, In my case it's the current url <br>
So I can do something like : `href: window.location.href,` <br>

If i want to add a quote : <br>
`quote: 'Hello world'`

So my Js file look like that :
```js
$(document).ready(function() {
  $('#vote_with_fb').click(function() {
    FB.ui({
      method: 'share',
      href: `http://winwin0932.dev-capsens.com/projects/projet-de-test`,
      quote: `Hello world`
      // href: window.location.href,
    }, function(response){});
  });
});
```

If the `response` from google is an empty array It mean that the share was ok !
