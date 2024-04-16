# To Perl from Python

Python is a general-purpose programming language offering a balanced set of
features ranging from procedural programming through object-oriented features
to functional programming.

## Similarities

As with Python, in Perl:

- A program is a list of statements that are directly executed, with no further
required structure like in C or Java.
- Variables are dynamically typed.
- Regexes are included in the language.
- Common data structures (arrays and dictionaries) are a core part of
the language.
- Integer ranges are built in.
- Mapping and filtering over arrays is included and straightforward.
- Annonymous functions are supported.
- You have a built-in inline documentation tool (called pod).
- There is support for easily calling external programs and collecting their
output.
- The distribution comes with a package manager.
- Modules can call into native C code easily for performance.
- Performance is not bad for an interpreted language.

## Differences

Unlike Python, in Perl:

- Variables are named according to their structure, starting with `$` for scalars,
`@` for arrays, `%` for hashes (the Perl way of saying "dictionary"), and `&` for
subroutines (the Perl way of saying "functions").
- Scope of a variable is declared explicitly, using `my` keyword for local variables
and `our` keyword for global ones.
- Regular braces, not square or curly braces, are used for declaring arrays and
hashes.
- Hash key-value pairs are separated with `=>`, not with `:`.
- Subroutine arguments are handled dynamically and available in an array called
`@_` instead of having named parameters.
- There are no list comprehensions, a combination of `grep` (Perl name for `filter`)
and `map` is used instead.
- Backticks (``) are used to call a program and capture its output (equivalent
of Python's `subprocess.check_output`).
- Object-oriented programming is done indirectly by adding subroutines from
a package to a value with the `bless` statement.

## Examples

Here are a few examples of how to do common Python constructions in Perl.

### Hello world

```python
# Python
print("Hello world!")
```

```perl
# Perl
say "Hello world!";
```

```bash
# Output
Hello world!
```

### Basic list operations

```python
# Python
lst = ["a", "b", "c"]
lst.append("d")

print("List length:", len(lst))
print("First element: ", lst[0])
print("Second-to-last element: ", lst[-2])

print("Entire list:", lst)
print("Slice:", lst[1:3])
```

```perl
# Perl
my @array = ("a", "b", "c");
push "d", @array;

say "List length: ", scalar @array;
say "First element: ", $array[0];
say "Second-to-last element: ", $array[-2];

say "Entire list: ", "@array";
say "Slice: ", "@array[1..2]";
```

```bash
# Output
List length: 4
First element: a
Second-to-last element: c
Entire list: ['a', 'b', 'c', 'd'] # a b c d in Perl
Slice: ['b', 'c'] # b c in Perl
```

### List comprehension
```python
# Python
number_list = [x * 3 for x in range(0, 10) if x % 2 == 0]
for x in number_list:
    print(x)
```

```perl
# Perl
my @number_array = map {$_ * 3} grep {$_ % 2 == 0} 0..9;
for my $x (@number_array) {
    say $x;
}
```

```bash
# Output
0
6
12
18
24
```

### Iterate list with indices
```python
# Python
fun_languages = ["Python", "Perl", "Ruby"]
for idx, language in enumerate(fun_languages):
    print(idx, language)
```
```perl
# Perl
my @fun_languages = ("Python", "Perl", "Ruby");
while (my ($idx, $language) = each @fun_languages) {
    say $idx, " ", $language;
}
```
```bash
# Output
0 Python
1 Perl
2 Ruby
```