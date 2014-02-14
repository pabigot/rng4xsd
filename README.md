This repository contains [RELAX NG](http://relaxng.org/) schema for the
[W3C XML Schema Definition Language](http://www.w3.org/TR/xmlschema-1).

The project home for this material is the <a
href="https://github.com/pabigot/rng4xsd">rng4xsd project page</a> on
github.  Obtain updates and report issues on the project page.

History
=======

The work is based on
[xmlschema.rng](http://www.jenitennison.com/schema/xmlschema.rng)
created by [Jeni Tennison](http://www.jenitennison.com/index.xml) to
support [XSD 1.0](http://www.w3.org/TR/xmlschema-1/).  See the [original
notice](https://lists.oasis-open.org/archives/relax-ng/200106/msg00228.html)
for some background.

The `xsd10.rng` file is a refactored version that more closely follows
the [XSD 1.0 Schema for Schemas](http://www.w3.org/2001/XMLSchema.xsd).
It incorporates about a dozen substantive changes to fix false
diagnostics on a subset of the [Open Geospatial Consortium,
Inc.](http://www.opengeospatial.org/) schemas and other schemas
supported by [PyXB](http://pyxb.sourceforge.net/).

The `xsd11.rng` file derives from that to support [XSD
1.1](http://www.w3.org/TR/xmlschema11-1/), following its [Schema for
Schemas]( http://www.w3.org/TR/xmlschema11-2/XMLSchema.xsd).

`rng4xsd.rng` is a support schema defining documentation elements and
annotations that relate the RELAX NG patterns to XSD components.

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

Emacs Support
=============

As of version 23, emacs integrates support for [nXML
mode](http://www.thaiopensource.com/nxml-mode/).  The installation can
be customized for XSD by these steps:

(1) Add the following to your `.emacs` file:

    ;; nXML mode customization
    (add-to-list 'auto-mode-alist '("\\.xsd\\'" . xml-mode))
    (add-hook 'nxml-mode-hook
        '(lambda ()
           (make-local-variable 'indent-tabs-mode)
           (setq indent-tabs-mode nil)
           (add-to-list 'rng-schema-locating-files
            "~/.emacs.d/nxml-schemas/schemas.xml")))

(2) Install the following as `schemas.xml` in `~/.emacs.d/nxml-schemas/`:

    <locatingRules xmlns="http://thaiopensource.com/ns/locating-rules/1.0">
      <!-- Extend to support W3C XML Schema Definition Language, which as
           of 1.1 are known as "XSD" rather than "XML Schema" to avoid
           confusion with other XML schema languages such as RELAX NG. -->
      <uri pattern="*.xsd" typeId="XSD"/>
      <namespace ns="http://www.w3.org/2001/XMLSchema" typeId="XSD"/>
      <documentElement localName="schema" typeId="XSD"/>
      <typeId id="XSD 1.0" uri="xsd10.rnc"/>
      <typeId id="XSD 1.1" uri="xsd11.rnc"/>
      <typeId id="XSD" uri="xsd.rnc"/>
    </locatingRules>

(3) Install the following as `xsd.rnc` in `~/.emacs.d/nxml-schemas/`:

    xsd10 = external "xsd10.rnc"
    xsd11 = external "xsd11.rnc"
    start = xsd10 | xsd11

(4) Copy the `xsd10.rng` and `xsd11.rng` schemas to
    `~/.emacs.d/nxml-schemas/`

(5) Edit the copied `xsd10.rng` file to eliminate the `pattern`
    parameter, which causes parsing errors in emacs when editing files
    that include
    [xs:field](http://www.w3.org/TR/xmlschema-1/#element-field) or
    [xs:selector](http://www.w3.org/TR/xmlschema-1/#element-selector)
    elements:

    diff --git a/xsd10.rng b/xsd10.rng
    index 32002b6..b202bfe 100644
    --- a/xsd10.rng
    +++ b/xsd10.rng
    @@ -1029,7 +1029,9 @@
         <ref name="annotated"/>
         <attribute name="xpath">
           <data type="token">
    +<!-- Causes problems in emacs
             <param name="pattern">(\.//)?(((child::)?((\i\c*:)?(\i\c*|\*)))|\.)(/(((child::)?((\i\c*:)?(\i\c*|\*)))|\.))*(\|(\.//)?(((child::)?((\i\c*:)?(\i\c*|\*)))|\.)(/(((child::)?((\i\c*:)?(\i\c*|\*)))|\.))*)*</param>
    +-->
           </data>
         </attribute>
       </element>
    @@ -1041,7 +1043,9 @@
         <ref name="annotated"/>
         <attribute name="xpath">
           <data type="token">
    +<!-- Causes problems in emacs
             <param name="pattern">(\.//)?((((child::)?((\i\c*:)?(\i\c*|\*)))|\.)/)*((((child::)?((\i\c*:)?(\i\c*|\*)))|\.)|((attribute::|@)((\i\c*:)?(\i\c*|\*))))(\|(\.//)?((((child::)?((\i\c*:)?(\i\c*|\*)))|\.)/)*((((child::)?((\i\c*:)?(\i\c*|\*)))|\.)|((attribute::|@)((\i\c*:)?(\i\c*|\*)))))*</param>
    +-->
           </data>
         </attribute>
       </element>

(6) Convert the XML-format schemas to compact syntax with
    [trang](http://code.google.com/p/jing-trang):

    trang xsd10.rng xsd10.rnc
    trang xsd11.rng xsd11.rnc
