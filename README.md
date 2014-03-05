# Autolinker.js

Because I had so much trouble finding a good autolinking implementation out in the wild, I decided to roll my own. It 
seemed that everything I found out there was either an implementation that didn't cover every case, or was just limited 
in one way or another. 

So, this utility attempts to handle everything. It:

- Autolinks URLs, whether or not they start with the protocol (i.e. 'http://'). In other words, it will automatically link the 
  text "google.com", as well as "http://google.com".
- Will properly handle URLs with special characters
- Will properly handle URLs with query parameters or a named anchor (i.e. hash)
- Will autolink email addresses.
- Will autolink Twitter handles.
- Will properly handle HTML input. The utility will not change the `href` attribute inside anchor (&lt;a&gt;) tags (or any other 
  tag/attribute for that matter), and will not accidentally wrap the inner text of an anchor tag with a new one (which would cause 
  doubly-nested anchor tags).

Hope that this utility helps you as well!


## Usage

Simply copy the `dist/Autolinker.js` (or `dist/Autolinker.min.js`) file into your project, link to it with a script tag, and then run it as such:

	var linkedText = Autolinker.link( textToAutolink[, options] );
	
Example:

	var linkedText = Autolinker.link( "The sky is falling from google.com" );
	// Produces: "The sky is falling from <a href="http://google.com" target="_blank">google.com</a>"
	
### Options
There are options which may be specified for the linking. These are specified by providing an Object as the second parameter to `Autolinker.link()`. Options include:

- **newWindow** : Boolean<br />
  `true` to have the links should open in a new window when clicked, `false` otherwise. Defaults to `true`.
- **stripPrefix** : Boolean<br />
  `true` to have the 'http://' or 'https://' and/or the 'www.' stripped from the beginning of links, `false` otherwise. Defaults to `true`.
- **truncate** : Number<br />
  A number for how many characters long URLs/emails/twitter handles should be truncated to inside the text of a link. If the URL/email/twitter is over the number of characters, it will be truncated to this length by replacing the end of the string with a two period ellipsis ('..').
  Ex: a url like 'http://www.yahoo.com/some/long/path/to/a/file' truncated to 25 characters may look like this: 'yahoo.com/some/long/pat..'


If you wanted to disable links opening in new windows, you could do:

	var linkedText = Autolinker.link( "The sky is falling from google.com", { newWindow: false } );
	// Produces: "The sky is falling from <a href="http://google.com">google.com</a>"

And if you wanted to truncate the length of URLs (while also not opening in a new window), you could do:

	var linkedText = Autolinker.link( "http://www.yahoo.com/some/long/path/to/a/file", { truncate: 25, newWindow: false } );
	// Produces: "<a href="http://www.yahoo.com/some/long/path/to/a/file">yahoo.com/some/long/pat..</a>"


### More Examples
One could update a DOM element that has unlinked text to autolink them as such:

	var myTextEl = document.getElementById( 'text' );
	myTextEl.innerHTML = Autolinker.link( myTextEl.innerHTML );


## Changelog:

### 0.5.0

- Simplified the path / query string / hash processing into a single regular expression instead of 3 separate ones.
- Added support for parenthesis in URLs, such as: `en.wikipedia.org/wiki/IANA_(disambiguation)` (thanks @dandv)
- Add all known top-level domains (TLDs) (thanks @wouter0100)

### 0.4.0

Merged pull requests from @afeld:

- strip protocol and 'www.' by default - fixes #1
- truncate URLs from the end
- make simpler regex for detecting prefix
- remove trailing slashes from URLs, and handle periods at the end of paths
- re-use domain+TLD regexes for email matching
- add .me and .io to list of TLDs

Thanks Aidan

### 0.3.1

- Fixed an issue with handling nested HTML tags within anchor tags.

### 0.3

- Implemented the `truncate` option.

### 0.2

- Implemented autolinking Twitter handles.

### 0.1

* Initial implementation, which autolinks URLs and email addresses. Working on linking Twitter handles.
