# NumFOCUS GSOC Operations

[NumFOCUS](http://numfocus.org/)
will apply to Google Summer of Code (GSoC)
as a umbrella organization.
[All projects/organizations supported by NumFOCUS](http://numfocus.org/projects/)
can participate at GSoC under NumFOCUS umbrella.
Projects/organizations not supported by NumFOCUS
can participate at GSoC under NumFOCUS umbrella
if approved by NumFOCUS board.

## NumFOCUS Administrator for GSoC

The NumFOCUS board will indicate two persons,
named in this document as **organization administrator**,
to be in charge of NumFOCUS application **for each edition** of GSoC.

The **organization administrators** are responsible for

- create the initial application for NumFOCUS to apply for GSoC
- advertise the application to **all** projects/organization supported
  by NumFOCUS
- manage NumFOCUS profile at GSoC
- request a minimum and a maximum number of slots
  that can accomodate the slots requested by each project/organization
  under NumFOCUS umbrella.
- stay in contact with sub-org admins

### How to sign up mentors

You can use google forms to sign up mentors who want to work with NumFOCUS. This
will have the advantage that we automatically have a list of all mentors and
their contact information. Mentors should be given access to numfocus mentoring
mailing list.

## Timeline for Organization Administrators

*January*:
- Ask NumFOCUS if they want to participate. Preferably with help of NumFOCUS staff

*Feburary*:
- Check if application docs are  up to date
- Apply as umbrella organization to GSoC
- check projects ideas page
            
*March*:
- If accepted advertise to possible students.
- Accept sub-org until 5 days after Google announces participating orgs
- check projects ideas page is ready for students again
         
*April*:
- request slots numbers from sub-orgs and tell them Google (google forms are good here)
- select students for sub-orgs
- have a blog post about accepted projects
         
*First Evaluation*:
- Ensure that every mentor fulfills the evaluation

*Second Evaluation*:
- Ensure that every mentor fulfills the evaluation

*Final Evaluation*:
- Ensure that every mentor fulfills the evaluation

*September/October*:
- Select administrators for next year and tell information the NumFOCUS staff.
- Visit Mentor Summit

## Guidelines to Select Accept Students Proposals

**Note**: Slots are requested after the students submission phase ends

**Sub Orgs** will discuss and accept/reject students proposal based on:

1.  the number of slots received;
2.  if the proposal already has at least one mentor;
3.  student background and proposal.

**If** NumFOCUS received less slots than then requested the **organization
administrators** solve conflicts in slots allocation for each sub org. We will
try to assign at least one slot to each sub org.

**Changes for 2022**

GSoC changed the rules for slot requests in 2022 and now we need to provide them
with a strict ranking of our proposals with the number of slot requests.

Each sub organisation will submit a tiered ranked list of student proposals they
want to accept. The tiers are divided into 3 categories:
- 1: We absolutely want to mentor this contributor, this is the whole reason we want to participate in GSoC!
- 2: We would love to have them contribute and provide mentorship.
- 3: If there are enough slots we would love to take them on too!

Please do remember you still need to have enough mentors for all the contributors, you don't need
to rank every submitted application. Just the ones you have the bandwidth to mentor over the summer.
This system adds a new level to the minimum and maximum number of slots we used to do till 2021.

Please do keep in mind this system doesn't work if all the suborgs put all of the contributors in tier 1.
So if you requesting more than 3 slots please try to put them in different tiers.

Once all the suborgs have contributed the tiered ranking the NumFOCUS org admins will run a script
to randomise all the applicants in their respective tier and then create the final rank list. The
final ranked list will be shared with everyone.


The script will look something like this:
``` python
>>> sub_org_1 = {1: ["Contributor_1"], 2: ["Contributor_2", "Contributor_3"], 3: []}
>>> sub_org_2 = {1: ["Contributor_4"], 2: [], 3: []}
>>> sub_org_3 = {1: ["Contributor_5"], 2: [], 3: []}
...
...
...
>>> all_sub_orgs = [sub_org_1, sub_org_2, ... ]

>>> from collections import defaultdict
>>> import random
>>> contributor_tiers = defaultdict(list)
>>> for sub in all_sub_orgs:
>>>     for tier in sub:
>>>         contributor_tiers[tier].extend(sub[tier])

>>> rank = 1
>>> for n in contributor_tiers:
>>>     random.shuffle(contributor_tiers[n])
>>>     for i in contributor_tiers[n]:
>>>         print(f'Rank {rank}: {i}')
>>>         rank += 1
Rank 1: Contributor_5
Rank 2: Contributor_4
Rank 3: Contributor_1
Rank 4: Contributor_2
Rank 5: Contributor_3

```
