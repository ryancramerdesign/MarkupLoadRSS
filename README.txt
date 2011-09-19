ProcessWire RSS Loader
======================

Given an RSS feed URL, this module will pull it, and let you foreach() it 
or render it. This module will also cache feeds that you retrieve with it.

For ProcessWire 2.1+

Copyright 2011 by Ryan Cramer


INSTALLATION
============

The MarkupLoadRSS module installs in the same way as all PW modules:

1. Copy the MarkupLoadRSS.module file to your /site/modules/ directory. 
2. Login to ProcessWire admin, click 'Modules' and 'Check for New Modules'. 
3. Click 'Install' next to the Markup Load RSS module. 


USAGE
=====

The MarkupLoadRSS module is used from your template files. 
Usage is described with these examples: 


Example #1: Cycling through a feed
----------------------------------

    $rss = $modules->get("MarkupLoadRSS"); 
    $rss->load("http://www.di.net/articles/rss/");

    foreach($rss as $item) { 
        echo "<p>";
        echo "<a href='{$item->url}'>{$item->title}</a> ";
        echo $item->date . "<br /> ";
        echo $item->description; 
        echo "</p>";
    }


Example #2: Using the built-in rendering
----------------------------------------

    $rss = $modules->get("MarkupLoadRSS");
    echo $rss->render("http://www.di.net/articles/rss/");


Example #3: Specifying options and using channel titles
-------------------------------------------------------

    $rss = $modules->get("MarkupLoadRSS");

    $rss->limit = 5;
    $rss->cache = 0; 
    $rss->maxLength = 255; 
    $rss->dateFormat = 'm/d/Y H:i:s';

    $rss->load("http://www.di.net/articles/rss/");

    echo "<h2>{$rss->title}</h2>";
    echo "<p>{$rss->description}</p>";
    echo "<ul>"; 

    foreach($rss as $item) {
         echo "<li>" . $item->title . "</li>";
    }

    echo "</ul>";


OPTIONS
=======

Options MUST be set before calling load() or render().

    // specify that you want to load up to 3 items (default = 10)
    $rss->limit = 3;             

    // set the feed to cache for an hour (default = 120 seconds)
    // if you want to disable the cache, set it to 0.
    $rss->cache = 3600;          

    // set the max length of any field, i.e. description (default = 2048)
    // field values longer than this will be truncated
    $rss->maxLength = 255;

    // tell it to strip out any HTML tags (default = true)
    $rss->stripTags = true;

    // tell it to encode any entities in the feed (default = true); 
    $rss->encodeEntities = true;

    // set the date format used for output (use PHP date string)
    $rss->dateFormat = "Y-m-d g:i a";

See the $options array in the class for more options.

You can also customize all output produced by the render() method,
though it is probably easier just to foreach() the $rss yourself. But
see the module class file and $options array near the top to see how
to change the markup that render() produces. 


MORE DETAILS
============

This module loads the given RSS feed and all data from it. It then populates 
that data into a WireArray of Page-like objects. All of the fields in the RSS 
<items> feed are accessible, so you use whatever the feed provides.

The most common and expected field names in the RSS channel are: 

    $rss->title
    $rss->pubDate             (or $rss->date)
    $rss->description         (or $rss->body)
    $rss->link                (or $rss->url)
    $rss->created             (unix timestamp of pubDate)

The most common and expected field names for each RSS item are:

    $item->title
    $item->pubDate            (or $item->date)
    $item->description        (or $item->body)
    $item->link               (or $item->url)
    $item->created            (unix timestamp of pubDate)

For convenience and consistency, ProcessWire translates some common RSS 
fields to the PW-equivalent naming style. You can choose to use either the 
ProcessWire-style name or the traditional RSS name, as shown above.


HANDLING ERRORS
===============

If an error occurred when loading the feed, the $rss object will
have 0 items in it:

    $rss->load("...");
    if(!count($rss)) { error }

In addition, the $rss->error property always contains a detailed
description of what error occurred:

    if($rss->error) { echo "<p>{$rss->error}</p>"; }

I recommend only checking for or reporting errors when you are
developing and testing. On production sites you should skip
error checking/testing, as blank output is a clear indication
of an error. This module will not throw runtime exceptions so
if an error occurs, it's not going to halt the site.


SUPPORT
=======

Visit the ProcessWire forum at http://processwire.com/talk/


Copyright 2011 by Ryan Cramer


