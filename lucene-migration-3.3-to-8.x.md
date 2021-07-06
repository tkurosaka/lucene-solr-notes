# Lucene 3.3 to 8.8 Migration Notes
This repo is just to share the information I have been collecting to migrate [Okapi Framework](https://okapiframework.org/wiki/index.php?title=Main_Page) core library from Lucene 3.3 to 8.8, to implement its issue [#837](https://bitbucket.org/okapiframework/okapi/issues/837/).

The Pensieve Translation Memory, a part of Okapi Framework, uses Lucene to find exsiting *translation pair*, to reuse the existing translations. It uses a very old Lucene, version 3.3, and we want to upgrade it to 8.8. 

The current Pensieve implementation defines custome Query, Weight and Scorer classes. It turned out there have been a lot of incompatible changes between 3.3 and 8.8, and straight port is not possible.

# Version Incompatibility - Overview

The APIs change over time. At the release of each major version (x.0.0), incompatible changes happen including:
* Removal of classes, methods and/or field previously marked @Deprecated or @deprecated.
* Existing classes, methods and field become *final*, no longer allowing subclassing or overriding.
* Exsiting interfaces and/or (often abstract) classes change, requring new methods to be defined, and some methods become no longer overridable.
* Programming models change, often to enable more flexible, plugin architecture.

Lucene lists (or at least tries to list) all major differences between the version and its previous major version in these migration notes:
* 3.x to 4.0: https://lucene.apache.org/core/4_0_0/MIGRATE.html
* 4.x to 5.0: https://lucene.apache.org/core/5_0_0/MIGRATE.html
* 5.x to 6.0: https://lucene.apache.org/core/6_0_0/MIGRATE.html
* 6.x to 7.0: https://lucene.apache.org/core/6_0_0/MIGRATE.html
* 7.x to 8.0: https://lucene.apache.org/core/8_0_0/MIGRATE.html
* 7.x to 8.8: https://lucene.apache.org/core/8_8_0/MIGRATE.html

There is no migration guide spanning more than one major versions. To migrate for 5 major versions like my project, you have to read all migration notes. (Hint: Save them locally for easy grepping).

*more coming*
# Apendices
## Solr Change List
Solr has a comprehensive list of changes, all the way from version 1.1.0 to the latest version. As of July 2021, this is the latest:
https://solr.apache.org/docs/8_9_0/changes/Changes.html
Click the "Expand All" button at the top to see the complete changes.
