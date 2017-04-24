# Online Rewards Perl Style Guide

## Table of Contents
  1. [Whitespace](#whitespace)
  1. [Blocks](#blocks)
  1. [perltidy](#perltidy)
  1. [Variables] (#variables)

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

## Variables

### Declare variables near their usage
Create variable declarations as near to their usage as possible.  For example, don't declare a variable that is not referenced until 25 lines later.
```perl
// bad
my $test_variable;

# ... 25 rows later ...

$test_variable = $self->g('base3/constants/something');

// good
# ... 25 rows of other code ...

my $test_variable = $self->g('base3/constants/something');
```

### Check the existence of linked objects before referencing them
If the object does not exist, referencing one of it's linked objects will blow up.  Check it's existence first.
```perl
// bad
my $registry = $self->_manager('Registry')->get_objects( ... );

$self->tree_append(
    tag  => 'bars',
    data => $registry->foo->bars->as_tree,
);
// good
my $registry = $self->_manager('Registry')->get_objects( ... );

if ($registry->foo && $registry->foo->bars)
{
    $self->tree_append(
        tag  => 'bars',
        data => $registry->foo->bars->as_tree,
    );
}

**[⬆ back to top](#table-of-contents)**
