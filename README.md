# ŜABLONO
  [![Build Status](https://travis-ci.org/r0man/sablono.svg)](https://travis-ci.org/r0man/sablono)
  [![Dependencies Status](http://jarkeeper.com/r0man/sablono/status.svg)](http://jarkeeper.com/r0man/sablono)
  [![Downloads](https://jarkeeper.com/r0man/sablono/downloads.svg)](https://jarkeeper.com/r0man/sablono)

Lisp/Hiccup style templating for Facebook's
[React](http://facebook.github.io/react) in
[ClojureScript](https://github.com/clojure/clojurescript).

![](http://imgs.xkcd.com/comics/tags.png)

## Installation

Via Clojars: https://clojars.org/sablono

[![Current Version](https://clojars.org/sablono/latest-version.svg)](https://clojars.org/sablono)

## Dependencies

*Ŝablono* doesn't declare a dependency on React anymore. Use the React
 dependencies from one of the ClojureScript wrappers or provide the
 dependencies yourself like this:

``` clj
[cljsjs/react "0.14.3-0"]
[cljsjs/react-dom "0.14.3-1"]
[cljsjs/react-dom-server "0.14.3-0"]
```

## Usage

Most functions from [Hiccup](https://github.com/weavejester/hiccup)
are provided in the `sablono.core` namespace. The library can be used
with [Om](https://github.com/swannodette/om) like this:

``` clj
(ns example
  (:require [om.core :as om :include-macros true]
			[sablono.core :as html :refer-macros [html]]))

(defn widget [data]
  (om/component
   (html [:div "Hello world!"
		  [:ul (for [n (range 1 10)]
				 [:li {:key n} n])]
		  (html/submit-button "React!")])))

(om/root widget {} {:target (. js/document (getElementById "my-app"))})
```

## HTML Tags

*Ŝablono* only supports tags and attributes that can be handled by
React. This means you can't have your own custom tags and attributes
at the moment. For more details take a look at the
[Tags and Attributes](http://facebook.github.io/react/docs/tags-and-attributes.html)
section in the React documentation.

## HTML Attributes

HTML attributes in
[React](http://facebook.github.io/react/docs/tags-and-attributes.html#html-attributes)
are camel-cased and the `class` and `for` attributes are treated
special. *Ŝablono* renames attributes with dashes in their name to the
camel-cased version and handles the `class` and `for` special
case. This is more consistent with
[Hiccup](https://github.com/weavejester/hiccup) and naming conventions
used in Clojure.

An `input` element with event listeners attached to it would look like
this in *Ŝablono*:

``` clj
(html [:input
	   {:auto-complete "off"
		:class "autocomplete"
		:on-change #(on-change %1)
		:on-key-down #(on-key-down %1)
		:type "text"}])
```

## Setting innerHTML of a DOM node

It is not recommended to directly set the innerHTML of DOM nodes, but
in some cases it is necessary. i.e. injecting a HTML string that was
generated from Markdown.

``` clj
(html [:div {:dangerouslySetInnerHTML {:__html "<div>hello world</div>" }}])
```

You can read more at [React's special attributes](http://facebook.github.io/react/docs/special-non-dom-attributes.html).

## Thanks

This library is based on James Reeves excellent [Hiccup](https://github.com/weavejester/hiccup) library.

## License

Copyright © 2013-2015 r0man

Distributed under the Eclipse Public License either version 1.0 or (at
your option) any later version.
