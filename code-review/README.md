**In order to maintain a high quality of Online-Rewards code, this document
establishes the means by which code is vetted for deployment by OLR developers.**

"Mandatory code review is a best practice.  Code review that is not mandatory is called asking for advice."

Goals:
======

* Propogate development culture and ecosystem through code review.

* Identify and promote shareable coding practices.

* Improve practice, in finite steps, with finite chunks of code.

* Promote commonality and consistency (but not conformity and copy-pasta).

* Make testable code.

* Make literate code.

* Identify useful/crusty OLR code idioms, fixtures and artifacts.
  * Tables class
  * Forms class
  * tree_append, as_tree, etc.

* Identify and ameliorate code-smells with improved logic or enhanced
comments.
  * Overly long routines
  * Overly complex conditions
  * Magic literals, etc.

* Identify effective code documentation and in-line comment principles.
  * [POD](https://github.com/Whapps/best-practices/blob/master/perl-style/Pod_Coverage.md "POD coverage")
  * Inline code comments on the line preceding each code paragraph.

* Use automated tools to maintain constency, eliminate tedium and mistakes.
  * [Perltidy](https://github.com/Whapps/best-practices/tree/master/perl-style#perltidy/ "Perltidy")

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

* Every member of the team has their code reviewed.

* Each member of the group submits code for review via email (or in a
best-practices meeting).

* Read code flagged for review.

* Each member of the group should bring any questions that they have regarding
the logic of the code or the documentation.

* Each member of the group, that has found a section of code to change, files a
pull request for the original code in question ("TODO").

* Keep your changes focused to the minimum necessary.  Otherwise further review
will be necessary.

* Commit comments - Use an action verb and explain why the changes were made in 50
chars or less.  If there are more substantial changes to document, use the
standard commit message format of a single line with "Multiple changes:" followed
by a blank line, followed by the type of lines described earlier.

* Provide suggestions for follow up.
  * Sometimes you'll want to suggest a change, but it's big, or not strictly
    related to the current patch, and can be done separately.
  * The author can use the TODO flag in the code

* The person whos code is being reviewed is either not to be in the room or
there to quietly take notes or answer questions.  Argument is not accptible but
discussion is.

* If too many bugs or major issues were identified by a reviewer, a second
review may be required before committing any fixes.

* The developer responsible for the code is also responsible for making all
necessary review-related changes.

In the review, focus on:

1. Intent
  * What change is the author trying to make?
  * Is the bug they're fixing really a bug?
  * Is the feature they're adding one that is wanted?
2. Architecture
  * Is this the right place for the code to operate?
3. Implementation
  * Does the code do what it says?
  * Does it introduce new bugs?
  * Is it documented?
4. Grammar
  * Comment and doc quality
  * Variable, routine name quality, etc

Benefits
--------

This may be "preaching to the choir" but:

"By forcing someone else to review a piece of code you guarantee that at least two people understand it."

"By getting someone else to provide feedback based on reading, rather than writing, the code you verify that the code is readable."

"You increase the chances that someone will notice a bug before it manifests itself in production."

"By having a culture of "everyone's code gets reviewed" you promote a culture of positive, constructive feedback."
