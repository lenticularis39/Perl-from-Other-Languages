# Perl from Other Languages

Perl cannot deny its origin in the classic ways of Unix. If you're familiar
with Unix and come across some Perl code for the first time, you'll probably
think it's some kind of weird mix between C and shell with some awk and sed
sauce thrown into it. But Perl is not just a Swiss army knife for text
processing: it is an elegant general-purpose programming language that does its
best to provide you, the programmer, with means to express your ideas in code
as seamlessly as possible, whatever languages you already know. It's also not
bad in extensibility: there are many third-party modules essentially extending
the base functionality of the language.

This document contains two sections. The first one shows you how to do common
constuctions from another language in Perl, staying as close to the original
style of the other language as possible: there's more than one way to do it, and
why not start with the way most familiar to you? The second section goes over
the most important features of Perl and compared them to what is found in other
languages you might be familiar with.

This document is inspired by a similar summary for Ruby:
https://www.ruby-lang.org/en/documentation/ruby-from-other-languages/.


## Language X to Perl

- To Perl from Python

## Important Features

Here are some important features that set Perl apart from most programming
languages.

### Language version declaration

Sometimes, adding a new feature into a language makes existing code behave
in a different way. Consider, for example, this piece of code:

```perl
sub say {
    print "Saying @_\n";
}

say "Hello";
```

In Perl 5.9, `say` is a regular identifier, and the program works as expected:

```bash
# Output
Saying Hello
```

However, in Perl 5.10, a built-in function named `say` was added that takes over
preference over a declared subroutine. In order not to break backwards
compatiblity, Perl 5.10 and higher by default behaves just like no `say` keyword
was added:

```bash
$ perl5.38.0 -e '
sub say {
    print "Saying @_\n";
}

say "Hello";
'
Saying Hello
```

To use the new `say` built-in, the programmer has to acknowledge they want to use Perl 5.10 features:

```bash
$ perl5.38.0 -e '
use v5.10;

sub say {
    print "Saying @_\n";
}

say "Hello";
'
Hello
```

This has the additional benefit of throwing an explicit error in case your
interepreter is too old:

```bash
$ perl5.38.0 -e '
use v6;

"New shiny stuff!".say;
'
Perl v6.0.0 required--this is only v5.38.0, stopped at -e line 2.
BEGIN failed--compilation aborted at -e line 2.
$ perl6 -e '
use v6;

"New shiny stuff!".say;
'
New shiny stuff!
```

Compare this to a mysterious error (or even worse, an unexpected behavior) in
case of new syntax being used in an old interpreter:

```bash
$ perl5.38.0 -e '"New shiny stuff!".say;'
# No output, `.` is interpreted as string concatenation in Perl 5
```

Note: All code in the remained of the document is assumed to be prefixed 
with `use v5.36;`:

```perl
use v5.36;

# Example code
```

### Sigils

Variables in Perl are named and accessed according to the data structure they
represent. They are five built-in data structures in Perl:

```perl
$scalar     # Plain value, no data structure
@array      # Holds multiple values in order, indexed from 0
%hash       # Holds key-value pairs
&subroutine # Holds executable code
*typeglob   # Holds symbol table references (used for aliasing variables)
```

When referencing the values, the sigil corresponding to the value type is used,
not the sigil corresponding to the variable declaration:

```perl
my @array = (1, 5, 9, 42); # An array
say @array; # Print the entire array
say $array[1]; # Print second element of array (which is itself a scalar, not
               # an array, hence, it uses $, not @)

my %hash = (one => 1, two => 2); # A hash
say $hash{"two"}; # A value of the hash
```

Implicit conversion happens when a different kind of data structure is expected,
for example when using an array in scalar context:

```perl
my @array = (1, 2, 3, 4);
if (@array > 2) {
    # The array is cast to an integer, its length
    say "A long array";
}
```
```bash
# Output
A long array
```

Cast to scalar can also be explicitely enforced:
```perl
my @array = (1, 2, 3, 4);
say "Array: ", @array;
say "Array length: ", scalar @array;
```
```bash
# Output
Array: 1234
Array length: 4
```

### Default variable

Most built-in Perl commands that take an argument default to an implicit
variable `$_` if no argument is provided. For example:

```perl
my @array = ("Hello", "Hello world", "Hello Perl");
for (@array) {
    # Iterating over @array, putting the current element into the default
    # variable $_
    s/Hello/Hi/; # sed-like substitution command, operating again on $_
    say; # Print $_ with new line
}
```
```bash
# Output
Hi
Hi world
Hi Perl
```

This makes it unnecessary to create and pass a separate variable where it is
clear from the context. Compare the example above to the more verbose syntax,
common in other programming languages:

```perl
my @array = ("Hello", "Hello world", "Hello Perl");
for my $x (@array) {
    $x =~ s/Hello/Hi/;
    say $x;
}
```

You might have heard that Perl is inspired among other things by natural 
language: you can think of `$_` as a pronoun "it", or, in this case, "the
element".

There is also a default array variable `@_`, used for subroutine arguments:

```perl
sub describe_args {
    # Arguments are stored in variable @_
    say "Got $#_ arguments:";
    for my $i (0..$#_) {
        say "Argument $i: $_[$i]";
    }
}

describe_args(5);
describe_args("Hello", "world", "and", "peace", "be", "upon", "you");
```
```bash
# Output
Got 0 arguments:
Argument 0: 5
Got 6 arguments:
Argument 0: Hello
Argument 1: world
Argument 2: and
Argument 3: peace
Argument 4: be
Argument 5: upon
Argument 6: you
```
