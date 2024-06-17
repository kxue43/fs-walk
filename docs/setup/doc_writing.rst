Documentation Writing
=============================

The documentations being read are written in the `reStructuredText`_ markup language. This allows us to compile
the sources by the Python program `Sphinx`_ and publish them to different destinations, such as GitHub Pages and
Confluence. The :program:`Sphinx` and :abbr:`rst (reStructuredText)` combination is especially useful for
generating package API documentations from the inline Python docstrings.

.. note:: Why not Markdown instead of reStructuredText?
   To put it simply --- Markdown lacks very much in "semantic markup" compared to :abbr:`rst (reStructuredText)`.
   For example, in :abbr:`md (Markdown)`, there is only one type of "inline markup" --- wrapping some texts
   inside a pair of backticks. The wrapped texts are usually rendered in monospace fonts, indicating
   that it is something related to code, but this semantics doesn't convey any more information.
   On the other hand, :abbr:`rst (reStructuredText)` uses `roles`_ to tell the rendering program whether the wrapped 
   text is the name of a program, a file, or even a Python class/function. This enables the program to make better
   decisions about the rendering style and to create more precise and helpful hyperlinks.

Prerequisites
-----------------

Before contributing to the documentations, we need to make the following preparations.

* Make sure you have installed dependencies via :program:`poetry`.

* Install some :program:`npm` packages for viewing the generated doc site during development.

  .. code-block:: bash

     $ pushd docs
     $ npm install
     $ popd

* Go through the `reStructuredText Primer`_ for an introduction to :abbr:`rst (reStructuredText)` concepts and syntax.

Writing Documentations
-----------------------

Documentation sources are all in the :file:`docs/` directory, so we need to :command:`cd` into the :file:`docs/`
directory and always work there when writing documentations. The following workflow is recommended.

* Start a local static site server run by `nodemon`_.

  .. code-block:: bash

     $ npm start

* Go to http://localhost:8080 in a browser to view the generated doc site.

* Open another terminal to run commands that compile documentation sources. Don't forget to :command:`cd` into
  :file:`docs/` as well.

* After writing some new documentations, run the following command, make sure there is no warning or error issued,
  and go to http://localhost:8080 to check if the rendering effect is as expected.
  *Refresh the browser window if the contents don't change*.

  .. code-block:: bash
  
     $ make html

* Source compilation is incremental by default. Sometimes not all hyperlinks work correctly unless we compile from
  fresh. In this case, clean up all generated files and perform a clean compile:

  .. code-block:: bash
  
     $ make clean && make html

* Offending line numbers will be reported when there are errors or warnings after :command:`make html`. Correct 
  them and repeat the above process.

.. tip:: When a PR is merged to the ``main`` branch of the `repository`_, if there are any changes to files in the
   :file:`docs/` directory, the `Publish Documentation`_ will be triggered, which automatically compiles the sources
   and publishes them to `Confluence`_ and `GitHub Pages`_.
     

.. _reStructuredText: https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html
.. _Sphinx: https://www.sphinx-doc.org/en/master/index.html
.. _roles: https://www.sphinx-doc.org/en/master/usage/restructuredtext/roles.html
.. _reStructuredText Primer: https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html
.. _nodemon: https://www.npmjs.com/package/nodemon
.. _repository: https://github.com/gdcorp-ecomm/fintech-tax-global-vertex-edge-vms
.. _Publish Documentation: https://github.com/gdcorp-ecomm/fintech-tax-global-vertex-edge-vms/blob/main/.github/workflows/publish-docs.yml
.. _GitHub Pages: https://ubiquitous-adventure-g31epqm.pages.github.io/index.html
.. _Confluence: https://godaddy-corp.atlassian.net/wiki/spaces/FINTECH/pages/3282662594/Vertex+Edge+Container+Orchestration
