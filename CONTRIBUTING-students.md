<!-- markdown-toc start - Don't edit this section. Run M-x markdown-toc-generate-toc again -->
**Table of Contents**

- [Contributing Guide for Students](#contributing-guide-for-students)
    - [Getting Started Early](#getting-started-early)
    - [Am I experienced enough?](#am-i-experienced-enough)
    - [Our Expectations From Students](#our-expectations-from-students)
    - [Default Instructions](#default-instructions)
        - [How to write a great Proposal](#how-to-write-a-great-proposal)
        - [Dividing your project](#dividing-your-project)
        - [How to estimate time needed for development](#how-to-estimate-time-needed-for-development)
        - [Proposal draft](#proposal-draft)
        - [Final Proposal](#final-proposal)
- [Organization Instructions](#organization-instructions)

<!-- markdown-toc end -->


# Contributing Guide for Students

## Getting Started Early

The 2017 student application window is March 20th to April 3rd.
[The Python Software Foundation Wiki](https://wiki.python.org/moin/SummerOfCode/2016#Students)
lists a few things you can do to get started in free and open source software.

## Am I experienced enough?

The answer is generally: **Yes**. 
We value creativity, intelligence and enthusiasm above specific knowledge of the libraries or algorithms we use. 
We think that an interested and motivated student who is willing to learn is more valuable than anything else.
The range of available projects should suit people with different backgrounds.
At the same time if you have experience using your project of choice or one of it's dependencies (e.g. language) make sure to let us know about that as well. [FLOSS manual](http://write.flossmanuals.net/gsocstudentguide/am-i-good-enough/) gives a good overview of this part for GSoC.

## Our Expectations From Students

**Communication**

- Write a short report for us every second week, this can be a blog post.
- Commit early and commit often! Push to a public repository (e.g. github) so that we can see and review your work.
- Actively work on our project timeline and communicate with us during the community bonding period.
- Communicate every working day with your mentor. Preferably in public using the standard channels of your project.
- If there is a reason why you can't work or can't contact us on a regular basis please make us aware of this.
- If you don't communicate with us regularly we will fail you.

**Evaluations**

- Set a realistic goal for all evaluation deadlines. If you fail to meet your own goal we are more likely to fail you in the evaluations.
- Create at least one public commit that has been reviewed before each evaluation.
- Have at least one commit merged into the current development branch of your project at the end of the summer to pass the final evaluation.
- The last point is a hard requirement. Make sure that your time plan includes time for the review process.


## Default Instructions

**This is a general guide.
Projects/Organizations can have different instructions
and you must follow [their instructions](#organization-instructions).**

Projects proposed by mentors are listed at our [ideas list][IL] and
questions can be asked at [our issue tracker][issues].

You are welcome to propose your own project. If you wish to do so, please
open an issue to discuss your proposal and/or send a pull request with it.

If you choose to propose your own project idea you will need to find
a mentor for the project
and you can start with [our mentor list][ML].
**Proposals without a mentor will not be considered.**

### How to write a great Proposal

Firstly, think about your choice of project carefully, you're going to be doing it for a couple of months, so it's important that you choose something you're going to enjoy. Once you've made your mind up:

1. Make sure you've thought about the project and understand what it entails
2. Contact us early! The earlier you contact us the earlier you will be able to get feedback from us to improve your application
3. Don't be afraid to come up with original solutions to the problem
4. Don't be afraid to give us lots of detail about how you would approach the project

Overall, your application should make us believe that you are capable of completing the project and delivering the functionality to our users. If you aren't sure about anything, get in touch with us, we're happy to advise you.

### Dividing your project

We require that all of our students have at least one commit in the development branch of your project before the end of the summer. The best way to achieve this is to divide your project into small self contained subprojects and plan to merge at least one of them around the phase 2 evaluations. We also require you to have at least one commit reviewed by your mentor before each evaluation, the division of the project will help with this too. But don't worry about it we are sure that you will create dozens if not hundreds of commits for your mentors review over the summer.

During your summer you'll likely encounter bugs in your project or find code that can be refactored to help you implement your ideas. You can also immediately fix them and help us all out. This has several advantages. All your pull request will only concentrate on specific features and are much better to review. And you'll also get direct feedback from other developers and users during the summer.

Since this is a hard requirement we as mentors will also have an eye on that and check if your proposal incorporates it and also warn you ahead of time during the summer if we see that you might not make it. Communicating with us on a regular basis is vital for that, though. 

### How to estimate time needed for development

To get experience with a code base we recommend you try to fix some easy/beginner bug or refactor a piece of code that doesn't conform to the current style guides. 
Look at the code that you want to change, check if it follows our coding guidelines. 
Do some research on the API's you want to use, plan what classes you will add and how their public API will look. 
Write down your algorithms in pseudo code. 
The better your research is and the better you plan ahead the easier it will be to judge how long a given task will take. 
For your time estimates you should also consider that you can do less stuff during exams and try to be a bit conservative. 
If you have never done anything like GSoC before you will tend to underestimate the time to complete a task. 
We know that giving these estimates is not easy and that also professionals have problems with it. 
Having a good plan, knowing its weak and strong points will help a lot. 

### Proposal draft

**You should avoid add some personal/private information
like phone number and address, at your proposal.**

1.  Fork https://github.com/numfocus/gsoc

2.  Clone your fork:

    ~~~
    $ git clone https://github.com/username/gsoc.git
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
**before April 3rd 16:00 UTC**
as a PDF file.

You can use [Pandoc][] to convert your proposal in Markdown
to PDF **if you have LaTeX installed on your machine**
or convert to HTML and print it to PDF using your web browser.
For example, you can

~~~
$ pandoc -f markdown -t pdf YYYY/proposals/your-name.md
~~~

# Organization Instructions

**Please use the instructions of the project/organization
you are interested in work with.**

<!-- ## biocore -->

<!-- Read [default instructions](#default-instructions). -->

<!-- ## DyND -->

<!-- Read [default instructions](#default-instructions). -->

<!-- ## Gensim -->

<!-- Read [default instructions](#default-instructions). -->

<!-- ## JuliaOpt -->

<!-- Read [default instructions](#default-instructions). -->

<!-- ## matplotlib -->

<!-- Read [default instructions](#default-instructions). -->

<!-- ## Pandas -->

<!-- Read [default instructions](#default-instructions). -->

<!-- ## Software Carpentry -->

<!-- Read [default instructions](#default-instructions). -->

[IL]: 2015/ideas-list.md
[issues]: https://github.com/numfocus/gsoc/issues
[GSoC]: http://summerofcode.withgoogle.com/
[ML]: organization/team.md
[Pandoc]: http://pandoc.org/
