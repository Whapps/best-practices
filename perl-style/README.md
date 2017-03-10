# Online Rewards Perl Style Guide

## Table of Contents
  1. [Whitespace](#whitespace)
  1. [Blocks](#blocks)
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

## Rose::DB
TODO: description

### Invoking Rose
BaseJob scripts:
C5 Base module and site inherits thereof:
Anywhere else in a C5 site:

### Query ambiguity, or how to avoid `->[0]`
You may come across some old code with Rose queries that look like this:
```perl
# bad
my $registry = $self->_manager{'Registry'}->get_objects(
    query => { external_id => $external_id },
)->[0];
```
This runs the query and takes the first result.  There is no accounting for the possibility of multiple results, which should be a fatal error in a case like this.
Rose knows what columns are unique, so take advantage of that:
```perl
# good
my $registry = $self->_rose{'Registry'}->new(
    external_id => $external_id,
)->load;
```
This is unambiguous, and easier to read.  Rose will give you exactly one result, and throw an error if it can't.  The arguments to `new()` must comprise a unique key or Rose will throw an error.

If no result is an expected possibility that needs to be handled, `load_speculative` can be used:
```perl
# good
my $registry = $self->_rose{'Registry'}->new(
    external_id => $external_id,
);
unless ( $registry->load_speculative )
{
    # do stuff to handle the lack of a result
    $registry->init(
        external_id => $external_id,
    )->save;
}
```

`->[0]` is only valid when combined with a sort, where we specifically *do* want the first result of a set:
```perl
# good
my $registry = $self->_manager{'Registry'}->get_objects(
    query => {
        locked => 0,
        archived => 0,
    },
    sort_by => 'last_login_date',
)->[0];
```

**[⬆ back to top](#table-of-contents)**
