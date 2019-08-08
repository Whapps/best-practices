# Online Rewards Perl Style Guide

## Table of Contents
  1. [Whitespace](#whitespace)
  1. [Blocks](#blocks)
  1. [perltidy](#perltidy)
  1. [Subroutine Lengths](#Subroutine Lengths)
  1. [Delimiters for Escaping Strings with Quotes](#Delimiters for Escaping Strings with Quotes)
  1. [Exception vs Die vs Returning Strings](#Exception vs Die vs Returning Strings)
  1. [Verbosity Level of Comments](#Verbosity Level of Comments)
  1. [POD Coverage and Guidelines](#POD Coverage and Guidelines)
  1. [Usage of unless()](#Usage of unless)
  1. [Adding Features to Base](#Adding Features to Base)
  

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

## Subroutine Lengths

From the code review on 8 May 2014, we should have a convention for how long we attemt to keep routine lengths.

**[⬆ back to top](#table-of-contents)**

## Delimiters for Escaping Strings with Quotes

From the code review on 8 May 2014, we should have a standard delimiter we fall back on for strings that have " or ' quote characters.

**[⬆ back to top](#table-of-contents)**

## Exception vs Die vs Returning Strings

From the code review on 8 May 2014, we should decide if Exception classes are what are used across the board, or whether die/cluck/confess/returning a string can ever be legit usage.

**[⬆ back to top](#table-of-contents)**

## Verbosity Level of Comments

From the code review on 8 May 2014, we should have an agreement on how verbose is too verbose.  

**[⬆ back to top](#table-of-contents)**

## POD Coverage and Guidelines

From the code review on 8 May 2014, we should have a convention for minimum POD coverage on POD in code.  We also need a cheat sheet of some sort that we can share out.

**[⬆ back to top](#table-of-contents)**

## Usage of unless()

From the code review on 8 May 2014, we have no quiteline on where unless() is confusing and where its OK.

**[⬆ back to top](#table-of-contents)**

## Adding Features to Base

From the code review on 8 May 2014, we should have a process documented on how to create features for base code in the public eye.  Likely with pull requests, etc.

**[⬆ back to top](#table-of-contents)**