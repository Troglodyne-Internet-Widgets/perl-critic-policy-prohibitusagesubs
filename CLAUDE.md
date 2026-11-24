# CLAUDE.md

This file is the procedure for work in this repository.

The repository holds one Perl::Critic policy, `ProhibitUsageSubs`. It flags a
hand-written `usage()` sub, because the interface belongs in POD where
`Pod::Usage` can print it. The policy is in
`lib/Perl/Critic/Policy/ProhibitUsageSubs.pm`. Its tests are in `t/main.t`.

## Which skills, and when

The hooks of the perl-slop plugin hold you to the procedure. They refuse an
edit to Perl until `perl-slop:reading-perl` is loaded. They refuse a commit of
Perl until `perl-slop:data-perl`, `perl-slop:testing-perl` and
`perl-slop:reviewing-perl` are loaded, each one after the last commit. A
refusal names the skills that are missing.

Before you answer a question about the code, or look for something in it, load
`perl-slop:reading-perl`. The hooks cannot see that case.

To load a skill is not to apply it. Apply `perl-slop:reviewing-perl` to the
whole diff before you commit.

## Read the code before you change it

A line that looks pointless is usually the scar of something that went wrong
once. The reason is in the commit, not in the file. Read before the first edit.

## Tests

The test passes `-profile => q{}` to Perl::Critic. Without it, Perl::Critic
finds the `.perlcriticrc` of this repository and runs every policy against the
test snippets. Keep the hyphen in `-single-policy`. The form `-single_policy`
is accepted and ignored.

## Finishing a changeset

Do not run `perltidy` or `perlcritic` yourself to decide whether a change is
ready. The pre-commit hook tidies the Perl that you staged, runs perlcritic on
it, compiles it, and runs the tests that the commit can break. If a step fails,
the hook stops the commit and prints the reason. If a test fails, run that file
with `prove -v t/<file>.t` and read the output.

Run `podchecker` on each changed file.

## When something is slow

Use `perl-slop:profiling-perl`. Measure before you conclude anything, and
measure again after a change.

## Commits and pull requests

Make a branch. Do not commit to `master`.

A commit message says what was wrong and why the change fixes it. Write it in
prose and in the imperative mood. The why goes in the message, not in a
comment.

After you make sure that something works, say what you ran and what it
printed. Do not put names of hosts, accounts or customers in a commit message,
a pull request or an issue. `perl-slop:information-security` has the full rule.
