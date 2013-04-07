.. title: Moving to Nikola, part 2
.. slug: moving-to-nikola-part-2
.. date: 2013/02/15 16:23:32
.. tags: 
.. link: 
.. description: 

One of the biggest issues moving all of the site to Nikola was how to integrate the former main page - the about-me page into the Nikola system. At first I thought it would be relatively easy since both systems use twitter bootstrap as their css foundation, it should be possible to translate most of the html into RestructuredText and have it look very similar. But as it turned out I could never muster up the motivation to do so and that this was for the better.

Adding html pages
=================

After some thinking about the problem and reading the change log for Nikola 5.2 it finally dawned upon me that I could just drop the html page into Nikola as a html-story and be finished with it. A first try to create a new story with html formatting failed with the message:

        Exception: Can't find a way, using your configuration, to createa page in format html. You may want to tweak post_compilers or post_pages in conf.py

I am thankful to the author of Nikola, Roberto Alsina, for writing helpful error messages. Fixing my config resolved this problem and I could successfully create a htlm-story.

.. code-block:: python

	post_pages = (
	    [...]
	    ("pages/*.html", "", "empty.tmpl", False),
	)
	
As you can see in the config line I also added a new template for html stories. The ``story.tmpl`` template includes the story title before the content and I do no want that when writing a story using html. The ``empty.tmpl`` looks like this:

.. code-block:: mako

	# -*- coding: utf-8 -*-
	<%inherit file="post.tmpl"/>
	<%block name="content">
	    ${post.text(lang)}
	</%block>

And the result is that I can just paste everything in the `<body>`-tag from the current main page into the created html-story and looks just like the original.

Setting the homepage
====================

When a Nikola site is opened the homepage is set to be the index of blog posts. For this site I want the homepage to be the newly created html story page. The first step to do so is to tell Nikola to place the generated file in to the root of the output so it can be read as an index, this is done by the ``post_pages`` configuration as seen above. That line specifies that ``*.html`` files found in the ``pages`` directory are to be placed in ``""`` - the root. The file ``pages/index.html`` will then be placed in ``output/index.html`` and is the top level file.

But for this to work I need to move the old index file, the blog index, or the page building will fail with an error message::

        ERROR: Two different tasks can't have a common target.'output/index.html' is a target for render_pages:output/index.html and render_indexes:output/index.html.

Fortunately the target for the blog index can be changed easily in the configuration file.

.. code-block:: python

        INDEX_PATH = "posts"

Tells Nikola to place the blog index into the ``posts`` directory. As a result I can find my new index directly under ``localhost:8000/`` and the blog index under ``localhost:8000/posts`` and all is well.
