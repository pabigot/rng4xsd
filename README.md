This repository contains [RELAX NG](http://relaxng.org/) schema for the
[W3C XML Schema Definition Language](http://www.w3.org/TR/xmlschema-1).

The work is based on
[xmlschema.rng](http://www.jenitennison.com/schema/xmlschema.rng)
created by [Jeni Tennison](http://www.jenitennison.com/index.xml).  See
the [original
notice](https://lists.oasis-open.org/archives/relax-ng/200106/msg00228.html)
for some background.

Changes in this repository reflect modifications required to fix false
diagnostics on a subset of the [Open Geospatial Consortium,
Inc.](http://www.opengeospatial.org/) schemas and other schemas
supported by [PyXB](http://pyxb.sourceforge.net/).

Testing
=======

These directories contain tests:

* `test10` has tests that pass or fail only on XSD 1.0, not on XSD 1.1
* `test11` has tests that pass or fail only on XSD 1.1, not on XSD 1.0
* `test1x` has tests that pass or fail similarly on both XSD 1.0 and XSD
   1.1

Tests are XSD files, and are named with an initial `t` if the contained
schema is valid and an initial `f` if the contained schema is invalid.

The script `runchecks` runs all the tests, using
[xmllint](http://xmlsoft.org/xmllint.html) and (if `jing.jar` is present
in the workspace)
[Jing](http://www.thaiopensource.com/relaxng/jing.html) on these tests.
By default testing is done with the `xsd10.rng` schema; invoking
`runchecks xsd11.rng` will test the XSD 1.1 schema.

These diagnostics are emitted by jing:

    xsd10.rng:1127:16: warning: conflicting ID-types for attribute "id" of element "restriction" from namespace "http://www.w3.org/2001/XMLSchema"
    xsd11.rng:1336:16: warning: conflicting ID-types for attribute "id" of element "restriction" from namespace "http://www.w3.org/2001/XMLSchema"

Note that the diagnostic is normally an error, which prevents
validation.  A patch to [jing-trang issue
178](https://code.google.com/p/jing-trang/issues/detail?id=178) which
converts the diagnostic to a warning is available in the `patches`
subdirectory.
