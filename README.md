Welcome to History.js (v1.4 - Unreleased)
==================


This project is the successor of [jQuery History](http://balupton.com/projects/jquery-history), it aims to:

- Support [HTML5's History/State APIs](https://developer.mozilla.org/en/DOM/Manipulating_the_browser_history)
- Provide a backwards compatible experience for Browsers which do not support HTML5's History APIs
- Provide a backwards compatible experience for Browsers which do not support HTML4's OnHashChange
- Provide a forwards compatible experience for HTML4 States in HTML5 Browsers (see usage section for an example)
- Follow the original API's as much as possible and support attaching data and title properties to states as well as both the `pushState` and `replaceState` methods in **both** HTML4 **and** HTML5 browsers.
- Support as many javascript frameworks as possible via adapters - especially [jQuery](http://jquery.com/), [MooTools](http://mootools.net/) and [Prototype](http://www.prototypejs.org/)

Licensed under the [New BSD License](http://creativecommons.org/licenses/BSD/)
Copyright 2011 [Benjamin Arthur Lupton](http://balupton.com)


## Usage

Working with History.js:

	(function(window,undefined){

		var History = window.History; // Note: We are using a capital H instead of a lower h

		History.Adapter.bind(window,'statechange',function(){ // Note: We are using statechange instead of popstate
			var State = History.getState(); // Note: We are using History.getState() instead of event.state
			History.log(State.data, State.title, State.url);
		});

		History.pushState({state:1}, "State 1", "?state=1"); // logs {state:1}, "State 1", "?state=1"
		History.pushState({state:2}, "State 2", "?state=2"); // logs {state:2}, "State 2", "?state=2"
		History.replaceState({state:3}, "State 3", "?state=3"); // logs {state:3}, "State 3", "?state=3"
		History.pushState(null, null, "?state=4"); // logs {}, '', "?state=4"
		History.back(); // logs {state:3}, "State 3", "?state=3"
		History.back(); // logs {state:1}, "State 1", "?state=1"
		History.back(); // logs {}, "Home Page", "?"
		History.go(2); // logs {state:3}, "State 3", "?state=3"

	})(window);

So how would the above operations look in a HTML5 Browser?

> 1. www.mysite.com
> 1. www.mysite.com?state=1
> 1. www.mysite.com?state=2
> 1. www.mysite.com?state=3
> 1. www.mysite.com?state=4
> 1. www.mysite.com?state=3
> 1. www.mysite.com?state=1
> 1. www.mysite.com
> 1. www.mysite.com?state=3
>
> _Note: These urls also work in HTML4 browsers and Search Engines. So no need for the `#!` fragment-identifier that google ["recommends"](http://getsatisfaction.com/balupton/topics/support_googles_recommendation_for_ajax_deeplinking)._

And how would they look in a HTML4 Browser?

> 1. www.mysite.com
> 1. www.mysite.com#?state=1/uid=1
> 1. www.mysite.com#?state=2/uid=2
> 1. www.mysite.com#?state=3/uid=3
> 1. www.mysite.com#?state=4
> 1. www.mysite.com#?state=3/uid=3
> 1. www.mysite.com#?state=1/uid=1
> 1. www.mysite.com
> 1. www.mysite.com#?state=3/uid=3
>
> _Note: These urls also work in HTML5 browsers - we use `replaceState` to transform these HTML4 states their HTML5 equivalents so the user won't even notice :-)_

What's the deal with the UIDs used in the HTML4 States?

- UIDs are used when we utilise a `title` and/or `data` in our state. Adding a UID allows us to associate particular states with data and titles while keeping the urls as simple as possible (don't worry it's all tested, working and a lot smarter than I'm making it out to be).
- If you aren't utilising `title` or `data` then we don't even include a UID (as there is no need for it) - as seen by State 4 above :-)
- We also shrink the urls to make sure that the smallest url will be used. For instance we will adjust `http://www.mysite.com/#http://www.mysite.com/projects/History.js` to become `http://www.mysite.com/#/projects/History.js` automatically. (again tested, working, and smarter).
- It works with domains, subdomains, subdirectories, whatever - doesn't matter where you put it. It's smart.

Is there a working demo?

> - Sure is, give it a download and navigate to the demo directory in your browser :-)
> - If you are after something a bit more adventurous than a end-user demo, check out the tests.php file - it'll rock your world and show all the vast use cases that History.js supports.


## Download & Installation

1. Download History.js and upload it to your webserver. Download links: [tar.gz](https://github.com/balupton/History.js/tarball/master) or [zip](https://github.com/balupton/History.js/zipball/master)

2. Include [JSON2](http://www.json.org/js.html) for HTML4 Browsers Only *(replace www.yourwebsite.com)*

		<script type="text/javascript">
			if ( typeof JSON === 'undefined' ) {
				var
					url = 'http://www.yourwebsite.com/history.js/scripts/compressed/json2.min.js',
					scriptEl = document.createElement('script');
				scriptEl.type = 'text/javascript';
				scriptEl.src = url;
				document.body.appendChild(scriptEl);
			}
		</script>

3. Include the Adapter for your Framework:

	- [jQuery](http://jquery.com/)

			<script type="text/javascript" src="http://www.yourwebsite.com/history.js/scripts/compressed/history.adapter.jquery.min.js"></script>

	- [Mootools](http://mootools.net/)

			<script type="text/javascript" src="http://www.yourwebsite.com/history.js/scripts/compressed/history.adapter.mootools.min.js"></script>

	- [Prototype](http://www.prototypejs.org/)

			<script type="text/javascript" src="http://www.yourwebsite.com/history.js/scripts/compressed/history.adapter.prototype.min.js"></script>

	- _Would you like to support another framework? No problem! It's very easy to create adapters, and I'll be happy to include them or help out if you [let me know](https://github.com/balupton/history.js/issues) :-)_

4. Include History.js

		<script type="text/javascript" src="http://www.yourwebsite.com/history.js/scripts/compressed/history.min.js"></script>


## Browsers: Tested and Working In

### HTML5 Browsers

- Chrome 8,9
- Firefox 4

### HTML4 Browsers

- Opera 10,11
- Safari 5
- Firefox 3
- IE 6,7,8


## Exposed API

### Functions

- `History.pushState(data,title,url)` <br/> Pushes a new state to the browser, `data` and `title` can be null
- `History.replaceState(data,title,url)` <br/> Replaces the existing state with a new state to the browser, `data` and `title` can be null
- `History.getState()` <br/> Get's the current state of the browser, returns an object with `data`, `title` and `url`
- `History.getHash()` <br/> Get's the current hash of the browser
- `History.Adapter.bind(element,event,callback)` <br/> A framework independent event binder, you may either use this or your framework's native event binder.
- `History.Adapter.trigger(element,event)` <br/> A framework independent event trigger, you may either use this or your framework's native event trigger.
- `History.Adapter.onDomLoad(callback)` <br/> A framework independent onDomLoad binder, you may either use this or your framework's native onDomLoad binder.
- `History.back()` <br/> Go back once through the history (same as hitting the browser's back button)
- `History.forward()` <br/> Go forward once through the history (same as hitting the browser's forward button)
- `History.go(X)` <br/> If X is negative go back through history X times, if X is positive go forwards through history X times
- `History.log(...)` <br/> Logs messages to the console, the log element, and fallbacks to alert if neither of those two exist
- `History.debug(...)` <br/> Same as `History.log` but only runs if `History.debug.enable === true`

### Events

- `window.onstatechange` <br/> Fired when the state of the page changes (does not include hashchanges)
- `window.onanchorchange` <br/> Fired when the anchor of the page changes (does not include state hashes)


## Notes on Compatibility

- State data will always contain the State's title and url at: `data.title` and `data.url`
- State data and title will not persist if the page was closed then re-opened, or navigated to another website then back - this is expected/standard functionality.
- State titles will always be applied to the document.title if set.
- ReplaceState functionality is emulated in HTML4 browsers by discarding the replaced state, so when the discarded state is accessed it is skipped using the appropriate `History.back()` / `History.forward()` call.
- History.js fixes a bug in Google Chrome where traversing back through the history to the home page does not return the correct state data.
- Setting a hash (even in HTML5 browsers) causes `onpopstate` to fire - this is expected/standard functionality.
	- As such, to ensure correct compatability between HTML5 and HTML4 browsers, we now have two new events:
		- `window.onstatechange`: this is the same as onpopstate except does not fire for traditional anchors
		- `window.onanchorchange`: this is the same as onhashchange except does not fire for states
	- To fetch the anchor/hash, you may use `History.getHash()`.


## Changelog

- v1.4.0 - February 09 2011
	- Small updates to Documentation
	- Unit Testing now uses [QUnit](http://docs.jquery.com/Qunit)
	- Corrected Safari 5 Support
	- Now uses queues instead of timeouts
		- This means the API works exactly as expected, no more need to wrap calls in timeouts

- v1.3.1 - February 04 2011
	- Improved Documentation

- v1.3.0 - January 31 2011
	- Support for cleaner HTML4 States

- v1.2.1 - January 30 2011
	- Fixed History.log always being called - [reported by dlee](https://github.com/balupton/History.js/issues/#issue/2)
	- Re-Added `History.go(index)` support

- v1.2.0 - January 25 2011
	- Support for HTML4 States in HTML5 Browsers (added test)
	- Updates of Documentation

- v1.1.0 - January 24 2011
	- Developed a series of automated test cases
	- Fixed issue with traditional anchors
	- Fixed issue with differing replaceState functionality in HTML4 Browsers
	- Fixed issue with Google Chrome artefacts being carried over to the initial state
	- Provided `onstatechange` and `onanchorchange` events

- v1.0.0 - January 22 2011
	- Supported `History.pushState` and `History.replaceState` degradation
	- Supported jQuery, MooTools and Prototype Frameworks


## Todo for Upcoming Releases

- Minor: Add a compilation test to ensure `.debug = false` and no `History.log` calls exist.
