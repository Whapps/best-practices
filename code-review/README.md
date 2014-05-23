In order to maintain a high quality of Online-Rewards code, this document
establishes the means by which code is vetted for deployment by OLR developers.

Goals:
======

* Propogate development culture and ecosystem through code review.

* Identify and promote shareable coding practices.

* Collect and disseminate knowledge.

* Improve practice, in finite steps, with finite chunks of code.

* Promote commonality and consistency (but not conformity and copy-pasta).

* Promote "effortless" development workflow (e.g. git, c5, legacy, etc).

* Make testable code.
  * TDD
  * Use argument passing to influence behavior for testing purposes

* Make literate code.
  * Naming conventiions

* Identify intrensic OLR code idioms, fixtures and artifacts.
  * Tables class
  * Form class
  * tree_append, as_tree

* Identify effective code documentation and in-line comment principles.
  * [POD](https://github.com/Whapps/best-practices/blob/master/perl-style/Pod_Coverage.md "POD coverage")
  * Inline code comments on the line preceding each code paragraph.

* Use automated tools to maintain constency, eliminate tedium and mistakes.
  * [Perltidy](https://github.com/Whapps/best-practices/tree/master/perl-style#perltidy/ "Perltidy")
  * XMLLint

Mechanics:
==========

When?
-----

* At least once per week for not more than 1 hour.

Where?
------

* Github + Paris

* The Github Issues interface, while cool, would just be another layer of
communication that would not benefit code review.  Also, it includes an
overabundance of time and milestone based items that we would never use.

How?
----

* Each member of the group submits code for review via email (or in a
best-practices meeting).

* Read code flagged for review.

* Each member of the group should bring any questions that they have regarding
the logic of the code or the documentation.

* Each member of the group, that has found a section of code to change, files a
pull request for the original code in question.

(A PR takes into account not only what code to inspect, but also where you
intend changes to be applied.  Maintains a history of code review based
changes.)

* Commit comments - Use an action verb and explain why the changes were made in 50
chars or less.  If there are more substantial changes to document, use the
standard commit message format of a single line with "Multiple changes:" followed
by a blank line, followed by the type of lines described earlier.

* The person whos code is being reviewed is either not to be in the room or
there to quietly take notes or answer questions.  Argument is not accptible but
discussion is.

* If too many bugs or major issues were identified by a reviewer, a second
review may be required before committing any fixes.

* The developer responsible for the code is also responsible for making all
necessary review-related changes.

* Use the GitHub compare view to see the change diff.

