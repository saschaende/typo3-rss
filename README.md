# Generate RSS 1.0, RSS 2.0 or ATOM formatted feeds

This package can be used to generate feeds in either RSS 1.0, RSS 2.0 or ATOM
format.

Applications can create a feed object, several feed item objects, set
several types of properties of either feed and feed items, and add items to
the feed.

Once a feed is fully composed with its items, the feed class can generate
the necessary XML structure to describe the feed in RSS or ATOM format. This
structure can be directly sent to the browser, or just returned as string.

Credits for use of the FeedWriter Library go to: https://github.com/mibe/FeedWriter

## Requirements

PHP >= 5.3

If you don't have 5.3 available on your system, there's a version supporting
PHP >= 5.0 in the "legacy-php-5.0" branch.

# Examples

## Example Atom

```
date_default_timezone_set('UTC');
use \SaschaEnde\Rss\ATOM;

// IMPORTANT : No need to add id for feed or channel. It will be automatically created from link.
//Creating an instance of ATOM class.
$TestFeed = new ATOM();
//Setting the channel elements
//Use wrapper functions for common elements
$TestFeed->setTitle('Testing the RSS writer class');
$TestFeed->setDescription('This is just a short example for using the FeedWriter classes...');
$TestFeed->setLink('http://www.ajaxray.com/rss2/channel/about');
$TestFeed->setDate(new DateTime());
$TestFeed->setImage('https://upload.wikimedia.org/wikipedia/commons/thumb/0/07/CommaFeed.svg/256px-CommaFeed.svg.png');
//For other channel elements, use setChannelElement() function
$TestFeed->setChannelElement('author', array('name'=>'Anis uddin Ahmad'));
//You can add additional link elements, e.g. to a PubSubHubbub server with custom relations.
$TestFeed->setSelfLink('http://example.com/myfeed');
$TestFeed->setAtomLink('http://pubsubhubbub.appspot.com', 'hub');
//Adding a feed. Generally this portion will be in a loop and add all feeds.
//Create an empty Item
$newItem = $TestFeed->createNewItem();
//Add elements to the feed item
//Use wrapper functions to add common feed elements
$newItem->setTitle('The first feed');
$newItem->setLink('http://www.yahoo.com');
$newItem->setDate(time());
$newItem->setAuthor('Anis uddin Ahmad', 'anis@example.invalid');
$newItem->addEnclosure('http://upload.wikimedia.org/wikipedia/commons/4/49/En-us-hello-1.ogg', 11779, 'audio/ogg');
//Internally changed to "summary" tag for ATOM feed
$newItem->setDescription('This is a test of adding CDATA encoded description by the php <b>Universal Feed Writer</b> class');
$newItem->setContent('<h1>hi.</h1> <p>This is the content for the entry.</p>');
//Now add the feed item
$TestFeed->addItem($newItem);
//OK. Everything is done. Now generate the feed.
$TestFeed->printFeed();
```

## Example Minimum

```
date_default_timezone_set('UTC');

use \SaschaEnde\Rss\RSS2;

/**
 * Copyright (C) 2008 Anis uddin Ahmad <anisniit@gmail.com>
 *
 * This file is part of the "Universal Feed Writer" project.
 *
 * This program is free software: you can redistribute it and/or modify
 * it under the terms of the GNU General Public License as published by
 * the Free Software Foundation, either version 3 of the License, or
 * (at your option) any later version.
 *
 * This program is distributed in the hope that it will be useful,
 * but WITHOUT ANY WARRANTY; without even the implied warranty of
 * MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
 * GNU General Public License for more details.
 *
 * You should have received a copy of the GNU General Public License
 * along with this program.  If not, see <http://www.gnu.org/licenses/>.
 */

//Creating an instance of RSS2 class.
$TestFeed = new RSS2();

//Setting the channel elements
//Use wrapper functions for common channel elements
$TestFeed->setTitle('Testing & Checking the RSS writer class');
$TestFeed->setLink('http://www.ajaxray.com');
$TestFeed->setDescription('This is a test of creating a RSS 2.0 feed Universal Feed Writer');

//Let's add some feed items: Create two empty Item instances
$itemOne = $TestFeed->createNewItem();
$itemTwo = $TestFeed->createNewItem();

//Add item details
$itemOne->setTitle('The title of the first entry.');
$itemOne->setLink('http://www.google.de');
$itemOne->setDate(time());
$itemOne->setDescription('And here\'s the description of the entry.');

$itemTwo->setTitle('Lorem ipsum');
$itemTwo->setLink('http://www.example.com');
$itemTwo->setDate(1234567890);
$itemTwo->setDescription('Lorem ipsum dolor sit amet, consectetur, adipisci velit');

//Now add the feed item
$TestFeed->addItem($itemOne);
$TestFeed->addItem($itemTwo);

//OK. Everything is done. Now generate the feed.
$TestFeed->printFeed();
```

## Example RSS1

```
date_default_timezone_set('UTC');

use \SaschaEnde\Rss\RSS1;

//Creating an instance of RSS1 class.
$TestFeed = new RSS1();

//Setting the channel elements
//Use wrapper functions for common elements
//For other optional channel elements, use setChannelElement() function
$TestFeed->setTitle('Testing the RSS writer class');
$TestFeed->setLink('http://www.ajaxray.com/rss2/channel/about');
$TestFeed->setDescription('This is test of creating a RSS 1.0 feed by Universal Feed Writer');

//It's important for RSS 1.0
$TestFeed->setChannelAbout('http://www.ajaxray.com/rss2/channel/about');

// An image is optional.
$TestFeed->setImage('https://upload.wikimedia.org/wikipedia/commons/0/07/Rss.jpg', 'You can add an image, if you want', 'http://www.ajaxray.com/rss2/channel/about');

//Adding a feed. Generally this portion will be in a loop and add all feeds.

//Create an empty FeedItem
$newItem = $TestFeed->createNewItem();

//Add elements to the feed item
//Use wrapper functions to add common feed elements
$newItem->setTitle('The first feed');
$newItem->setLink('http://www.yahoo.com');
//The parameter is a timestamp for setDate() function
$newItem->setDate(time());
$newItem->setDescription('This is test of adding CDATA encoded description by the php <b>Universal Feed Writer</b> class');
//Use core addElement() function for other supported optional elements
$newItem->addElement('dc:subject', 'Nothing but test');

//Now add the feed item
$TestFeed->addItem($newItem);

//Adding multiple elements from array
//Elements which have an attribute cannot be added by this way
$newItem = $TestFeed->createNewItem();
$newItem->addElementArray(array('title'=>'The 2nd feed', 'link'=>'http://www.google.com', 'description'=>'This is a test of the FeedWriter class'));
$TestFeed->addItem($newItem);

//OK. Everything is done. Now generate the feed.
$TestFeed->printFeed();
```

## Example RSS2

```
date_default_timezone_set('UTC');

use \SaschaEnde\Rss\RSS2;

// Creating an instance of RSS2 class.
$TestFeed = new RSS2();

// Setting some basic channel elements. These three elements are mandatory.
$TestFeed->setTitle('Testing & Checking the Feed Writer project');
$TestFeed->setLink('https://github.com/mibe/FeedWriter');
$TestFeed->setDescription('This is just an example how to use the Feed Writer project in your code.');

// Image title and link must match with the 'title' and 'link' channel elements for RSS 2.0,
// which were set above.
$TestFeed->setImage('https://upload.wikimedia.org/wikipedia/commons/thumb/d/d9/Rss-feed.svg/256px-Rss-feed.svg.png', 'Testing & Checking the Feed Writer project', 'https://github.com/mibe/FeedWriter');

// Use the setChannelElement() function for other optional channel elements.
// See http://www.rssboard.org/rss-specification#optionalChannelElements
// for other optional channel elements. Here the language code for American English is used for the
// optional "language" channel element.
$TestFeed->setChannelElement('language', 'en-US');

// The date when this feed was lastly updated. The publication date is also set.
$TestFeed->setDate(time());
$TestFeed->setChannelElement('pubDate', date(\DATE_RSS, strtotime('2013-04-06')));

// By using arrays as channelElement values, can be set element like this
//  <skipDays>
//      <day>Saturday</day>
//      <day>Sunday</day>
// </skipDays>
// Thanks to - Peter Fargaš <peter.fargas.work@googlemail.com>
$TestFeed->setChannelElement("skipDays", array('day'=>['Saturday','Sunday']));

// You can add additional link elements, e.g. to a PubSubHubbub server with custom relations.
// It's recommended to provide a backlink to the feed URL.
$TestFeed->setSelfLink('http://example.com/myfeed');
$TestFeed->setAtomLink('http://pubsubhubbub.appspot.com', 'hub');

// You can add more XML namespaces for more custom channel elements which are not defined
// in the RSS 2 specification. Here the 'creativeCommons' element is used. There are much more
// available. Have a look at this list: http://feedvalidator.org/docs/howto/declare_namespaces.html
$TestFeed->addNamespace('creativeCommons', 'http://backend.userland.com/creativeCommonsRssModule');
$TestFeed->setChannelElement('creativeCommons:license', 'http://www.creativecommons.org/licenses/by/1.0');

// If you want you can also add a line to publicly announce that you used
// this fine piece of software to generate the feed. ;-)
$TestFeed->addGenerator();

// Here we are done setting up the feed. What's next is adding some feed items.

// Create a new feed item.
$newItem = $TestFeed->createNewItem();

// Add basic elements to the feed item
// These are again mandatory for a valid feed.
$newItem->setTitle('Hello World!');
$newItem->setLink('http://www.example.com');
$newItem->setDescription('This is a test of adding a description by the <b>Feed Writer</b> classes. It\'s automatically CDATA encoded.');

// The following method calls add some optional elements to the feed item.

// Let's set the publication date of this item. You could also use a UNIX timestamp or
// an instance of PHP's DateTime class.
$newItem->setDate('2013-04-07 00:50:30');

// You can also attach a media object to a feed item. You just need the URL, the byte length
// and the MIME type of the media. Here's a quirk: The RSS2 spec says "The url must be an http url.".
// Other schemes like ftp, https, etc. produce an error in feed validators.
$newItem->addEnclosure('http://upload.wikimedia.org/wikipedia/commons/4/49/En-us-hello-1.ogg', 11779, 'audio/ogg');

// If you want you can set the name (and email address) of the author of this feed item.
$newItem->setAuthor('Anis uddin Ahmad', 'admin@ajaxray.com');

// You can set a globally unique identifier. This can be a URL or any other string.
// If you set permaLink to true, the identifier must be an URL. The default of the
// permaLink parameter is false.
$newItem->setId('http://example.com/URL/to/article', true);

// Use the addElement() method for other optional elements.
// This here will add the 'source' element. The second parameter is the value of the element
// and the third is an array containing the element attributes.
$newItem->addElement('source', 'Mike\'s page', array('url' => 'http://www.example.com'));

// Now add the feed item to the main feed.
$TestFeed->addItem($newItem);

// Another method to add feeds items is by using an array which contains key-value pairs
// of every item element. Elements which have attributes cannot be added by this way.
$newItem = $TestFeed->createNewItem();
$newItem->addElementArray(array('title'=> 'The 2nd item', 'link' => 'http://www.google.com', 'description' => 'Just another test.'));
$TestFeed->addItem($newItem);

// OK. Everything is done. Now generate the feed.
// Then do anything (e,g cache, save, attach, print) you want with the feed in $myFeed.
$myFeed = $TestFeed->generateFeed();

// If you want to send the feed directly to the browser, use the printFeed() method.
$TestFeed->printFeed();
```

# Authors

(in chronological order)

- [Anis uddin Ahmad](https://github.com/ajaxray)  
- [Michael Bemmerl](https://github.com/mibe)  
- Phil Freo  
- Paul Ferrett
- Brennen Bearnes
- Michael Robinson
- Baptiste Fontaine
- Kristián Valentín
- Brandtley McMinn
- Julian Bogdani
- Cedric Gampert
- Yamek
- thielj
- Pavel Khakhlou
- Daniel
- Tino Goratsch
- Sascha Ende (TYPO3 port)