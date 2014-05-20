Pod Cheatsheet
==============
This is intended as a quick reference to Pod commands and formatting codes.  There are other codes which have been left out since they would not be commonly used by us.  See [the perlpod documentation](http://perldoc.perl.org/perlpod.html) or `perldoc perlpod` for more information.

Pod Commands
------------
A command paragraph is used for special treatment of whole chunks of text, usually as headings or parts of lists. All command paragraphs (which are typically only one line long) start with "=", followed by an identifier, followed by arbitrary text that the command can use however it pleases.

These commands should have blank lines above and below so all parsers can read them.

Commonly used commands:

* `=head1 HEADER`, `=head2 SUBHEADER`, `=head3 SUBHEADER`, `=head4 SUBHEADER` - Section Headers
* `=over indentlevel`, `=item LISTITEM`, `=back` - Bullet list generation
* `=pod`, `=cut` - Start and End Pod Blocks
* `=begin FORMAT`, `=end FORMAT` - Explicitly formatted test of type FORMAT (text, html) 

One thing to note is that text in these blocks is usually formatted however the parser wants as regular paragraphs.  If you want the text to be left verbatim (useful for example code) then the first character of the line should be a space or tab.

Examples:

```
=head1 Heading One

Some regular text

  Some::Verbatim->code_example($here);

=head2 A sub-header

Some sub-header explanation

=cut

sub code_here
{
    my $self = shift;
    print "OK this would be code, not pod\n";
    return;
}

=head2 Another sub header

Pod will not show the code between this and the last header

=over 4

=item List Item One

With text

=item List Item Two

 my $Verbatim_Text = 1;

=back

=begin html

<i>Just some regular html here</i>

=end html

Lets end the pod here

=cut
```

Creates the following HTML (via `pod2html`):

```html
<ul id="index">
  <li><a href="#Heading-One">Heading One</a>
    <ul>
      <li><a href="#A-sub-header">A sub-header</a></li>
      <li><a href="#Another-sub-header">Another sub header</a></li>
    </ul>
  </li>
</ul>

<h1 id="Heading-One">Heading One</h1>

<p>Some regular text</p>

<pre><code>  Some::Verbatim-&gt;code_example($here);</code></pre>

<h2 id="A-sub-header">A sub-header</h2>

<p>Some sub-header explanation</p>

<h2 id="Another-sub-header">Another sub header</h2>

<p>Pod will not show the code between this and the last header</p>

<dl>

<dt id="List-Item-One">List Item One</dt>
<dd>

<p>With text</p>

</dd>
<dt id="List-Item-Two">List Item Two</dt>
<dd>

<pre><code> my $Verbatim_Text = 1;</code></pre>

</dd>
</dl>

<i>Just some regular html here</i>

<p>Lets end the pod here</p>
```


Pod Formatting Codes
--------------------

A formatting code starts with a capital letter (just US-ASCII [A-Z]) followed by a "<", any number of characters, and ending with the first matching ">".  You would use a formatting code to control text rendering inside a block defined in a command above.

Commonly used formatting codes:

* `I<text>` - *italic* text
* `B<text>` - **bold** text
* `C<code>` - `code formatting`
* `L<link>` - [hyperlink](http://online-rewards.com)
* `E<code>` - HTML entity code

Examples:

```
The following is I<italic>
The following is B<bold>
The following is C<code formatted>
The following is a L<Link to our site|http://online-rewards.com/>
The following is a L<Chameleon5::Site::Base> perldoc                                                                                                                                
The following is a link to a man page L<crontab(1)/"DESCRIPTION">
This is also a man page link L<ls(1)>
This following uses escapes for gt/lt Some Person, E<lt>some.person@online-rewards.comE<gt>
Here are some html entities: E<eacute>, E<laquo>, E<0x00A1>, E<170>
```

Creates the following HTML (via `pod2html`):

```html
The following is <i>italic</i>
The following is <b>bold</b>
The following is <code>code formatted</code>
The following is a <a href="http://online-rewards.com/">Link to our site</a>
The following is a <a>Chameleon5::Site::Base</a> perldoc
The following is a link to a man page <a href="http://man.he.net/man1/crontab">&quot;DESCRIPTION&quot; in crontab(1)</a>
This is also a man page link <a href="http://man.he.net/man1/ls">ls(1)</a>
This following uses escapes for gt/lt Some Person, &lt;some.person@online-rewards.com&gt;
Here are some html entities: &eacute;, &laquo;, &iexcl;, &ordf;
```

The above also rensers on the command line correctly using `perldoc`
