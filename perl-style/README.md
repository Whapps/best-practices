# Online Rewards Perl Style Guide

## Table of Contents
  1. [Whitespace](#whitespace)
  1. [Blocks](#blocks)
  1. [Perl::Tidy](#Perl::Tidy)

## Whitespace

  - Use soft tabs set to 4 spaces. [Sorry, tabs lost the war.](http://sideeffect.kr/popularconvention) [Even perltidy says to avoid them](http://perltidy.sourceforge.net/perltidy.html#tabs).
    
**[⬆ back to top](#table-of-contents)**

## Blocks
  - Use [Allman (or BSD) style](https://en.wikipedia.org/wiki/Indent_style#Allman_style) indentation. There is no good reason for this, just a decade of legacy code written in this format.
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
    
## Perl::Tidy
Perltidy is a perl script indenter and reformatter. It is a tool, not a gospel, so its output should be treated pragmatically rather than dogmatically.

Perltidy can be installed via:
```shell
cpanm Perl::Tidy
```

Included in this folder is a .perltidyrc file that contains the Online Rewards suggested defaults.

**[⬆ back to top](#table-of-contents)**