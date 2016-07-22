Progressive Web Apps

Ada Rose Edwards - Developer, Financial Times

[[TOC]]

# So what are Web Apps?

 A web app is a website; A website so good users will be unable to tell it is not a first class citizen on the device. The website has got to be so nice that the user wants to save it to their home screen where it will have an icon like a native app.

An ideal web app, should have the best aspects of the web and a native app. It should be app-like in that it is fast and quick to interact with the user. It perfectly fits the device’s viewport regardless of device orientation; it should be usable when the device is offline and be able to have an icon on the homescreen. In the same breath it must not sacrifice the things which make the web great such as the ability to deeply link into the app and use urls to enable sharing of content. Like the web it should work well across platforms, it should not focus solely on mobile; it should behave just as well on a Desktop computer as on mobile. Lest we risk having another era of unresponsive m.example.com websites.

Progressive Web Apps are not new. The Financial Times chose to use a Web App for digital content delivery on mobile devices since 2012. (https://app.ft.com)

Moving to a Web App enabled the same app to ship across platforms using a single distribution channel. With a single build we supported:

* iOS

* Android (4.4+) Chrome

* Old Android (via wrapper)

* Windows 8

* Blackberry (RIP)

* Firefox OS (RIP)

That truly is build once deploy anywhere.

# "But it’s not in an App Store"

Some people get concerned with how the app will get exposure if not in the app store.

![image alt text](image_0.png)

Source: [http://dazeend.org/2015/01/the-shape-of-the-app-store/](http://dazeend.org/2015/01/the-shape-of-the-app-store/)

Are you in the top 0.1% of apps in the App Store? No, good luck getting that exposure then.


Web apps allow you to improve engagement by reducing the number of clicks required to re-engage the user between landing on your website and engaging with your app.

By having the user ‘install’ your web app by adding it to their homescreen they can continue engaging with your site. When they close down the desktop the phone will show them where the web app is installed. Bringing you back to the forefront of their mind.

[Insert video https://ada.is/progressive-web-apps-talk/images/save-to-homescreen.mp4]

# Background and Current Climate

Since 2011 (2013 on Chrome Android) mobile browsers have had the ability to bookmark a website to your phone’s home screen. Meta tags in the head would determine the appearance of the installed web page. This is the origins of Web Apps.

Chrome 38 introduced the Web App manifest, which is a JSON file that describes the installed app configuration of your Web App. Allowing us to remove the configuration out of the head.

In Chrome 40 (December 2014) Service Workers started to be rolled out across Firefox and Chrome, nothing on Safari at the time of writing. Service workers simplify bringing the app offline and lay the foundation for future app-like features such as push notifications and background sync.

Building in app based on the new Service Workers and Web App manifest became known as a *Progressive Web Apps*.

*Progressive Web Apps* is not a spec, it started as a definition of what a Web App should be in the era of service workers given the new technology being built into browsers with Web Apps in mind. Chrome uses this definition to trigger an install prompt in the browser when a number of conditions are fulfilled. The conditions are:

* has a service worker (requires https)

* has a web app manifest (with at least minimal config and is *display: "standalone"*)

* it is the second distinct visit to the web site.

In this case progressive means it progressively becomes more app-like the more features the device supports.

The prompt is currently shown under varying conditions across Opera, Chrome and the Samsung Browser.

iOS has indicated interest in progressive web apps but at the time of writing still relies on metatags for web app configuration (and appcache for offline) .

# At what point does a website become a Web App?

## A progressive web app should exhibit certain app-like properties:

📱💻 - Responsive - Perfectly filling the screen, These sites are primarily aimed at mobile and tablets and will need to respond to the plethora of screen sizes. They should also just work as desktop websites.

✈ - Offline first - The app must be capable of starting offline and still display useful information.

👉 - Touch capable - an interface designed for touch with Gesture interaction. User interaction must feel responsive and snappy. No delays between a touch and reaction.

🐵 - It provides metadata to tell the browser how it should look when installed so you get a nice high resolution icon on the homescreen and a splash screen on some platforms.

🔔 - Push Notifications (only if required) - The ability to receive notifications when the app is not running.

## But still maintain certain web-like properties:

**e **🍏 - Progressive - It's ability to be installed is a progressive enhancement. It needs to still work as a normal website, especially on platforms which may not support installing or Service Workers yet.

https:// - On the open web - Not locked into any browser or app store. It should be able to be deep linked to and should provide methods of sharing the current url.

# Taking your website offline

Taking your website offline gives two main advantages, it works when the user is on a flakey network connection.

The time from opening the app and using the app is greatly reduced if it is not reliant on the network. This provides a great experience. A carefully optimised web app can start faster than native if the browser has been used recently.

There are two methods of getting a site to work offline:

Old and busted - Support for starting your website offline has been around for years in the form of AppCache. AppCache has some serious flaws though and has even been [deprecated from spec](https://github.com/w3c/html/issues/40). It is difficult to work with and can risk permanently breaking your websites if misconfigured. Still, it is the only way to do offline on iOS, at least until Apple make a move to support Service Workers.

New Hotness - Service workers, currently supported in Chrome, Firefox, Opera coming very soon to Edge. Apple’s Webkit team has it marked ‘Under consideration’.

It is a progammable proxy which sits between the user’s tab and the wider internet. It can intercept and rewrite or fabricate network requests to allow very granular caching, offline support. Because it is not tied to any tab, it can be brought to life in the background to handle push notifications or background sync. Also it is impossible to permanently break your website with it since it will automatically update when a new service worker script is detected.

If you are building a new website from scratch start off with a Service Worker, if your website already works offline with AppCache then you can use the tool [sw-appcache-behavior](https://github.com/GoogleChrome/sw-helpers) to generate a service worker from this as soon we may be in a state where some browsers will only accept service workers and some will only accept app cache..

# Setting up a Service Worker

Also see: [Setting up a service worker](https://www.google.com/url?q=https://www.smashingmagazine.com/2016/02/making-a-service-worker/&sa=D&ust=1468180224105000&usg=AFQjCNGzNkKFX0iNjGmqWcACXFZYXmBiHg) for more detailed instructions.

A Service Worker is a special type of Shared WebWorker. Being a worker it runs in a seperate thread to your main page. It is shared by all web pages in the same path as the service worker. E.g. A service worker located at /my-page/sw.js will be able to effect /my-page/index.html and my-page/images/header.jpg but not /index.html

Service workers are able to intercept and rewrite or spoof all network requests made on the page including those made to data:// urls!!

This power allows it to provide cached responses to get pages to work when there is no data connection but it is flexible enough to allow for many possible use cases.

Being so powerful it is only allowed in [secure contexts](https://developer.mozilla.org/en-US/docs/Web/Security/Secure_Contexts) (i.e. https). This prevents third parties from permanently overriding your site using an injected service worker from an infected or malicious wifi access point.

Setting up https nowadays may seem daunting and expensive but it has never been easier or cheaper. [Let's Encrypt](https://letsencrypt.org/) provides free SSL certificates and scripts to automatically configure your server. If you host on github, github pages are automatically served over https. Tumblr pages can be configured to run on https, too. Cloudflare can also proxy requests to upgrade to https.

Offlining usually involves picking certain caching methods for different parts of your site to allow them to be served faster or when there is no connection. I will discuss various caching methods.


To abstract away complex caching logic I use the tool [sw-toolbox](https://github.com/GoogleChrome/sw-toolbox). This library can be set to handle the routing by providing four preconfigured routes which can be configured in a clean fashion. It can be imported into your service worker

### importScripts('scripts/sw-toolbox.js');

###
Usecase 1. Precaching

Precaching pulls down requests before your site works out they are needed. This can greatly increase first paint time because your site doesn’t need to parse '/site.css' before it starts downloading your websites logo '/images/logo.png'.


toolbox.precache(['/index.html', '/site.css', '/images/logo.png']);

### Usecase 2. Offline

Allow users to revisit your site when the user is offline in the simplest case this means falling back to the cache if the device is offline. It is important to set a timeout here because a flakey network, a misconfigured router or a captive portal could have the user waiting indefinitely.

toolbox.router.default = toolbox.networkFirst;

toolbox.options.networkTimeoutSeconds = 5;

In reality we can actually be a little smarter. This is because the majority of your assets will not change over time. So we probably want to just get it as soon as possible. Whether from cache or network. This line tells sw-toolbox that all requests to the images path should come from cache if they are available.

toolbox.router.all('/images/*', toolbox.fastest);

Although it is important that when the user is authenticating we aren’t just returning a cached response. So this we will state should be network only.

toolbox.router.post('/auth/*', toolbox.networkOnly);


This example provides examples of some good practises for offlining:

* Initial static assets are precached, this downloads them and caches them when the Service Worker is installed. This means that they do not need to be loaded from the server when they are eventually required.

* By default requests will try to be freshly from the network but will fallback to the cache so they are available offline.

* A relatively short network timeout means that requests will even complete with cached data on a network connection which lies about being on the internet.

* Infrequently updated assets such as images will be dispatched from cache first then and the browser will also try to update them. If toolbox.cacheOnly was used then this could also save the user’s data.

**NB! **The browser cache and the Cache API are different. Just because the Cache API has been bypassed in the case of network first or network only. The request may still hit the browser cache because the caching headers on the request say it is still valid. Problems with this may result in user’s receiving a mixture of cached and fresh data. Jake Archibald  has some goodsuggestions for avoiding this issue here: [https://jakearchibald.com/2016/caching-best-practices/](https://jakearchibald.com/2016/caching-best-practices/)

## Debugging your service worker:

if you are in Chrome go to: chrome://serviceworker-internals. This will allow you to inspect and pause and uninstall your service worker script.

# Interaction and Animation Performance

Users of the web have to expect that the web does not have smoothly animated interface the way native apps do. This is not acceptable for web apps.  Web apps must animate smoothly less our users feel we are delivering a degraded, second class experience. We have three goals in order to ‘feel’ fast:

* When the user does something the app must do something within 100ms otherwise the user will notice. This counts for taps, clicks, drags and scrolls.

* Each frame must render at a consistent 60fps (16ms per frame), even only a few slow frames is very noticeable.

* Must be fast on a three year old budget phone on a poor network as well as well as your shiny development machine.

* Must start fast: Long has the web been focused on maintaining user engagement  by getting a visible and usable site in under 1-2s. Here start up time is as important as ever. If all the app content is cached and the browser is still in the devices memory a webapp can start faster than a native app. One mistake made by native and web alike is requiring networked content to work.

* Must be small to download and update - 10MB from an app store don’t feel like much, but 10 uncached MB downloaded every time are very much  impossible to reach for a lot of mobile users

Off the bat the most essential item is this in the head of the document:

<meta name="viewport" content="width=device-width">

This line will ensure that there is no 300ms tap delay on phones Chromium based and Firefox browsers but still allows the user to zoom in by pinch zooming (great for accessibility.)

Since iOS 8 clicks were fast by default but may be slow if the tap was fast according to [certain heuristics](http://developer.telerik.com/featured/300-ms-click-delay-ios-8/). If this is an issue for you I recommend using [FastClick](https://github.com/ftlabs/fastclick) to remove the delay.

There is also the issue of animation performance. You probably want to have lots of pretty animating in and out elements, elements which can be dragged by the user or other lovely interactions.

I could and have talked for hours on making web page animations fast. There are a myriad of ways to introduce slow moments to your website but I have a page limit here so instead I will give advice for avoiding jank from the start.

Dig around or ask around your friends or family for an old smartphone, I borrow my partners Nexus 4.

Plug it into your computer and go to chrome://inspect/#devices this will open an inspection window you can use to inspect the web page running on the phone. You can use profiling and view the timeline to find sources of poor performance and optimise them based on a real baseline device.

Animating certain css properties is one of the biggest cause of jank to [CSS Triggers](https://csstriggers.com/) is great resource for determining what properties can be safely animated without causing the browser to repaint or re-layout the whole page.

If writing performant animations is a daunting task there are very many libraries out there which handle high performance animations for you. A favourite of mine is [Greensock](https://greensock.com/) which as well as doing very fancy animations and tweening it can also handle touch interactions such as dragging items very nicely.

# Push Notifications

Push notifications are a great way to re-engage with your users. By prompting the user you bring your app to the forefront of mind. They can finish off an unfinished transaction or receive alerts about relevant new content.

Make them relevant for synchronous and timely for events which *require* user action. E.g. Reply to person, go to event. Also don’t make a push notification if your web app is already visible or focused.

Make sure your notification goes to a page which works offline. Notifications will hang around unread. They may be interacted with when the user has no network connection. Refusing to work when they have tried to interact with you will anger your user.

*The very best experience for push notifications allows the user to not open your web app at all! *

Allow the user to NOT open the web app. 'You have a new message' is useless. It is as annoying as a clickbait headline. Display the message and the sender.

Action buttons on the notification allow you to provide interaction prompts which may not open the browser. E.g. ‘like this post’, ‘reply with yes’, ‘reply with no’, ‘remind me later’. These will provide a positive engagement experience on the user’s terms to keep them engaged with minimal time investment on their part.

If you spam the user with regular or irrelevant notifications. They may disable notifications for your app, in the browser! Then it is impossible to re-engage and you can't prompt for permission again!

To avoid this have a clear and easy to get to 'disable notification' button. Once you have addressed the issues frustrating your users you can then try to re-engage.

The Push Notification API requires a Service Worker. As it is possible to receive Push Notifications when no browser tabs are open. The Service Worker will handle the notification request in a background thread. It can perform async operations such as making a fetch requests to your API before displaying the notification to the user.

To make an API request make a request to an endpoint provided by the browser manufacturer. For Chromium based browsers (Opera, Samsung and Chrome) it is "Firebase Cloud Messaging". These browsers also behave a little off spec as well.

One can reveal the details of this by requesting push notification permission.

<pre><code class="language-javascript">
	serviceWorkerRegistration

	.pushManager
	.subscribe({
		// Required parameter as receiving updates but not displaying a message is not supported everywhere.
		userVisibleOnly: true
	})
	.then(function(subscription) {
		return sendSubscriptionToServer(subscription);
	})

	The subscription is an object that looks like:

	{
		"endpoint": "http://example.com/some/uuid"
	}
</code></pre>

The uuid identifies the browser you are currently using.

**NB:** Chromium based browsers behave a little differently. You need a Firebase app ID, this needs to be entered into your Web App Manifest e.g. "gcm_sender_id": "90157766285"

Also the endpoint does not work in the format it is given. Your server needs to mangle it slightly to get it to work. It needs to be a POST request with your api key in the head and the uuid in the body. The details are here: [https://developers.google.com/web/fundamentals/getting-started/push-notifications/step-07?hl=en](https://developers.google.com/web/fundamentals/getting-started/push-notifications/step-07?hl=en)

When one is sending push notifications it is possible to send data in the body of the push notification itself. This is very complex and involves encrypting the content into the api request. The [‘web-push’ package for node](https://www.npmjs.com/package/web-push) can handle this but it is not what I prefer to do.

One can perform async requests once the notification has been received so I prefer to send a minimal notification, known as a ‘tickle’, to the client and then the service worker will make a fetch request to my API to pull down any recent updates.

**NB:** The service worker can be closed by the browser at any point. The event.waitUntil function in the push event will tell the service worker to not close until our event is finished.

<pre><code class="language-javascript">
self.addEventListener('push', function(event) {
	**event.waitUntil**(

// Makes api request, returns Promise
	getMessageDetails()
	.then(function (details) {
		self.registration.showNotification(

details.title, {
		body: details.message,
		icon: '/my-app-logo-192x192.png'
	})

);
})

);

});
</code></pre>



You can listen for click/press on the notification events too. Use these to decide how to react. You can open new browser tabs, focus an existing tab or make api requests. You also choose whether to close the notification or keep it open. Use this functionality with actions on the notification to build great functionality into the notification itself. Rather than requiring the user to have to open your app each time. This will be a great experience.

# Don’t ignore the strengths of the Web

My final and most important point is, in our rush for an app like experience we should not forget to stay web like in a very important aspect: URLs.

An installed web app often will chose to hide the browser shell. So there will be no address bar for the user to share their current url nor can they save current page for later.

Having URLs is a unique web advantage, you can get users to use your app via click rather than describing how to navigate an app. But it is very easy to forget to expose this to the users. You could write the best app in the world but you have done yourself a major disservice if no one can share it.

Make sure you provide ways to share your app either via share buttons or a button to expose the url.


<h4>Final Notes</h4>

Other writings on PWA:

<a href="https://ada.is/blog/2016/06/01/yet-another-progressive-webapp-post/">https://ada.is/blog/2016/06/01/yet-another-progressive-webapp-post/</a>

Links referenced in the article:

<ul>
	<li><a href=http://dazeend.org/2015/01/the-shape-of-the-app-store/>http://dazeend.org/2015/01/the-shape-of-the-app-store/</a></li>
	<li><a href=https://github.com/GoogleChrome/sw-helpers>https://github.com/GoogleChrome/sw-helpers</a></li>
	<li><a href=https://github.com/GoogleChrome/sw-toolbox>https://github.com/GoogleChrome/sw-toolbox</a></li>
	<li><a href=http://developer.telerik.com/featured/300-ms-click-delay-ios-8/>http://developer.telerik.com/featured/300-ms-click-delay-ios-8/</a></li>
	<li><a href=https://jakearchibald.com/2016/caching-best-practices/>https://jakearchibald.com/2016/caching-best-practices/</a></li>
</ul>

<h3>Author's Biography</h3>

<p>Ada is a Developer Advocate for Samsung and a previous member of FT Labs.</p>

<p><a href="https://twitter.com/lady_ada_king">@lady_ada_king</a></p>
<p><a href="https://ada.is">Website</a></p>