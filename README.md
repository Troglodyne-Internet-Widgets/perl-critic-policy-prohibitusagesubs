# NAME

Perl::Critic::Policy::ProhibitUsageSubs - Document the interface once, in POD, and let Pod::Usage print it.

# VERSION

version 1.000

# Perl::Critic::Policy::ProhibitUsageSubs

A hand-rolled `usage()` is a second copy of the script's interface, kept in
step with the POD and with `GetOptions` by hope alone:

```perl
sub usage {
    return "Usage: $0 [--name NAME] DOMAIN\n";
}
die usage() unless $domain;
```

Three places now say what the options are, and nothing checks that they agree.
Whichever the reader finds first is the one they believe, and it is reliably
the one that stopped being true two commits ago.

Write it in POD, where `perldoc` and the man page find it too, and let
[Pod::Usage](https://metacpan.org/pod/Pod%3A%3AUsage) print that:

```perl
=head1 SYNOPSIS

    myscript [--name NAME] DOMAIN

=cut

pod2usage( -exitval => 2, -verbose => 1, -input => __FILE__,
           -message => 'No domain passed' ) unless $domain;
```

## PROHIBITED

```perl
sub usage         { ... }
sub _usage        { ... }
sub print_usage   { ... }
sub usage_message { ... }
```

## ALLOWED

Anything else, including calls to `pod2usage` and to a `usage` imported from
elsewhere.  It is writing your own that this is about.

## CONFIGURATION

- `sub_names`

    Space separated list of subroutine names to complain about.  Defaults to
    `usage _usage print_usage usage_message`.

    ```perl
    [ProhibitUsageSubs]
    sub_names = usage _usage help
    ```

## CAVEATS

This goes by the name.  A `sub help_text` that does the same job under another
name goes unnoticed, and a `sub usage` that computes something unrelated to
the command line gets flagged anyway.  The name is the only evidence available
in the source.

## METHODS

### supported\_parameters

`sub_names`, the list of subroutine names to flag.

### default\_severity

SEVERITY\_MEDIUM

### default\_themes

maintenance, documentation

### applies\_to

PPI::Statement::Sub

### violates

Standard [Perl::Critic::Policy](https://metacpan.org/pod/Perl%3A%3ACritic%3A%3APolicy) interface.  Returns a violation for a
declaration of one of the configured names, and nothing for a forward
declaration, which promises no second copy of anything.

# BUGS

Please report any bugs or feature requests on the bugtracker website
[https://github.com/teodesian/perl-critic-policy-prohibitusagesubs/issues](https://github.com/teodesian/perl-critic-policy-prohibitusagesubs/issues)

When submitting a bug or request, please include a test-file or a
patch to an existing test-file that illustrates the bug or desired
feature.

# AUTHORS

Current Maintainers:

- George S. Baugh <teodesian@gmail.com>

# COPYRIGHT AND LICENSE

Copyright (c) 2026 Troglodyne LLC

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:
The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.
THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
