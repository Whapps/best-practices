# Online Rewards Perl Style Guide

## Table of Contents
  1. [Whitespace](#whitespace)
  1. [Blocks](#blocks)
  1. [Names](#names)
  1. [perltidy](#perltidy)
  1. [Encoding](#encoding)
  1. [Rose::DB](#rosedb)

## Whitespace

### Use soft tabs set to 4 spaces.
[Sorry, tabs lost the war.](http://sideeffect.kr/popularconvention) [Even perltidy says to avoid them](http://perltidy.sourceforge.net/perltidy.html#tabs).

### Avoid lines longer than 80 columns.

It may seem like a dated convention, but it is a convention that is [still widely followed today.](http://sideeffect.kr/popularconvention) Generally, 80 columns will let someone on a small screen (like a 13" non-retina Macbook) fully view 2 files side by side.

There are cases when a long line is better than overzealous line breaking. Use your personal judgement when breaking this rule.

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

### Vertically align corresponding code.

When a section of repeating code, such as a large hash assignment, has more than one 'column', align it.  It makes your code look and read much better.
```perl
# bad
my %user = (
    first_name => $first_name,
    last_name => $last_name,
    email => $email,
);

# good
my %user = (
    first_name => $first_name,
    last_name  => $last_name,
    email      => $email,
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

## Names

### Filenames should be hyphen-separated.
bad: `some_cron_script.pl`
good: `some-cron-script.pl`

### Variable names should normally be lowercase, underscore\_separated.
Unless they are `$CONTANT_VARIABLES`.

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

## Encoding

### JSON
Never use `to_json` or `from_json`, as they are not unicode-aware.
`encode_json` and `decode_json` are preferred.

### utf8 and UTF-8
Perl internally uses `utf8` encoding for most string scalars.  However, it's important to note that this is not quite the same thing as the [common](https://en.wikipedia.org/wiki/UTF-8), [strictly-defined](https://tools.ietf.org/html/rfc3629) `UTF-8`.
`utf8` is a superset of `UTF-8`.  Specifically, `utf8` allows any 32-bit code point, while `UTF-8` allows only valid codepoints as defined in the UTF-8 encoding standard.
This situation creates some caveats to be aware of, mainly with regard to how invalid UTF-8 is handled:
```perl
my $string = "abc\x{110000}\n";
print $string;   # warning: "Wide character in print"
# output: "abc????", chars: 61 62 63 110000
```
`\x{110000}` is not a valid character, but perl accepts and handles it anyway.  `print` only warns about it as the character is sent outside perl.

There are different ways to handle getting proper UTF-8 for output, hashing, or encryption:
```perl
# BAD
utf8::encode($string);  # in-place conversion (never use this)
$string = encode('utf8',$string);  # this does the same thing
print $string;
# output: "abc????", chars: 61 62 63 f4 90 80 80
```
'encoding' with perl's utf8 just gives you bytes representing perl's internal storage of the characters.  `\x{110000}` wasn't valid UTF-8 to begin with, so we just get garbage.
So let's try encoding to proper UTF-8 specifically:
```perl
# BAD
$string = encode('UTF-8', $string);
print $string;
# output: "abc�" chars: 61 62 63 ef bf bd
```
This gives us valid UTF-8, but where'd that [`\x{efbfbd}`](http://www.fileformat.info/info/unicode/char/0fffd/index.htm) character come from?  By default, `encode` automatically replaces any invalid characters.  In almost all cases, we would prefer that it didn't.  Fortunately, this is [very configurable](http://perldoc.perl.org/Encode.html#Handling-Malformed-Data):
```perl
# GOOD
$string = encode('UTF-8', $string, 1);
# dies: "\x{110000}" does not map to utf8
```

**[⬆ back to top](#table-of-contents)**

## Rose::DB

### Query ambiguity, or how to avoid `->[0]`
You may come across some old code with Rose queries that look like this:
```perl
# bad
my $registry = $self->_manager{'Registry'}->get_objects(
    query => { external_id => $external_id },
)->[0];
die "not found" unless $registry;
```
This is ambiguous!  It runs the query and takes the first result, which will be at random if there's more than one.

A better appraoch is to use `->new` and `->load` to guarantee no more than one result:
```perl
# good -- dies if no result
my $registry = $self->_rose{'Registry'}->new(
    external_id => $external_id,
)->load;

# --or--

# good -- doesn't die if no result
my $registry = $self->_rose{'Registry'}->new(
    external_id => $external_id,
);
unless ( $registry->load_speculative )
{
    # do stuff to handle the lack of a result
}
```
The arguments to `new` must include or comprise at least one unique key as known to Rose::DB, so tables need to be structured correctly, with unique constraints where appropraite.
Thus this approach also helps enforce good DB design.

#### The exception
If the order of results is known, as when using a `sort_by`, then `->[0]` is not ambiguous:
```perl
# good - the result is not random
my $registry = $self->_manager{'Registry'}->get_objects(
    query => {
        locked => 0,
        archived => 0,
    },
    sort_by => 'last_login_date DESC',
)->[0];
```

### init
When setting multiple properties of a Rose::DB object, `init` can help make things cleaner:
```perl
# messy
$registry->first_name($args{first_name});
$registry->last_name($args{last_name});
# etc etc
$registry->email($args{email});

# better
$registry->init(
    first_name => $args{first_name},
    last_name  => $args{last_name},
    # etc etc
    email      => $args{email},
);
```

**[⬆ back to top](#table-of-contents)**
