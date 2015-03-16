# Contributing Guide for Students

Please use the instructions of the project/organization
you are interested in work with.

## Julia

Read [default instructions](#default-instructions).

## Software Carpentry

Read [default instructions](#default-instructions).

## Sympy

Read https://github.com/sympy/sympy/wiki/GSoC-2015-Application-Template.

## Default Instructions

**This are a general guide.
Projects/Organizations can have different instructions
and you must follow their instructions.**

Projects proposed by mentors are listed at our [ideas list][IL] and
questions can be asked at [our issue tracker][issues].

You are welcome to propose your own project. If you wish to do so, please
open an issue to discuss your proposal and/or send a pull request with it.

If you choose to propose your own project idea you will need to find
a mentor for the project
and you can start with [our mentor list][ML].
**Proposals without a mentor will not be considered.**

### Proposal draft

1.  Fork https://github.com/numfocus/gsoc

2.  Clone your fork:

    ~~~
    $ git clone https://github.com/username/gsoc2015.git
    ~~~

    where `username` is your GitHub username.

    In my case is `r-gaia-cs`, so I will use

    ~~~
    $ git clone https://github.com/r-gaia-cs/gsoc.git
    ~~~

2.  Copy `YYYY/proposals/skeleton.md` to `YYYY/proposals/your-name.md`
    where `YYYY` is the currently year, `your` is your last name, all lowercase,
    and `name` is your firts name, all lowercase.

    For example, my name is Raniere Silva so I need to
    copy `YYYY/proposals/skeleton.md` to `YYYY/proposals/silva-raniere.md`.

3.  Edit `YYYY/proposals/your-name.md` filling all sections.

    If you need you can create new sections.

    The text in a few sections are reminders for you
    when writing your proposal.

4.  Commit your changes:

    ~~~
    $ git add proposals
    $ git commit
    ~~~

5.  Update it to GitHub:

    ~~~
    $ git push origin master
    ~~~

6.  Create a pull request.

7.  Edit `YYYY/proposals/your-name.md` to address the comments.

8.  Update your pull request:

    ~~~
    $ git commit -a
    $ git push origin master
    ~~~

9.  Back to step 7.

### Final Proposal

Your final proposal must be submitted to [GSoC][]
**before March 27th 19:00 UTC**.

You can use [Pandoc][] to convert your proposal in Markdown
to HTML and paste the HTML at [GSoC][].
For example, you can

~~~
$ pandoc -f markdown -t html YYYY/proposals/your-name.md | xclip -selection clipboard
~~~

and `CTRL+v` your proposal at [GSoC][].

[IL]: 2015/ideas-list.md
[issues]: https://github.com/numfocus/gsoc/issues
[GSoC]: https://www.google-melange.com/gsoc/homepage/google/gsoc2015
[ML]: organization/team.md
[Pandoc]: http://pandoc.org/
