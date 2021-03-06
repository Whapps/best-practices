**In order to maintain a high quality of Online-Rewards code, this document
establishes the means by which code is vetted for deployment by OLR developers.**

"Mandatory code review is a best practice.  Code review that is not mandatory is called asking for advice."

Benefits
========

This may be "preaching to the choir" but:

"By forcing someone else to review a piece of code you guarantee that at least two people understand it."

"By getting someone else to provide feedback based on reading, rather than writing the code, you verify that the code is readable."

"You increase the chances that someone will notice a bug before it manifests itself in production."

"By having a culture of "everyone's code gets reviewed" you promote a culture of positive, constructive feedback."

Goals:
======

* Propogate development culture and ecosystem through code review.

* Identify and promote shareable coding practices.

* Improve practice, in finite steps, with finite chunks of code.

* Promote commonality and consistency (but not conformity and copy-pasta).

* Make testable code.

* Make self-documenting code.

* Identify useful/crusty OLR code idioms, fixtures and artifacts.
  * Tables class
  * Forms class
  * tree_append, as_tree, etc.

* Identify and ameliorate code-smells with improved logic or enhanced
comments.
  * Overly long routines
  * Overly complex conditions
  * Magic literals, etc.
  * TODOs that never get acted on

* Identify effective code documentation and in-line comment principles.
  * [POD](https://github.com/Whapps/best-practices/blob/master/perl-style/Pod_Coverage.md "POD coverage")
  * Inline code comments on the line preceding each code paragraph.
  * Active voice
  * Explain with "W words" Why, when, who, what as opposed to "how."

* Use automated tools to maintain constency, eliminate tedium and mistakes.
  * [Perltidy](https://github.com/Whapps/best-practices/tree/master/perl-style#perltidy/ "Perltidy")

Mechanics:
==========

When?
-----

* At least once per project and before QA.

Where?
------

* Your workstation.

* Github + Paris

Who?
----

* Every member of the team has their code reviewed!

* Assign an official note taker.

What?
-----

* Client.pm and related "side-car" modules

* Jobs

* Forms, Tables

* Schemas

How?
----

* Submit code for review via email or in a best-practices meeting.

* Before the review, read the code!

* Use the Github Issues tool to flag sections of code needing attention.

* Bring any questions regarding the code logic or documentation.

* The person whos code is being reviewed is there to take notes or answer questions.  Argument is not acceptable but discussion is. :)

* For the review, focus on:

1. Intent
  * What change is the author trying to make?
  * Is the bug they're fixing really a bug?
  * Is the feature they're adding one that is wanted?
2. Architecture
  * Is this the right place for the code to operate?
  * Is this the right data structure to use?
3. Implementation
  * Does the code do what it says?
  * Does it introduce new bugs?
  * Is it documented?
4. Grammar
  * Comment and doc quality
  * Variable, routine name quality, etc

* Provide suggestions for follow up.
  * Sometimes a change is big, or not strictly related to the current patch, and can be done separately.
  * The author can use the TODO flag in the code

* If too many bugs or major issues were identified, a second
review may be required before committing any fixes to master.

* If you have a patch, file a pull request for found sections of code to change/fix.

* The developer responsible for the code is also responsible for making all
necessary review-related changes.

Possibly Handy Links
--------------------

http://blog.fogcreek.com/increase-defect-detection-with-our-code-review-checklist-example
