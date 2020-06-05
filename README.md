
<!-- README.md is generated from README.Rmd. Please edit that file -->

# extrachecks

<!-- badges: start -->

<!-- badges: end -->

This is a place to dump extra ad-hoc checks that CRAN does that are not
checked for by `devtools::check()`. Some of them are generally useful,
some of them are highly specific, but all of them are reasons that an R
package has been rejected by CRAN. The hope is that by making them
public, we can lower the number of rejections by making package
developers more informed.

## Problem 1

You have an un-exported function that you wrote a roxygen example
section for.

<details>

<summary>Resolution</summary>

If you have written a roxygen example section for un-exported functions,
your example section must call those functions with `:::` like
`pkg:::my_fun()`.

Alternatively, you can use the roxygen tag `@noRd` to suppress the
creation of the `.Rd` file.

</details>

## Problem 2

You used `\dontrun{}` in an example and got a note about that.

<details>

<summary>Resolution</summary>

`\dontrun{}` should only be used if the example really cannot be
executed (e.g. because of missing additional software, missing API keys,
…) by the user. If you want to include an example that errors, and you
want to show the error, wrap the call in `try()`.

Sometimes it is useful to create a custom predicate function
(e.g. `googlesheets4::sheets_has_token()`) that tests for a
prerequisite. Then such examples can be placed inside an `if () {...`
instead of `\dontrun{}`. Instead of a custom predicate, sometimes
`interactive()` can be used as the condition.

</details>

## Problem 3

You have exported functions that don’t have return value documentation.

<details>

<summary>Resolution</summary>

This is a fairly new check that CRAN is being much stricter on. You must
provide return value documentation for all exported functions now. If
you use roxygen2, use the tag `@return`.

This note is also applicable if your function has no return value: “If a
function does not return a value, please document that too,
e.g. `\value{None}`.”

</details>

## Problem 4

You have exported functions that don’t have examples.

<details>

<summary>Resolution</summary>

This is similar to the problem about return value documentation, but
slightly less strict. If your exported function has a meaningful return
value, then it will almost definitely require an examples section. Use
the roxygen2 tag `@examples` to create one.

I have seen exceptions with functions that are used for their side
effects. For example, `hardhat::create_modeling_package()` creates a new
directory, which you would not want to include in an example section
(which CRAN runs in their regular checks). I didn’t include any examples
there, and it was accepted.

</details>

## Problem 5

Your package DESCRIPTION Title is flagged.

<details>

<summary>Resolution</summary>

There can be a number of problems here:

  - You must use title case with package Titles, generally capitalizing
    all words except articles like ‘a’ and ‘the’.

  - I’ve been flagged for a “redundant” title. I had: “A Toolkit for the
    Construction of Modeling Packages” which was flagged since “Toolkit
    for” seemed redundant. I changed it to “Construct Modeling Packages”
    and was accepted.

  - You generally have to put all software / package names in quotes.
    For example, the riingo package is an interface to Tiingo’s stock
    price api:
    <https://github.com/business-science/riingo/blob/a19c662d9a2acb526a15d119e00afcd3fdc7c24c/DESCRIPTION#L3>

</details>

## Problem 6

Your package DESCRIPTION Description is flagged.

<details>

<summary>Resolution</summary>

There can be a number of problems here:

> The Description field is intended to be a (one paragraph) description
> of what the package does and why it may be useful. Please elaborate.

In this case my description was 1 sentence, which I had to expand into a
3-4 sentence paragraph with a broader description of the types of
problems the package intended to help with.

> Please only double quote publication titles in the description of the
> DESCRIPTION file.

In this case I previously had a sentence that contained: ‘…, like “the
first Monday of December”’. I just removed the double quotes.

</details>

## Problem 7

You get asked if there are any “references describing the methods in
your package.”

<details>

<summary>Resolution</summary>

This comment normally comes as the following standard block of text:

> If there are references describing the methods in your package, please
> add these in the description field of your DESCRIPTION file in the
> form authors (year) \<doi:…\> authors (year) \<arXiv:…\> authors
> (year, ISBN:…) or if those are not available: \<https:…\> with no
> space after ‘doi:’, ‘arXiv:’, ‘https:’ and angle brackets for
> auto-linking. (If you want to add a title as well please put it in
> quotes: “Title”)

If there are no references, just nicely reply to the email and tell them
that you don’t have any. If this was the only note then it should be
accepted soon after, without going through another full review.

I have not found a good way to get them to accept a new package without
getting this note on the first round of checks. Possibly you could
include a preemptive note about it in your `cran-comments.md` file,
explaining that there are no references for the package.

</details>

## Problem 8

You get asked about the LICENSE year.

<details>

<summary>Resolution</summary>

I worked on a package in 2019, and then sent it in in 2020. I got the
following question back:

> Should the year in the LICENSE file be updated?

I updated the license year to 2020 and resubmit the package in. I then
nicely replied directly to my reviewer and thanked them for catching the
year discrepency, and then asked them if they could help me push the
package through without needing another review, since that was the only
change that had to be made.

</details>

## Problem 9

You get asked about being the copyright holder (cph).

<details>

<summary>Resolution</summary>

Submitted by @dirkschumacher, who got this comment:

> You also seem to be a copyright holder \[cph\]. Please add this
> information to the <Authors@R> field.

Even if you are the only author and no other copyright information is
given, always add a \[cph\] role to your Authors field.

</details>

## Problem 10

You get told not to comment out code in your `@examples` section.

<details>

<summary>Resolution</summary>

I had originally commented out some code in an example that would
otherwise modify the global state. I wanted to talk about the code
without having the user accidentally run it. I received the following
message on submission:

> Examples/code lines in examples should never be commented out. Ideally
> find toy examples that can be regularly executed and checked. Lengthy
> examples (\> 5 sec), can be wrapped in . If you don’t want your code
> to be executed but still visible to the user, use .

I didn’t want any of these options, so I removed the code from the
examples section entirely and just mentioned it in the `@details`
section instead.

</details>
