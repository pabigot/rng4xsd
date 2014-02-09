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

XSD schema that should pass validation are in `pass10`; schema that
should fail validation are in `fail10`.  The script `runchecks` invokes
xmllint and (optionally)
[Jing](http://www.thaiopensource.com/relaxng/jing.html) on these tests.
Due to [jing-trang issue
178](https://code.google.com/p/jing-trang/issues/detail?id=178) a patch
in `patches` may need to be applied to Jing.

An XSD 1.1 variant is in the work queue.
