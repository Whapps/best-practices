# Online Rewards Perl Style Guide

## Table of Contents
  1. [Whitespace](#whitespace)
  1. [Blocks](#blocks)
  1. [Comments](#comments)
  1. [perltidy](#perltidy)

## Whitespace

### Use soft tabs set to 4 spaces.
[Sorry, tabs lost the war.](http://sideeffect.kr/popularconvention) [Even perltidy says to avoid them](http://perltidy.sourceforge.net/perltidy.html#tabs).

### Avoid lines longer than 80 columns.

It may seem like a dated convention, but it is a convention that is [still widely followed today.](http://sideeffect.kr/popularconvention) Generally, 80 columns will let someone on a small screen (like a 13" non-retina Macbook) fully view 2 files side by side.

There are cases when a long line is better overzealous line breaking. Use your personal judgement when breaking this rule.

### Break immediately after an opening token if a line break is necessary at all.

Further, put the closing token on its own line and always include a trailing comma. This maximizes the ease with which the list can be modified.
```perl
// bad
my %cincinnati_reds = ( catcher => 'Devin Mesoraco',
    shortstop => 'Zack Cozart',
);

// worse
my %cincinnati_reds = ( catcher => 'Devin Mesoraco',
                        shortstop => 'Zack Cozart',
);

// worst
my %cincinnati_reds = ( catcher => 'Devin Mesoraco',
                        shortstop => 'Zack Cozart' );

// good
my %cincinnati_reds = (
    catcher => 'Devin Mesoraco',
    shortstop => 'Zack Cozart',
);
```

**[⬆ back to top](#table-of-contents)**

## Blocks

### Use [Allman (BSD) style](https://en.wikipedia.org/wiki/Indent_style#Allman_style) indentation.

This may not be the most popular indent style, but writing in this manner will keep new code consistent with nearly a decade's worth of legacy code.
```perl
// bad
if (test) {
    return false;
}

// good
if (test)
{
    return false;
}
```

### Always return via an explicit ```return```

Since perl will return the value of the last expression evaluated in a subroutine if return is not passed, explicit returns should be used to prevent unexpected return values. This also improves readability in subs where there are multiple returns.

```
# bad
sub do_something
{
    my $thing = 45;
}

# good
sub do_something
{
    my $thing = 45;
    return $thing;
}
```

**[⬆ back to top](#table-of-contents)**


## Comments

### Use POD sections to comment above each method

That way later we can just ```perldoc Your::Module``` and get a user interface guide.

```
=head2 some_method

 Takes no argument.  This is a reason you would
 call this method.  Here is what you might
 expect it to do and/or return.

=cut

sub some_method
{
    my $self = shift;
    print "Doing something\n";
}
```

Another benefit for using POD to comment on your methods is that pod2html can then generate
pretty versions of your docs.

But don't rely just on the POD sections to comment your code.  POD sections are more useful to programmers who are coming into your codebase later to use it.  Any sufficiently complicated section of code should also be commented inline.

**[⬆ back to top](#table-of-contents)**

## perltidy
`perltidy` is a perl script indenter and reformatter. It is a tool, not a gospel, so its output should be treated pragmatically rather than dogmatically.

`perltidy` can be installed via:

```shell
$ cpanm Perl::Tidy
```

Included in this folder is a .perltidyrc file that contains the Online Rewards suggested defaults.

When faced with poorly formatted code feel free to use perltidy but always commit the formatting changes separate from any other changes. Committing formatting changes along with other changes can cause complications when diff-ing.

**[⬆ back to top](#table-of-contents)**
