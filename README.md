**STATUS:** As of the 1.0 release, this project is no longer under active development. I still plan on accepting any passing pull requests to become part of future 2.x releases.

-----

Doorstop
========

[![Build Status](http://img.shields.io/travis/jacebrowning/doorstop/master.svg)](https://travis-ci.org/jacebrowning/doorstop)
[![Coverage Status](http://img.shields.io/coveralls/jacebrowning/doorstop/master.svg)](https://coveralls.io/r/jacebrowning/doorstop)
[![Scrutinizer Code Quality](http://img.shields.io/scrutinizer/g/jacebrowning/doorstop.svg)](https://scrutinizer-ci.com/g/jacebrowning/doorstop/?branch=master)
[![PyPI Version](http://img.shields.io/pypi/v/Doorstop.svg)](https://pypi.python.org/pypi/Doorstop)
[![PyPI Downloads](http://img.shields.io/pypi/dm/Doorstop.svg)](https://pypi.python.org/pypi/Doorstop)

Doorstop manages the storage of textual requirements alongside source code in version control.

<img align="right" width="200" src="https://github.com/jacebrowning/doorstop/blob/develop/pages/images/logo-black-white.png"/>
When a project utilizes this tool, each linkable item (requirement, test case, etc.) is stored as a YAML file in a designated directory. The items in each directory form a document. The relationship between documents forms a tree hierarchy. Doorstop provides mechanisms for modifying this tree, validating item traceability, and publishing documents in several formats.

Additional reading:

- publication: [JSEA Paper](http://www.scirp.org/journal/PaperInformation.aspx?PaperID=44268#.UzYtfWRdXEZ)
- talks: [GRDevDay](https://speakerdeck.com/jacebrowning/doorstop-requirements-management-using-python-and-version-control), [BarCamp](https://speakerdeck.com/jacebrowning/strip-searched-a-rough-introduction-to-requirements-management)
- sample: [Generated HTML](http://doorstop.info/reqs/index.html)
- documentation: [API](http://doorstop.info/docs/index.html), [Demo](http://nbviewer.ipython.org/gist/jacebrowning/9754157)

Getting Started
===============

Requirements
------------

* Python 3.3+
* A version control system for requirements storage

Installation
------------

Doorstop can be installed with pip:

```
$ pip3 install doorstop
```

or directly from source:

```
$ git clone https://github.com/jacebrowning/doorstop.git
$ cd doorstop
$ python3 setup.py install
```

After installation, Doorstop is available on the command-line:

```
$ doorstop --help
```

And the package is available under the name 'doorstop':

```
$ python3
>>> import doorstop
>>> doorstop.__version__
```

Basic Usage
===========

Document Creation
-----------------

**Parent Document**

A document can be created inside a directory that is under version control:

    $ doorstop create REQ ./reqs
    created document: REQ (@/reqs)

Items can be added to the document and edited:

    $ doorstop add REQ
    added item: REQ001 (@/reqs/REQ001.yml)

    $ doorstop edit REQ1
    opened item: REQ001 (@/reqs/REQ001.yml)

**Child Documents**

Additional documents can be created that link to other documents:

    $ doorstop create TST ./reqs/tests --parent REQ
    created document: TST (@/reqs/tests)

Items can be added and linked to parent items:

    $ doorstop add TST
    added item: TST001 (@/reqs/tests/TST001.yml)

    $ doorstop link TST1 REQ1
    linked item: TST001 (@/reqs/tests/TST001.yml) -> REQ001 (@/reqs/REQ001.yml)

Document Validation
-------------------

To check a document hierarchy for consistency, run the main command:

    $ doorstop
    valid tree: REQ <- [ TST ]

Document Publishing
-------------------

A text report of a document can be displayed:

    $ doorstop publish TST
    1       TST001

            Verify the foobar will foo and bar.

            Links: REQ001

Other formats are also supported:

    $ doorstop publish TST --html
    <!DOCTYPE html>
    ...
    <body>
    <h1>1 (TST001)</h1>
    <p>Verify the foobar will foo and bar.</p>
    <p><em>Links: REQ001</em></p>
    </body>
    </html>

Or a file can be created using one of the supported extensions:

    $ doorstop publish TST path/to/tst.md
    publishing TST to path/to/tst.md...

Supported formats:

- Text: **.txt**
- Markdown: **.md**
- HTML: **.html**

Content Interchange
-------------------

**Export**

Documents can be exported for editing or to exchange with other systems:

    $ doorstop export TST
    TST001:
      active: true
      dervied: false
      level: 1
      links:
      - REQ001
      normative: true
      ref: ''
      text: |
        Verify the foobar will foo and bar.

Or a file can be created using one of the supported extensions:

    $ doorstop export TST path/to/tst.csv
    exporting TST to path/to/tst.csv...
    exported: path/to/tst.csv

Supported formats:

- YAML: **.yml**
- Comma-Separated Values: **.csv**
- Tab-Separated Values: **.tsv**
- Microsoft Office Excel: **.xlsx**

**Import**

Items can be created/updated from the export formats:

    $ doorstop import path/to/tst.csv TST
