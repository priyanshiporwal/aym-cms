
# AYM CMS Readme
AN AMAZING PROJECT!
The AYM CMS is a simple static CMS system built on top
of [Django][1] templates, and a 76 line Python build script.

You can use all the default Django template filters and tags,
and AYM CMS has a couple of custom tags to facilitate
handling static content more intelligently.

[1]: http://djangoproject.com/


### Prerequisites

AYM makes use of a handful of Python libraries:

1.  Django
2.  Python-Markdown
3.  PIL
4.  Pygments

All of those libraries should be an easy_install away:

    sudo easy_install django
    sudo easy_install markdown
    sudo easy_install PIL
    sudo easy_install Pygments


### Installation

If you have the folder containing this README file, then
AYM CMS is already installed.

### Usage

Unzip/download the AYM CMS project folder. Then type

    python build.py

to build the current website to the ``deploy/`` directory.

### Adding Pages to your CMS

Adding a page to your cms is a two step affair:

1.  Create a template within the ``templates/`` directory.
     You can take advantage of template inheritance and all
     the normal Django templates goodies to do so.

2. Add the template's name to ``PAGES_TO_RENDER`` in
    your ``settings.py`` file.

You may create templates in subfolders of ``templates/``,
but be sure that the template string you give to ``PAGES_TO_RENDER``
reflects that.

For example, you might create the ``charts`` subfolder in ``templates/``,
so that its path is ``templates/charts/``. If you create a template named
``line_chart.html``, then you would update ``PAGES_TO_RENDER`` to look
like this:

    PAGES_TO_RENDER = (
        'index.html',
        'charts/line_chart.html',
    )

AKA, it works exactly the same way that Django does.

### Using YUI Compressor with AYM CMS

[yuic]: http://developer.yahoo.com/yui/compressor/

In the settings file, set ``YUI_COMPRESSOR`` to
be a path to a [YUI Compressor][yuic] jar on your computer.
Afterwards, all CSS and JS in your ``statc/`` directory
will automatically be compressed.

*You must download [YUI Compressor][yuic] yourself.*
*It is not bundled with AYM CMS.*

### Using CleverCSS with AYM CMS

[clever_css]: http://sandbox.pocoo.org/clevercss/

You can use [CleverCSS][clever_css] with AYM CMS as well.
Simply install CleverCSS

    sudo easy_install CleverCSS

And set these settings in ``settings.py``:

    USE_CLEVER_CSS = True
    CLEVER_CSS_EXT = ".ccss"

By default it treats ``.ccss`` files as CleverCSS,
but you could change it to treat all ``.css`` files
as CleverCSS if you desire.

``.ccss`` files will be renamed to ``.css`` by the
build script, and will be compressed with YUI Compressor
if you have it enabled.


### Using HSS with AYM CMS

[hss]: http://ncannasse.fr/projects/hss

You can now use .hss files, as specified by
[the HSS project page][hss], to create your
site's CSS files.

*You must download [HSS][hss] yourself.*
*It is not bundled with AYM CMS.*

Simply put the ``.hss`` file into your ``static/``
directory, and AYM CMS will translate them into
``.css`` files in your ``deploy/static/`` folder
when you run the deploy script.

Note that means in your HTML you'll need to import
``my_hss.hss`` as ``my_hss.css``, because the file
extension will be translated by the build script!

(If you are using YUI Compressor, then AYM CMS
will first convert ``.hss`` files to ``.css``,
and will then compress them. AKA: it'll just
work how it should.)


### Adding Static Content to your CMS

All content within the ``static/`` directory will be copied over
into the ``deploy/static/`` directory when you run the build
script.

The script will preserve subfolders as well, so simply arrange
things to your liking in the ``static/`` folder and you'll be
good to go.


### Adding Images to Project

Add any images you want to the ``images/`` folder, and AYM
CMS will automatically create thumbnails and copy them into a
subfolder of the ``deploy/static`` folder when you run the build
script.

You can access these images within your templates in two ways.
First, the ``images`` template context is a list of your images
which can can use as follows:

    {% for image in images %}
    <a href="{{ image.image }}">
        <img src="{{ image.thumbnail }}" alt="{{ image.filename }}">
    </a>
    {% endfor %}

Second, you may refer to images by name via the ``images_dict``
context variable. If you added the ``crazy_cats.png`` image to
your ``images/`` directory, you could then refer to it like this:

    <img src="{{ images_dict.crazy_cats.image }}">

And thats really all there is to it.


### AYM Template Tags

At this time there are two custom template tags included
in the AYM CMS project: ``markdown`` and ``syntax``.

``markdown`` renders the enclosed text as Markdown markup.
It is used as follows:

    {% load aym %}
    <p> I love templates. </p>
    {% markdown %}
    Render this **content all in Markdown**.

    >> Writing in Markdown is quicker than
    >> writing in HTML.

    1.  Or at least that is my opinion.
    2.  What about you?
    {% endmarkdown %}

``syntax`` uses Pygments to render the enclosed text with
a code syntax highlighter. Usage is:

    {% load aym %}
    <p> blah blah blah </p>
    {% syntax objc %}
    [obj addObject:[NSNumber numberWithInt:53]];
    return [obj autorelease];
    {% endsyntax %}

They are both intended to make writing static content
quicker and less painful.
