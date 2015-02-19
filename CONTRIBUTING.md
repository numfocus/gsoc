# Contributing

## Mentors

To be a mentor go to [GSoC homepage][GSoC]
and create a profile for you.
If you need help, please check
http://en.flossmanuals.net/melange/creating-and-editing/.

**After you create your profile,
open a issue for this
or send a pull request adding your username at [organization-team.md][OT].**

### Proposal summary

If you have a proposal,
please create a issue for it.

## Students

Projects proposed by mentors are listed at
https://github.com/swcarpentry/gsoc2015/issues.

You are welcome to proposal a project by your own.
If this is the case, please open a issue to discuss your proposal
or send a pull request with it.

### Proposal draft

1.  Fork https://github.com/swcarpentry/gsoc2015

2.  Clone your fork:

    ~~~
    $ git clone https://github.com/username/gsoc2015.git
    ~~~

    where `username` is your GitHub username.

    In my case is `r-gaia-cs`, so I will use

    ~~~
    $ git clone https://github.com/r-gaia-cs/gsoc2015.git
    ~~~

2.  Copy `proposals/skeleton.md` to `proposals/your-name.md`
    where `your` is your last name, all lowercase,
    and `name` is your firts name, all lowercase.

    For example, my name is Raniere Silva so I need to
    copy `proposals/skeleton.md` to `proposals/silva-raniere.md`.

3.  Edit `proposals/your-name.md` filling all sections.

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

7.  Edit `proposals/your-name.md` to address the comments.

8.  Update your pull request:

    ~~~
    $ git commit -a
    $ git push origin master
    ~~~

9.  Back to step 7.

### Final Proposal

Your final proposal need to submitted to [GSoC][]
**before March 27th 19:00 UTC**.

[GSoC]: https://www.google-melange.com/gsoc/homepage/google/gsoc2015
[OT]: organization-team.md
