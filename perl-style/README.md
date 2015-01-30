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

## Proposed change to eval calls

While playing with perlcritic I saw the following:


<pre>
    A common idiom in perl for dealing with possible errors is to use `eval'
    followed by a check of `$@'/`$EVAL_ERROR':

        eval {
            ...
        };
        if ($EVAL_ERROR) {
            ...
        }

    There's a problem with this: the value of `$EVAL_ERROR' can change
    between the end of the `eval' and the `if' statement. The issue is
    object destructors:

        package Foo;

        ...

        sub DESTROY {
            ...
            eval { ... };
            ...
        }

        package main;

        eval {
            my $foo = Foo->new();
            ...
        };
        if ($EVAL_ERROR) {
            ...
        }

    Assuming there are no other references to `$foo' created, when the
    `eval' block in `main' is exited, `Foo::DESTROY()' will be invoked,
    regardless of whether the `eval' finished normally or not. If the `eval'
    in `main' fails, but the `eval' in `Foo::DESTROY()' succeeds, then
    `$EVAL_ERROR' will be empty by the time that the `if' is executed.
    Additional issues arise if you depend upon the exact contents of
    `$EVAL_ERROR' and both `eval's fail, because the messages from both will
    be concatenated.

    Even if there isn't an `eval' directly in the `DESTROY()' method code,
    it may invoke code that does use `eval' or otherwise affects
    `$EVAL_ERROR'.
</pre>

It suggests the following be done to the eval to make sure it is trapped correctly:

<pre>
    The solution is to ensure that, upon normal exit, an `eval' returns a
    true value and to test that value:

        # Constructors are no problem.
        my $object = eval { Class->new() };

        # To cover the possiblity that an operation may correctly return a
        # false value, end the block with "1":
        if ( eval { something(); 1 } ) {
            ...
        }

        eval {
            ...
            1;
        }
        or do {
                # Error handling here
        };
</pre>

I played with this syntax and actually find it to make sense.
