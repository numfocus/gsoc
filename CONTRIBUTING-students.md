# Contributing Guide for Students

## Getting Started Early

The 2016 student application window is March 14th to 25th.
**We don't know if NumFOCUS will be accepted to participate until 29 February**
but if you are interested in start work early on your application,
[the Python Software Foundation Wiki](https://wiki.python.org/moin/SummerOfCode/2016#Students)
lists a few things you can do to get started in free and open source software.

## Getting Started

**Please use the instructions of the project/organization
you are interested in work with.**

## biocore

Read [default instructions](#default-instructions).

## DyND

Read [default instructions](#default-instructions).

## Gensim

Read [default instructions](#default-instructions).

## JuliaOpt

Read [default instructions](#default-instructions).

## matplotlib

Read [default instructions](#default-instructions).

## Pandas

Read [default instructions](#default-instructions).

## Software Carpentry

Read [default instructions](#default-instructions).

## Default Instructions

**This is a general guide.
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

**You should avoid add some personal/private information
like phone number and address, at your proposal.**

1.  Fork https://github.com/numfocus/gsoc

2.  Clone your fork:

    ~~~
    $ git clone https://github.com/username/gsoc2015.git
    ~~~

    where `username` is your GitHub username.

    In my case is `rgaiacs`, so I will use

    ~~~
    $ git clone https://github.com/rgaiacs/gsoc.git
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
**before March 25th 19:00 UTC**
as a PDF file.

You can use [Pandoc][] to convert your proposal in Markdown
to PDF **if you have LaTeX installed on your machine**
or convert to HTML and print it to PDF using your web browser.
For example, you can

~~~
$ pandoc -f markdown -t pdf YYYY/proposals/your-name.md
~~~

[IL]: 2015/ideas-list.md
[issues]: https://github.com/numfocus/gsoc/issues
[GSoC]: http://summerofcode.withgoogle.com/
[ML]: organization/team.md
[Pandoc]: http://pandoc.org/
