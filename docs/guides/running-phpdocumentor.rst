Running phpDocumentor
=====================

In this guide we are going to explain how to generate documentation for your application and how to tune it to your
liking. phpDocumentor supports a wide range of options related to the generation of documentation that can help you.

Quickstart
----------

Let's start with the minimum that you need in order to run phpDocumentor. In the next few chapters we will be adding
more and more options but you can safely ignore those and just use what we describe in this chapter.

When running phpDocumentor there are three command-line options that are essential:

**``-d``**
    specifies the directory, or directories, of your project that you want to document.

**``-f``**
    specifies a specific file, or files, in your project that you want to document.

**``-t``**
    specifies the location where your documentation will be written (also called 'target folder').

The above options are all you need to generate your documentation as demonstrated in this example:

.. code-block:: shell-session

    $ phpdoc -d path/to/my/project -f path/to/an/additional/file -t path/to/my/output/folder

.. note::

    For phpDocumentor to work you must specify at least a directory or file to scan, or have this information in a
    :doc:`configuration<../references/configuration>` file. phpDocumentor won't assume that you want to document the
    directory from where you run the command.

The target folder is optional but if you omit it then your documentation will be generated in a subfolder, of
your current working directory, called **``output``**.

Configuration
-------------

Before we continue to discuss the other options that phpDocumentor offers we would like to mention that phpDocumentor
supports the use of a :doc:`configuration<../references/configuration>` file. All you need to do is add a file called
'phpdoc.dist.xml' to the root of your project, add your options to it and then invoke the ``phpdoc`` command without
arguments.

phpDocumentor will look in the current working directory for the configuration file and use its contents to determine
options such as where your project files are and where to output your documentation.

You can override the settings in the configuration on a per user basis using another file called 'phpdoc.xml'. It is
recommended to prevent committing that file to your code repository using, for example, a ``.gitignore`` file.

.. important::

    When present, the file 'phpdoc.xml' is used instead of 'phpdoc.dist.xml' and thus does not supplement it.

**``--config``**
    You can even specify an alternate location for your 'phpdoc.xml' using the '--config' command-line option.

For more information on the options and format supported by the configuration it is best to consult the
:doc:`configuration reference<../references/configuration>`.

Influencing the List of Project Files
-------------------------------------

**``-d`` or ``-f``**
    As mentioned in the Quickstart above you can select which directories and files to document using the ``-d`` (for
    directories and their files) or the ``-f`` (for just single files). You can even provide those options multiple times
    if you need multiple files or directories.

**``--ignore``**
    Sometimes you may want to exclude entire directories, or files from your documentation build because they contain
    unwanted third-party documentation, or because you just don't need to transform documentation content for certain
    files in your project. The ``--ignore`` option lets you specify what directories and files to exclude from your
    project.

    A basic example of the ``--ignore`` option is excluding one or more directories from your project.
    If you have a 'vendor' directory that is not relevant to your project documentation, you can exclude it by specifying
    ``--ignore "vendor/"``. Repeat the ``--ignore`` option to exclude multiple directories.

    If you have a single file in the 'tests' directory that you want to exclude from the documentation build, you can
    declare it explicitly by specifying ``--ignore "tests/excludeme.php"``. This command will transform all PHP files in
    the 'tests' directory *except for* 'excludeme.php'.

    The ``--ignore`` option also supports `glob patterns`_ to exclude files and directories using wildcards. To ignore all
    files ending with ``test.php`` you can use ``--ignore "**/*test.php".``

.. important::

   Enclose any value for an option that provides a wildcard with double quotes to prevent your command line from
   interpreting them.

.. note::

    When you want to provide a relative path, keep in mind that this is relative to the Project Root Folder.
    The project's root folder is the first folder that the provided folders have in common, so for
    ``-d "src/phpDocumentor,src/SomethingElse" this is the directory "src" and not the current working directory. When
    in doubt, check the output of phpDocumentor, it mentions the project's root folder after all files are collected.

**``--hidden`` and ``--ignore-symlinks``**
    By default phpDocumentor will ignore hidden files and will not follow symlinks. This will prevent unwanted
    documentables and loops in paths. Should you want to document hidden files you can do so by supplying the option
    ``--hidden=off``, for traversing symlinks you can provide the option ``--ignore-symlinks=off``. Easy!

Customizing the Look and Feel
-----------------------------

phpDocumentor offers a wide range of options for changing the look and feel of your documentation but almost all of
them are captured in a template (believe me, you do not want to configure this on the command-line). So, the easiest way
is to pick a template using the option ``--template``.

**``--template``**
    It is possible to generate output using two templates at once. This can be convenient for generating HTML documentation
    and Checkstyle XML output at the same time. Generating output for two templates can be accomplished by providing the
    ``--template`` option twice or by using a comma-separated list:

    .. code-block:: shell-session

        $ phpdoc --template="clean" --template="checkstyle" -d .
        $ phpdoc --template="clean,checkstyle" -d .

In addition to the options offered by the templates themselves, there are two command-line options to influence the
output of your documentation:

**``--defaultpackagename``**
    This option changes the name of your 'default', or nameless, package to that of your preference. This way you can,
    for example, change the default package name to the name of your application.

**``--title``**
    This option will change the title in your browser's titlebar and, for some templates, the title text of the template
    itself. This is a small convenience to personalize the template for your application.

Using a configuration file you can apply more customization to the look and feel of the documentation, please see the
chapter on :doc:`templates` for more information on this subject.

File Encoding
-------------

**``--encoding``**
    phpDocumentor assumes that your project's files are encoded using **UTF-8**. If your encoding differs you can use the
    ``--encoding`` command line option to instruct phpDocumentor to expect that.

Limiting the included tags and elements
---------------------------------------

By default phpDocumentor documents all public and protected elements barring those with the tag
:doc:`../references/phpdoc/tags/internal` or :doc:`../references/phpdoc/tags/ignore`. All tags of an element feature in
the documentation, either by providing functionality or in the list of meta-data for that element.
It is possible to influence this behaviour using a series of options that affect the amount of information that is
provided in the documentation.

**``--visibility`` and ``--parseprivate``**
    To change which elements are shown in the documentation based on their visibility you can use the ``--visibility``
    option. This option accepts a comma-separated list of the visibilities supported by PHP (public, protected or private),
    the value 'api' to only document items that have the :doc:`../references/phpdoc/tags/api` tag associated with them or
    the value 'internal' to show all elements including those marked with the :doc:`../references/phpdoc/tags/internal` tag.
    This latter option (``--visibility=internal``) is deprecates the ``--parseprivate`` option as it is superseded by this
    option.

**``--ignore-tags``**
    Now that you know how to change the list of elements that can be displayed, you can even influence which tags are
    shown in your documentation. Contrary to phpDocumentor 1, version 2 will now display all tags by default; if you
    want to omit specific tags from the documentation you can do that using the ``--ignore-tags`` option. By providing a
    comma-separated list of tag names (case-sensitive) phpDocumentor can be instructed to omit those tags, and their
    contents, from the documentation.

Markers
-------

phpDocumentor is mostly about DocBlocks and processing inline documentation. However it will also collect
markers.

In short, a Marker is a single-line inline comment that starts with a single, identifying, word and has a description.
Let's take a look at an example to make this less abstract:

.. code-block::

    // TODO: Move this code to another location

As you can see here, we indicate that a specific piece of code on the following line should be moved. phpDocumentor
collects these markers and generates a report that shows which and where these markers are placed. In the example above
you may notice that there is a colon (``:``) after the marker text; this is optional and will be ignored when present.

**``--markers``**
    By default, phpDocumentor only collects markers that start with TODO or FIXME, as these are the most common, but you can
    provide an alternative list using the ``--markers`` command line option.

.. hint::

   TODO markers also get a special treatment; phpDocumentor generates a report detailing which todo items are in your
   code and uses both the :doc:`../references/phpdoc/tags/todo` tag and the TODO marker to compile this list.
