Pod Coverage Guideline
======================
This is indended as a guideline to the Pod sections and documentation that Online Rewards requires as part of all programmer written code. By "programmer written" code, we are referring to code written by a programmer directly and **NOT** written by any tool that autogenerates code.  For example, `Client::DataObjects` code does not require any documentation, since it is not written by hand.

Pod documentation is written to document the code for other programmers who might be using the code later, or maintaining the program in the future.  These docs should contain examples where needed to help other future developers use the module if it is intended to be a library module used by may client modules. 

What NOT to Document
----------------

*Soliciting opinion on this section: do we need to define this??*

Required Pod Sections
---------------------
The following sections should be present at the top of all pm files written:

`=head1 NAME` - The name of the moule and a short blurb.

`=head1 DESCRIPTION` - The description of the module.

`=head1 PARENT MODULES` - A quick blurb on the parent code used (Base/Base2/etc)

`=head1 XXX METHODS` - A header to signal the start of a method section

```
=head1 NAME 

The::Module - Business logic code for some client

=head1 DESCRIPTION

This module implements some custom nominations, has an API, contains ported code from an older base, whatever a programmer might find relevant at a glance.

=head1 PARENT MODULES

L<Chameleon5::Incentive::Base2>
L<Chameleon5::Contrib::Site::ExtendedBase>

=head1 NOMINATION METHODS
```

Per-Method Pod
--------------
Each method following the METHODS header section above should have its own subheader.  The methods should be related to the main `=head1` header they are listed under.  (For example, the method section might be NOMINAITON or GAME).

`=head2 method_name` - What the method does, how to use it.

If there was a method implemented in the code that **overrides** a method in the base/parent code, we should be noting that in the `=head2` section that describes the method.


Optional Pod Sections
---------------------

`=head1 SYNOPSIS` - Quick code example of the usage of the code.

`=head1 SEE ALSO` - References to other code or sites related to this module.  Might include references to other perl modules used to implement this code.

```
=head1 SYNOPSIS

 use The::Module;
 my $mod = The::Module->new( some => { constructor => args } );
 
 # do the thing
 $mod->do_something();

=head1 SEE ALSO

The site or something: L<Link to somewhere|http://site.online-rewards.com> 
``` 