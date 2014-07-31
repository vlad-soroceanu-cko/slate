---
title: CheckoutAnalyticsJS API Reference

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

search: true
---

# Quick description

CheckoutAnalytics provides methods to track a merchant's costumer's behaviour and provide insight into the robustness of the payment UI we provide to the merchant. It can track events and timing around the pay button, card form etc. 

# Setting up the Script Tag

CheckoutAnalytics connects to Google Analytics and sets up a tracker by providing the UA of the merchant. In order to obtain the UA and the costumer ID you need to provide the API with a publishableKey. The easiest way to do that is to provide it directly in the script tag.

## PublishableKey

The minimum you can provide is the publishableKey. Without this one the app will not run. It can be set later, but until this is provided the app will not run, although will still track HTML elements and build a stack that will be run later when the app is initialized.

> Setting the publishableKey:

```html
<script id="cko_script_tag" type="text/javascript"
    src="/js/CheckoutAnalytics.js"
    data-publishable-key="{{pk}}">
</script>
```

## UA

The UA is the ID of the merchant's channel that Google Analytics needs so that it can store data. This can also be provided in the script tag, given that you already have it, otherwise CheckoutAnalytics will attempt to take it from the DB using the publishableKey.

> Setting the UA:

```html
<script id="cko_script_tag" type="text/javascript"
    src="/js/CheckoutAnalytics.js"
    data-publishable-key="{[pk_00000000000000000]}"
    data-ua="{[UA-XXXXXXX-X]}">
</script>
```

## Customer Data

If the costumer has an entry already created for him in the DB, we can get that in the script tag. The unique indentifiable resource for the customer account is his/her email. Other data that can be passed are customer phone and customer name. The latter is highly optional.

> Setting the costumer data:

```html
<script id="cko_script_tag" type="text/javascript"
    src="/js/CheckoutAnalytics.js"
    data-publishable-key="{[pk_00000000000000000]}"
    data-ua="{[UA-XXXXXXX-X]}"
    data-user-email="john.doe@email.com"
    data-user-phone="07999999999"
    data-user-name="John Doe">
</script>
```

# JavaScript Methods

## setPublishableKey

###

Parameter | Type | Default | Description
--------- | ---- | ------- | -----------
pk | String | undefined | this is the unique merchant's channel publishableKey

```javascript
CheckoutAnalytics.setPublishableKey([pk]);
```

## trackElement

This method will track an elements click or touch events and send data to analytics every time an interaction is detected.

###

Parameter | type | Default | Description
--------- | ---- | ------- | -----------
el | HTML Element | null | the element analytics will be tracking
category | String | 'HTML Element' | a string that will go under category in the analytics dashboard
action | String | 'click' | a string that will go under action in the analytics dashboard
label | String | 'element.id' | a string that will go under label in the analytics dashboard
value | String | undefined | a string that will go under value in the analytics dashboard
opts | Object | null | an object containing specific settings for the Google Analytics 'event' event

```javascript
CheckoutAnalytics.trackElement(element, category, action, label, value, opts);
```
<aside class="success">
This method can be used even if the app hasn't yet been initialized. It will stack all the commands and run them later.
</aside>

## trackEvent

Send an event log to the Google Analytics.

###

Parameter | type | Default | Description
--------- | ---- | ------- | -----------
category | String | 'HTML Element' | a string that will go under category in the analytics dashboard
action | String | 'click' | a string that will go under action in the analytics dashboard
label | String | 'element.id' | a string that will go under label in the analytics dashboard
value | String | undefined | a string that will go under value in the analytics dashboard
opts | Object | null | an object containing specific settings for the Google Analytics 'event' event

```javascript
CheckoutAnalytics.trackEvent(category, action, label, value, opts);
```
<aside class="success">
This method can be used even if the app hasn't yet been initialized. It will stack all the commands and run them later.
</aside>

## trackPage

Send a pageview to Google Analytics.

###

Parameter | type | Default | Description
--------- | ---- | ------- | -----------
opts | Object | null | an object containing specific settings for the Google Analytics 'event' event

```javascript
CheckoutAnalytics.trackPage(category, action, label, value, opts);
```
<aside class="success">
This method can be used even if the app hasn't yet been initialized. It will stack all the commands and run them later.
</aside>


## trackException

Send an event log to the Google Analytics.

###

Parameter | type | Default | Description
--------- | ---- | ------- | -----------
desc | String | 'unknown error' | a string representing the description of the exception
fatal | Boolean | false | a boolean value that says if the exception was fatal to your program or not
opts | Object | null | an object containing specific settings for the Google Analytics 'event' event

```javascript
CheckoutAnalytics.trackException(desc, fatal, opts);
```
<aside class="success">
This method can be used even if the app hasn't yet been initialized. It will stack all the commands and run them later.
</aside>


## scanDOM

Scans the DOM and executes any tracking that was set up on HTML elements' attributes. See DOM Scanning for more info on how to set it up in HTML.

###

Parameter | type | Default | Description
--------- | ---- | ------- | -----------
cls | String | undefined | a string representing the class by which you want to search for elements in the DOM

```javascript
CheckoutAnalytics.scanDOM(cls);
```
<aside class="success">
See DOM Scanning for more info on how to set it up in HTML.
</aside>
<aside class="warning">
This method can be used even if the app hasn't yet been initialized. It will stack all the commands and run them later.
</aside>

## decorateURL

In order for cross-domain tracking to work in an iframe you need to decorate the url of the iframe with the cookie already set in Google Analytics object on the parent domain. This method will decorate an URL with the Google Analytics cookie.

###

Parameter | type | Default | Description
--------- | ---- | ------- | -----------
url | String | undefined | the URL that you want to decorate
return | String | undefined | the decorated URL containing the Google Analytics cookie


```javascript
CheckoutAnalytics.decorateURL(url);
```
<aside class="warning">
This method will only return a valid result after CheckoutAnalytics has been properly initialized.
</aside>

## ready

Once CheckoutAnalytics is loaded will start the process of looking for the UA and creating the Google Analytics object. Until it has the UA and the object has been properly created CheckoutAnalytics can't send any data to Google Analytics, but will stack an command it receives and run all of them, preserving the sequence in which they came, once everything is set up. At that moment the callback provided in the ready method will be called letting the developer know he can use the API.

###

Parameter | type | Default | Description
--------- | ---- | ------- | -----------
callback | function | null | the callback you want to be called once CheckoutAnalytics is initialized and ready to be used


```javascript
CheckoutAnalytics.ready(callback);
```

# DOM Scanning

You can set element tracking directly in HTML by placing certain attributes on the HTML elements. CheckoutAnalytics will scan the DOM automatically right after it has loaded.

## Container

You can provide a CSS class to the scanDOM method, mentioned above to make it only scan a certain part of the DOM. It will scan all the the children of the elements that contain that class. So you can specify multiple contains at once by setting an empty class on all of them.

> Setting up a container:

```html
<div class="ckoAnalyticsContainer">
    <ul>
      <li class="listItem" data-cko-analytics="track">Item 1</li>
      <li class="listItem">Item 2</li>
      <li class="listItem">Item 3</li>
      <li class="listItem" data-cko-analytics="track">Item 4</li>
    </ul>
    <div class="listItem">
      <div>
        <div data-cko-analytics="track">Just an element</div>
      </div>
    </div>
  </div>
```
<aside class="warning">
We strongly recommend using a unique CSS class that has no style attached to it whatsoever. This way you will be sure you are only using it for indetifying the container and not messing up any styling.
</aside>
<aside class="warning">
Only the elements that contain the attribute 'data-cko-analytics="track"' will be tracked. All other elements, even if they are children of the container will be ignored.
</aside>


## Tracking Element

You can set up an element to be tracked (besides using the JavaScript method explained earlier) by providing it with the attribute 'data-cko-analytics="track"'. This can be further decorated with the values you would like to be sent to Google Analytics in the exact same way the JavaScript trackElement method does.

> Setting up an element:

```html
<div>
    <ul>
      <li class="listItem" data-cko-analytics="track,button,click,contact,1">Item 1</li>
      <li class="listItem">Item 2</li>
      <li class="listItem">Item 3</li>
      <li class="listItem" data-cko-analytics="track">Item 4</li>
    </ul>
    <div class="listItem">
      <div>
        <div data-cko-analytics="track">Just an element</div>
      </div>
    </div>
  </div>
```

As you can see we give it the same parameters we give to the JavaScript trackElement method, with the exception of the opts object:

Parameter | type | Default | Description
--------- | ---- | ------- | -----------
category | String | 'HTML Element' | a string that will go under category in the analytics dashboard
action | String | 'click' | a string that will go under action in the analytics dashboard
label | String | 'element.id' | a string that will go under label in the analytics dashboard
value | String | undefined | a string that will go under value in the analytics dashboard