# Title
2D color maps

## Abstract

All of the color mapping in Matplotlib is currently derived from
`ScalerMappable` which as the name suggests maps scalers from `R^1 ->
R^4` RGBA color space.  It is common to want to map a vector to
colors, for example to control the alpha based on a second value in a
scatter plot or to show the orientation of a field.

| **Intensity** | **Involves**  | **Mentors** |
| ------------- | --------------|------------ |
| Intermediate  | Python, | [@tacaswell][@story645] |

## Technical Details

- All the work will be done in Python.
- It will be in accordance with current implementation of 1D colormaps.
- Extending the existing 1D normalization for 2D data : This can be done by
a family of normalizers which go from data -> unit-disk or square in the
complex plane
- Creating color maps that go from the unit disk or square -> RGBA
- Exposing these classes to user as either new API or extending the existing
`ScalerMappable` API
- Implementing a 2D color bar
- Developing 2D color maps : They can be created by doing bi-linear
interpolation between four colours. Also major focus would be on developing
perceptually friendly colormaps so that color blind people have no difficulty in
distinguishing colors.

## Schedule of Deliverables

### May 1th - May 28th, **Community Bonding Period**

- Before the official time period begins I will do some tasks listed under
[MEP21](http://matplotlib.org/devel/MEP/MEP21.html): color and cm refactor.
This will greatly help in understanding the current implementation of
normalization and color mapping tools in Matplotlib
- Set up a blog
- Along with this I will continue to solve issues on github

### May 29th - June 3rd

- Discuss implementation details with mentors
- Start working on normalizers

### June 5th - June 9th

- Finish up normalizers
- Write tests
- Starting working on color map that maps unit circle or square to rgba

### June 12th - June 16th

- Finish up color maps
- Debug and test
- Write tests

### June 19th - June 23th, **End of Phase 1**

- Complete any unfinished work in Phase 1
- Write documentation for code written so far
- Write blog for Phase 1

### June 26 - June 30th, **Begin of Phase 2**

- Decide on how API will be exposed to users as new API or as extension of
ScalerMappable
- Make API for exposing normalizers and color maps to user

### July 3rd - July 7th

- Test new API
- Document the API so that it is exposed to users

### July 10th - July 14th

- Implement 2D color bar

### July 17th - July 21th, **End of Phase 2**

- Complete any unfinished work of Phase 2
- Test and document
- Write blog for Phase 2

### July 24th - July 28th, **Begin of Phase 3**

- Research on perceptually friendly colormaps

### July 31st - August 4th

- Develop 2D colormaps

### August 7th - August 11th

- Test and document colormaps

### August 14th - August 18th

- Write examples for Matplotlib gallery to demonstrate 2D color maps

### August 21st - August 25th, **Final Week**

- Buffer period for any unfinished work
- Write blog for Phase 3
- Clean up code

### August 28th - August 29th, **Submit final work**

## Future works

In future the project can be extended to higher dimensions by mapping to
quaternions as well.

## Open Source Development Experience

- (Merged) [#8094](https://github.com/matplotlib/matplotlib/pull/8094) Cleaned up documentation by removing an example
- (Merged) [#8097](https://github.com/matplotlib/matplotlib/pull/8097) Improved the code to use plt.gca instead of plt.axes
- (Merged) [#8154](https://github.com/matplotlib/matplotlib/pull/8154) Merged fill_demo and fill_demo_features examples
- (Merged) [#8194](https://github.com/matplotlib/matplotlib/pull/8194) Added link to Gitter channel in readme
- (Merged) [#8234](https://github.com/matplotlib/matplotlib/pull/8234) Fixed broken Gitter badge
- (Merged) [#8343](https://github.com/matplotlib/matplotlib/pull/8343) Made ArrowStyle docstrings numpydoc compatible
- (Open) [#8336](https://github.com/matplotlib/matplotlib/pull/8336) Merged three streamplot examples into one plot with subplots
- (Open) [#8157](https://github.com/matplotlib/matplotlib/pull/8157) Added 'which' kwarg to autofmtxdate and wrote tests

## Other Experiences

- [AI-Bot](https://github.com/patniharshit/Ultimate-Tic-Tac-Toe) in python
  for 4X4 Ultimate-Tic-Tac-Toe.
- [Brick-Breaker](https://github.com/patniharshit/Brick-Breaker), a 2d shooter
  game in OpenGL.

## Why this project?

Currently there are no multidimensional colormaps in Matplotlib. This is a
big nuisance if we want to modulate the color and opacity independently based
on data in different dimensions independently. This project has been requested
for a long time by people in neuroscience, astronomy etc.
Here are some of those requests :
- #4369
- [bivariate colormaps](http://stackoverflow.com/questions/15207255/is-there-any-way-to-use-bivariate-colormaps-in-matplotlib)

Having used Matplotlib for displaying graphical information many times I
wanted to give something back to the community. I am the right person to do
this project because not only I want to contribute to Open Source but I have
also worked closely with the community for last month so I have good
understanding of workflow.


## Appendix

### About Me

I am a sphomore at International Institute of Information Technology, Hyderabad
majoring in Computer Science. I have intermediate proficiency in Python and
have worked on several projects with it. I am also an active contributor of
Matplotlib for some time.

### Contact
|          |                                                        |
|----------|--------------------------------------------------------|
| Name     | Harshit Patni                                          |
| Email    | patniharshit@gmail.com                                 |
|          | harshit.patni@students.iiit.ac.in                      |
| Github   | [patniharshit](https://github.com/patniharshit)        |
| Gitter   | patniharshit                                           |

### Availability

I don't have any commitments in summer and GSOC will be my full time job.
My summer vacations starts on 27 April and college reopens in last week of
July.
* **Time Zone :** Indian Standard Time (IST) UTC +5:30
*  **Hours per week :** 35-40 hours(during vacations), this may go down to
30-35 hours in August/September.
