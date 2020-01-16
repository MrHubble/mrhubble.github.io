---
id: 175
title: Turbolinks, browser back and duplicate plugins
date: 2019-02-01T03:21:35+00:00
author: finally
layout: rails
guid: https://www.mrcodebot.com/?p=175
permalink: /turbolinks-browser-back-and-duplicate-plugins/
categories:
  - Rails
---
After upgrading to Turbolinks 5 we started seeing duplicate plugins when pressing the browser back button.

This is because our plugin (ie `select2` and `datatables`) is being initialised for a second time and is explained with:

_Often you’ll want to perform client-side transformations to HTML received from the server. You may have a JavaScript function that queries the document for all `select` inputs and initialises `select2`. If this runs on `turbolinks:load` (ie when using `jquery-turbolinks` with the Turbolinks compatibility shim), when you navigate away, Turbolinks saves a copy of the transformed page to its cache. When we press the Back button, Turbolinks restores the page from cache, fires `turbolinks:load` again, and our code inserts a second set of the plugin._

_To avoid this problem, make your transformation function idempotent. An idempotent transformation is safe to apply multiple times without changing the result beyond its initial application_

The above is modified from <https://github.com/turbolinks/turbolinks#making-transformations-idempotent>

For example, with select2, this could be fixed with:

<pre class="wp-block-code"><code class="hljs language-javascript">&lt;span class="hljs-keyword">if&lt;/span> (!$(&lt;span class="hljs-string">'select'&lt;/span>).hasClass(&lt;span class="hljs-string">'select2-hidden-accessible'&lt;/span>))
  $(&lt;span class="hljs-string">'select'&lt;/span>).select2({...})</code></pre>

So it&#8217;s all happy now? Not so fast….

I think it&#8217;s worthwhile reinstating:

<blockquote class="wp-block-quote">
  <p>
    Turbolinks saves a copy of the transformed page to its cache
  </p>
</blockquote>

So what is the transformed page? I believe it&#8217;s the DOM elements only and not the events associated with the elements. This leads us to the next problem we run into. We&#8217;ve fixed our code so it&#8217;s idempotent and duplicates are no longer experienced. However, our plugin events no longer fire. This is explained here:

<blockquote class="wp-block-quote">
  <p>
    Turbolinks saves a copy of the current page to its cache just before rendering a new page. Note that Turbolinks copies the page using cloneNode(true), which means any attached event listeners and associated data are discarded.<br /> https://github.com/turbolinks/turbolinks#understanding-caching
  </p>
</blockquote>

I mistakenly thought [Persisting Elements Across Page Loads](https://github.com/turbolinks/turbolinks#persisting-elements-across-page-loads) would fix this:

<blockquote class="wp-block-quote">
  <p>
    Turbolinks allows you to mark certain elements as permanent. Permanent elements persist across page loads, so that any changes you make to those elements do not need to be reapplied after navigation. Designate permanent elements by giving them an HTML <code>id</code> and annotating them with <code>data-turbolinks-permanent</code>. Before each render, Turbolinks matches all permanent elements by id and transfers them from the original page to the new page, preserving their data and event listeners.
  </p>
</blockquote>

This did not fix the problem of events not firing.

## The easy but possibly unsuitable fixes

  1. Ensuring Specific Pages Trigger a Full Reload<figure class="wp-block-image">

<img src="https://www.mrcodebot.com/wp-content/uploads/2019/02/Screen-Shot-2018-12-16-at-7.24.54-pm-700x200.png" alt="" class="wp-image-186" srcset="https://www.mrcodebot.com/wp-content/uploads/2019/02/Screen-Shot-2018-12-16-at-7.24.54-pm-700x200.png 700w, https://www.mrcodebot.com/wp-content/uploads/2019/02/Screen-Shot-2018-12-16-at-7.24.54-pm-300x86.png 300w, https://www.mrcodebot.com/wp-content/uploads/2019/02/Screen-Shot-2018-12-16-at-7.24.54-pm-768x219.png 768w" sizes="(max-width: 700px) 100vw, 700px" /> </figure>

<https://github.com/turbolinks/turbolinks#ensuring-specific-pages-trigger-a-full-reload>

  1. Turbolinks.clearCache<figure class="wp-block-image">

<img src="https://www.mrcodebot.com/wp-content/uploads/2019/02/Screen-Shot-2018-12-16-at-7.26.26-pm-700x135.png" alt="" class="wp-image-187" srcset="https://www.mrcodebot.com/wp-content/uploads/2019/02/Screen-Shot-2018-12-16-at-7.26.26-pm-700x135.png 700w, https://www.mrcodebot.com/wp-content/uploads/2019/02/Screen-Shot-2018-12-16-at-7.26.26-pm-300x58.png 300w, https://www.mrcodebot.com/wp-content/uploads/2019/02/Screen-Shot-2018-12-16-at-7.26.26-pm-768x148.png 768w" sizes="(max-width: 700px) 100vw, 700px" /> </figure>

<https://github.com/turbolinks/turbolinks#turbolinksclearcache>

  1. Opting Out of Caching<figure class="wp-block-image">

<img src="https://www.mrcodebot.com/wp-content/uploads/2019/02/Screen-Shot-2018-12-16-at-7.27.42-pm-700x273.png" alt="" class="wp-image-188" srcset="https://www.mrcodebot.com/wp-content/uploads/2019/02/Screen-Shot-2018-12-16-at-7.27.42-pm-700x273.png 700w, https://www.mrcodebot.com/wp-content/uploads/2019/02/Screen-Shot-2018-12-16-at-7.27.42-pm-300x117.png 300w, https://www.mrcodebot.com/wp-content/uploads/2019/02/Screen-Shot-2018-12-16-at-7.27.42-pm-768x300.png 768w" sizes="(max-width: 700px) 100vw, 700px" /> </figure>

<https://github.com/turbolinks/turbolinks#opting-out-of-caching>

## Why not use the easy fixes?

In our app some of our jQuery plugins are used frequently (ie `select2` and `datatables`) over multiple views. The easiest fix would have been to opt out of caching completely but then we would obviously lose the speed benefits of caching:

<blockquote class="wp-block-quote">
  <p>
    Turbolinks maintains a cache of recently visited pages. This cache serves two purposes: to display pages without accessing the network during restoration visits, and to improve perceived performance by showing temporary previews during application visits.
  </p>
</blockquote>

<https://github.com/turbolinks/turbolinks#understanding-caching>

I read that you can opt out of caching within the `<body>` tag (rather than in `<head>`) and use logic based on the controller and action to determine whether to cache or not, however, as mentioned, we use the plugins frequently and I didn&#8217;t think this would be a clean and maintainable solution.

This is likewise for maintaining what actions need to use `Turbolinks.clearCache()`

## The solution for our situation &#8211; Stimulus and Turbolinks two-pack punch

  1. Preparing the Page to be Cached<figure class="wp-block-image">

<img src="https://www.mrcodebot.com/wp-content/uploads/2019/02/Screen-Shot-2018-12-16-at-7.28.56-pm-700x148.png" alt="" class="wp-image-189" srcset="https://www.mrcodebot.com/wp-content/uploads/2019/02/Screen-Shot-2018-12-16-at-7.28.56-pm-700x148.png 700w, https://www.mrcodebot.com/wp-content/uploads/2019/02/Screen-Shot-2018-12-16-at-7.28.56-pm-300x64.png 300w, https://www.mrcodebot.com/wp-content/uploads/2019/02/Screen-Shot-2018-12-16-at-7.28.56-pm-768x163.png 768w" sizes="(max-width: 700px) 100vw, 700px" /> </figure>

<https://github.com/turbolinks/turbolinks#preparing-the-page-to-be-cached>

We can unbind the plugin (`select2` in this example) before it&#8217;s cached:

`$('#example').select2('destroy');`

<https://select2.org/programmatic-control/methods#event-unbinding>

  1. Using Stimulus

<pre class="wp-block-code"><code class="hljs language-javascript">&lt;span class="hljs-keyword">export&lt;/span> &lt;span class="hljs-keyword">default&lt;/span> &lt;span class="hljs-class">&lt;span class="hljs-keyword">class&lt;/span> &lt;span class="hljs-keyword">extends&lt;/span> &lt;span class="hljs-title">Controller&lt;/span> &lt;/span>{
  connect() {
    &lt;span class="hljs-keyword">this&lt;/span>.select2mount()
    &lt;span class="hljs-built_in">document&lt;/span>.addEventListener(&lt;span class="hljs-string">"turbolinks:before-cache"&lt;/span>, () =&gt; {
      &lt;span class="hljs-keyword">this&lt;/span>.select2unmount()
     }, { &lt;span class="hljs-attr">once&lt;/span>: &lt;span class="hljs-literal">true&lt;/span> })

    select2unmount() {
      $(&lt;span class="hljs-keyword">this&lt;/span>.element).select2(&lt;span class="hljs-string">'destroy'&lt;/span>)
    }
  }</code></pre>

On `connect()` of our Stimulus controller (which is called even when navigating to a cached page), before we cache the page we tear down the `select2` plugin. For this instance we are storing state in the database so we don&#8217;t need to store the select values for the cache, however, if that was the case you could use:

<pre class="wp-block-code"><code class="hljs language-javascript">&lt;span class="hljs-comment">// make sure the HTML itself has those elements selected&lt;/span>
&lt;span class="hljs-comment">// since the HTML is what is saved in the turbolinks snapshot&lt;/span>
values.forEach(&lt;span class="hljs-function">(&lt;span class="hljs-params">val&lt;/span>) =&gt;&lt;/span> {
  $(&lt;span class="hljs-keyword">this&lt;/span>.selectTarget).find(&lt;span class="hljs-string">`option[value="&lt;span class="hljs-subst">${val}&lt;/span>"]`&lt;/span>).attr(&lt;span class="hljs-string">'selected'&lt;/span>, &lt;span class="hljs-string">'selected'&lt;/span>);
})</code></pre>

[https://github.com/pascallaliberte/stimulus-turbolinks-select2/blob/master/\_assets/controllers/multi-select\_controller.js](https://github.com/pascallaliberte/stimulus-turbolinks-select2/blob/master/_assets/controllers/multi-select_controller.js)

On initialisation Datatables may insert new DOM elements ie the Search input. Before we leave the page we tear down the plugin to avoid re-initialising it twice. With Datatables, I believe this should also remove all of the newly added DOM elements so we’re back at the original state of the table that we’ve coded in our views. There is a caveat to this though, with server side datatables populated by AJAX, the `tbody` maintains it’s data even though that wasn’t present in our original code.
Plus there’s different behaviour depending on whether you’re doing an application visit versus a restoration visit which added to the confusion.

## To disable cache or to not disable cache?

In the future, if Turbolinks with a jQuery plugin is giving us grief I would be highly tempted to save time and disable caching for that page….. if it’s not too complicated to maintain
This is how Basecamp (in 2016 at least) disabled cache for new and edit pages<figure class="wp-block-image">

<img src="https://www.mrcodebot.com/wp-content/uploads/2019/02/basecamp_no_cache-700x736.png" alt="" class="wp-image-176" srcset="https://www.mrcodebot.com/wp-content/uploads/2019/02/basecamp_no_cache-700x736.png 700w, https://www.mrcodebot.com/wp-content/uploads/2019/02/basecamp_no_cache-285x300.png 285w, https://www.mrcodebot.com/wp-content/uploads/2019/02/basecamp_no_cache-768x807.png 768w, https://www.mrcodebot.com/wp-content/uploads/2019/02/basecamp_no_cache.png 787w" sizes="(max-width: 700px) 100vw, 700px" /> </figure>

<https://github.com/turbolinks/turbolinks/issues/87#issuecomment-241425366>

## Helpful notes

<blockquote class="wp-block-quote">
  <p>
    When possible, avoid using the turbolinks:load event to add other event listeners directly to elements on the page body. Instead, consider using event delegation to register event listeners once on document or window<br /> https://github.com/turbolinks/turbolinks#observing-navigation-events
  </p>

  <p>
    State is stored in the HTML, so that controllers can be discarded between page changes, but still reinitialize as they were when the cached HTML appears again.<br /> https://stimulusjs.org/handbook/origin#how-stimulus-differs-from-mainstream-javascript-frameworks
  </p>

  <cite><br /></cite>
</blockquote>

**References:**

<https://m.phillydevshop.com/turbolinks-5-and-datatables-a882c29d6eff>