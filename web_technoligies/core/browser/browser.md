# links 
	- [history api](https://developer.mozilla.org/en-US/docs/Web/API/History)
	- [history tutorial](http://diveintohtml5.info/history.html)
	- [interfaces](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API#inerfaces)
	- [onpopstate event](https://developer.mozilla.org/en-US/docs/Web/API/Window/onpopstate)

	


# DOM
# next up
  - [mutation observer](https://developer.mozilla.org/en-US/docs/Web/API/MutationObserver)
  -

# DOM
  - When the browser loads the page, it transforms your HTML into a live document
    1. parses html (strings of text) into a data model (objects and nodes)
    2. preserves the HTML hierarchy by creating a tree of nodes (the DOM)



# window
## history 
	- enables you to use scripts to modify the URL (and related context) without triggering a page refresh
		- i.e. manipulation of the browser session history
		- i.e. pages visited in the tab/frame that the current page is loaded in

```js
	// navigation
		history.back()
			// previous page in sessino history 
		history.forward()
			// next page in session history
		history.go(-1)
			// @see history.back()
		history.go(1)
			// @see history.forward()
	// manipulation 
		// neither causes browser refreshes or ajax requests
		// eveyrthing you want to happen on state changes 
		// must be handled programmaticaly
		history.state
			// whenever the user navigates to a new state
			// a `popstate` event is fired and contains the state object 
			// see `pushState` and `replaceState`
			// if the browser restarts it MAY contain the state object 
			// associated with the last `pushState` or `replaceState()` methods 

		history.pushState({state}, 'title', 'url')
			// pushes the give data ont he session history stack
			// changes the referrer thats gets used in the HTTP header for ajax requests
			// the referrer will be the url of hte doc whose window is `this` at the time when the ajax object is created
			// calling history.back() after calling history.pushState() does not fire a `popstate` event
			// never fires a `hashchage` event 
		history.replaceState({state}, 'title', 'url')
			// exactly the same as `pushState` except replaces the current history entry 
			// useful when you want to update the stateObject/url of the current 
			// history entry in response to user action

	// events 
		window.onpopstate
			// fired everytime the active history entry changes 
			// contains the state object associated with `replaceState()` and `pushState()` history methods 

```
	
```




## URL 

```js 
	let params = (new URL(document.location)).searchParams;
	let name = params.get('name'); // is the string "Jonathan Smith".
	let age = parseInt(params.get('age')); // is the number 18
i
```

# workers 
	- [using service workers](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API/Using_Service_Workers)
	- [using web workers](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers)
	- [basic code example](https://github.com/mdn/sw-test)
	- [concepts and usage](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API#Service_worker_concepts_and_usage)
	- [google workbox](https://github.com/GoogleChrome/workbox)
	- [maybe the same thing?](https://serviceworke.rs/)
	- [mdn service worker API docs](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
	- [mdn service worker cookbook](https://github.com/mozilla/serviceworker-cookbook)
	- [Notification object docs](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerRegistration/showNotification#Parameters)
	- [notifications api](https://developer.mozilla.org/en-US/docs/Web/API/Notifications_API)
	- [push notifications](https://developers.google.com/web/ilt/pwa/introduction-to-push-notifications)
	- [service worker api](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
	- [service worker cache for responses](https://developer.mozilla.org/en-US/docs/Web/API/Cache)
	- [sw example](https://github.com/mdn/sw-test)
	- [understaanding service workers](http://blog.88mph.io/2017/07/28/understanding-service-workers/)
	- [using web workers](https://developer.mozilla.org/en-US/docs/Web/Guide/Performance/Using_web_workers)
	- [VAPID docs](https://tools.ietf.org/html/rfc8292)
	- [workbox window tut](https://medium.com/@webmaxru/workbox-4-implementing-refresh-to-update-version-flow-using-the-workbox-window-module-41284967e79c)


## terminology
	- web workers
		- run scripts in background threads
		- send/receive messages to the calling code
	- worker thread
		- perfeorm
		  - tasks without interfering with the UI
		  - I/O using XMLHttpRequest
		  -
	- service workers
		- act as proxy servers that sit between web applications, the browser, and the network
		- an event-driven worker registered against an origin and a path
		-  a JavaScript file that can control the web-page/site that it is associated with, intercepting and modifying navigation and resource requests, and caching resources in a very granular fashion
		-  only support HTTPS
		-  firefox: cant be used in private browsing mode
	-  worker context
		-  a service workers execution environment (e.g. browser, node, worker are all distinct contexts)
		- fully async
		  - cant be used with synchronous APIs like XHR and localStorage
		- no DOM access
		- runs a different thread than  to the main javascript browser context


## service workers 
### use cases
  - act as a middleware (opportunities are limitless)
  - enable the creation of offline experiences
  - intercept network request
  - react based on network availability
  - background data syncrhonization
  - responding to resource requests from other origins
  - receiving centralized updates to expensive to calculate data
  - client-side compiling and dependency management
  - hooks for background services
  - custom templating based on URL patterns
  - performance enhancments
    - prefecting resources, etc


### lifecycle
	1. registration
		- e.g. `workbox.register()`
		- e.g. `navigator.serviceWorker.register('./sw-test/sw.js', {scope: './sw-test/'})`
			- `register('./path/to/sw.js', {scope: 'relative/to/origin'})
			- in the above example, `sw-test` is the origin
		- A single service worker can control many pages. 
		- Each time a page within your scope is loaded, the service worker is installed against that page and operates on it.
		- be careful with global variables in the service worker script: each page doesn’t get its own unique worker.
	2. download
	3. install `oninstall` event
		- triggers
		  - when the downloaded file is found to be new/byte-wise different
		- use cases 
		  - populte idb 
		  - fetch resources to cache site assets
		  - i.e. whatever you would do when initialling a mobile app 
		    - e.g. make sure its available offline 
		  - etc
		- installation is not complete until `oninstall` handler completes
	4. activate `onactivate` event
		- triggers
		  - new installation - immediately activated
		  - update installation - activated when there are no longer any pages loaded that are still using the old service worker
		    - force activation 
		      - `ServiceWorkerGlobalScope.skipWaiting()` dont wait for existing workers to release claim
		- use cases 
		  - cleanup of resources used in previous service worker versions
		    - e.g. deleting stale idb caches

	5. update
		- triggers
		  - navigation to an in-scope page occurs
		  - event fired on the service worker and it hasnt been downloaded in the past 24 hrs

	6. claiming/controlling of pages 
		- service worker only controls pages opened after the `register()` is successful 
		- scope: 
		  - The path to your service worker file needs to be written relative to the origin, not your app’s root directory
		    - if the worker is at https://mdn.github.io/sw-test/sw.js, 
		    - and the app’s root is https://mdn.github.io/sw-test/.
		    - the path needs to be written as /sw-test/sw.js, not /sw.js.
		  - Each time a page within your scope is loaded, the service worker is installed against that page and operates on it.
		  - each page SHARES the same worker context, watchout for globals
		  - The service worker will only catch requests from clients under the service worker's scope.
		  - The max scope for a service worker is the location of the worker.
		  - If your service worker is active on a client being served with the Service-Worker-Allowed header, you can specify a list of max scopes for that worker.
		- `Clients.claim()` - force new worker to claim existing pages



### steal some shit 
 	- [they check for localhost](https://github.com/DennyScott/react-router-auth/blob/master/src/serviceWorker.js)

### tools 
  - [chrome: service workers](chrome://inspect/#service-workers)
  - [more information than inspect/#service-workers](chrome://serviceworker-internals)

## API
### ServiceWorker
  - Represents a service worker.
  - Multiple browsing contexts (e.g. pages, workers, etc.) can be associated with the same ServiceWorker object.
  -
```js
	// events self.addEventListener('EVENT_NAME', (event) => {})
		'install'
			// service worker was installed
			// good place to poopulate idb for offline functionality
		 
i
```

### Navigator.serviceWorker
  - Returns a ServiceWorkerContainer object,
    - provides access to the ServiceWorker objects for the associated document.
      - registration
      - removal
      - upgrade
      - communication
      - this occurs in a browser context (i.e. one of your application js files)
      	- wherever you call `register()`


### ServiceWorkerRegistration
  - represents a service worker registration

### ServiceWorkerState
  - state of the service worker


### ServiceWorkerContainer
  - Provides facilities to register, unregister, and update service workers, and access the state of service workers and their registrations.
  - i.e. the file js file that calls `navigator.serviceWorker.register()`


#### [register()](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerContainer/register)
  - service worker will be downloaded to the client and installation/activiation will be attemped

### ServiceWorkerGlobalScope
  - Represents the global execution context of a service worker.


### SyncManager
  - provides an interface for registering  and listing sync registrations


#### [skipWaiting()](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/skipWaiting)
  - when a serviceWorker is installed over an existing one
    - the new serviceWorker will be activated immediately
      - usually it waits for the old serviceWorker to not be used by an existing loaded pages

### Client
  - the scope of a client controlled by a service worker
  - either
    - a document in a browser context
    - a SharedWorker

##### WindowClient
  - special type of `Client`
  - scope of a service worker client that is a document in a browser context controlled by the active service worker


### Clients
 - represnets a container for a list of Client objects
 - the mainway to access all the clients owned by the active service worker
#### [Claim()](https://developer.mozilla.org/en-US/docs/Web/API/Clients/claim)
  - when a serviceWorker is installad over an existing one
    - the serviceWorker can claim all pages owned by the previous service worker
    - usually it waits to claim pages until the old serviceWorker is not used by any existing loaded pages



### cache
  - just use IDB - fuck cache
  - represents the storage for request/response object pairs that are cached as part of the serviceworker lifecycle
  - global object on the service worker that allows us to store assets delivered by responses, and keyed by their requests
  - works in a similar way to the browser’s standard cache, but it is specific to your domain


### CacheStorage
  - the storage for cache objects
  - provides
    - a master directory of all the named caches that a ServiceWorker can access
    - maintains a mapping of string names to corresponding Cache objects

### Events
#### [MessageEvent](https://developer.mozilla.org/en-US/docs/Web/API/MessageEvent)
  - used by service workers


#### FetchEvent
  - The param passed into the ServiceWorkerGlobalScope.onfetch handler
  - a fetch action that is dispatched on the `ServiceWorkerGlobalScope` of a `ServiceWorker`
  - contains info about the request and resulting response

##### respondWith()
  - provide an arbitary response back to the controlled page


#### InstallEvent

#### SyncEvent
  - a sync action that is dispatched on the `ServiceWorkerGlobalScope`


## TODO
### ExtendableEvent
### ExtendableMessageEvent
### NavigationPreloadManager
### NotificationEvent

# examples
```js
  // InstallEvent https://developer.mozilla.org/en-US/docs/Web/API/InstallEvent
  // prepare your service worker for usage when this event fires
  // e.g. create a cache (builtin storage API) and place assets inside that youll want for running yoru app offline

  // activate
  // a good time tro clean up old caches, etc, with the previous service worker

  // FetchEvent - https://developer.mozilla.org/en-US/docs/Web/API/FetchEvent
  // respond to requests

  // FetchEvent.respondWith
  // arbitrarily  modify the response to a FetchEvent
  // 
  // 
  
  // registration 
  if ('serviceWorker' in navigator) {
    // url is relative to the origin
    // scope specifies which paths is controlled by this service worker
    navigator.serviceWorker.register('./sw-test/sw.js', {scope: './sw-test/'})
    .then((reg) => {
      // registration worked
      console.log('Registration succeeded. Scope is ' + reg.scope);
    }).catch((error) => {
      // registration failed
      console.log('Registration failed with ' + error);
    });
  }
i
```


## push notifications 
### TODO 
	- [PushSubscription](https://developer.mozilla.org/en-US/docs/Web/API/PushSubscription)
	- [pushMessageData](https://developer.mozilla.org/en-US/docs/Web/API/PushMessageData)
		- the event sent when a service worker receives a push 
	- [pushmanager](https://developer.mozilla.org/en-US/docs/Web/API/PushManager)
	- [workbox](https://developers.google.com/web/tools/workbox)
	- [workbox with webpack](https://developers.google.com/web/tools/workbox/modules/workbox-webpack-plugin)
	- [workbox with webpack specific](https://developers.google.com/web/tools/workbox/guides/precache-files/webpack)
	- [always use this one cuz your dope](https://developers.google.com/web/tools/workbox/modules/workbox-webpack-plugin#injectmanifest_plugin_2)


### terms
	- push message
		- a message sent from the server to the client 
	- push notification 
		- a notification created in response to a push message 
		- object displayed in the status bar of a client, e.g. smartphone/browser 
	- notifications API 
		- an interface used to configure and display notifications to the user 
	- push api 
		- an interface used to subscribe your app to a push service and receive push messages in the service worker 
	- web push 
		- an informal term referring to the process or components involved in the process of pushing messages from a server to a client on the web
	- push service 
		- a system for routing push messaages form a server to a client 
		- each browser implements its own push service 
	- web push protocol 
		- describes how an application server or user agent interacts with a push service 
	- subscription
		- created by a service worker
		- creates a unique endpoint that receives pushes from a backend server and proxies to the service worker that created it

### flow 
#### setup
	1.	user is asked for and provides consent to receive notifications from your application 
	2.	service worker registration workflow completes 
	3.	service worker creates a push notification subscription and sends the endpoint it receives to your backend
	4.	your backend saves the subscription endpoint for later use to push messages back to the service worker

#### pushing: backend -> frontend 
	1.	backend sends a message to a saved subscription 
	2.	service worker receives message and sends to app thread 
	3.	app displays notification to user 
	4.	BOOM

#### key decision points 
	- 	ask user for permission 
		- 	user can press X (DENIED)
		- 	user can deny (DENIED)
		- 	user can grant (SUCCESS)

