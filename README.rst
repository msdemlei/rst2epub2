rst2epub2
===============

This code consists of two tools:

* a binary, ``rst2epub``, to convert rst files into epub2 compliant
  files (ie that pass epub check, can be loaded into Apple, BN, Kobo,
  etc. Or converted to mobi and thrown into AMZN)
* a library, ``epublib``, that has the ability to programatically
  create epub files. See the ``test`` function in ``epublib/epub.py``
  for more details. There is experimental support for KF8 fixed layout
  as well.


Install
============

running::

  make dev

will create a virtualenv in ``env``. The ``rst2epub.py`` binary will be
located in the ``env/bin/`` directory.

Known to work on linux systems. (Should work on apple, cygwin, MS with
some futzing).

Docs
======

There are a few rst tweaks to support features such as metadata. See
the sample doc for examples of a complete book and how to generate
both epub and mobi files.

Feel free to interact via github for support.

Injecting RST Extensions
------------------------

Sometimes you need custom RST extensions in a book project.  To let you 
declare these, rst2epub2 will import a module local_extensions.py that
sits in the directory it is executed in.  This module should, as a
side effect of the import, declare directives or text roles.  For 
instance, the following code would define directive ``article-intro`` (that 
essentially just attaches a class to the child material):

	from docutils import nodes
	from docutils.parsers.rst import directives
	from docutils.parsers import rst

	class _ArticleIntro(rst.Directive):
		has_content = True

		def run(self):
			self.assert_has_content()
			body = "\n".join(self.content)
			wrapper = nodes.container(rawsource=body)
			self.state.nested_parse(self.content, 0, wrapper)
			wrapper["classes"].append("article-intro")
			return [wrapper]

	directives.register_directive("article-intro", _ArticleIntro)

Note that with this feature someone might make you check out an rstx 
epub project and then execute arbitrary code while you simply run 
rst2epub.  We are not sure if that is an expectable attack scenario.


Thanks
========

timtambin@gmail.com wrote the original epublib and hosted it on google
code. I've tweaked (pep8'd) it and imported into github.
