Final Project Check-in
================

The application accident\_tracker and its associated functions will
serve as a tool to filter and present information related to accidents
in an easy accessible format.

##### *Planned Git Workflow*

There will be a total of two git repositories for the project, one for
the shiny application and one for the package containing the necessary
functions for the app to run. These repositories are owned by a single
group member, with the remaining three members all having forked
versions of them. Any changes done by a single group member are
initially done on the forked version before a pull request is sent to
the main repositories and the versions merged if no issues arise and the
changes are agreed upon.

##### *List of elements to be included*

The finalized project will include several elements between the two
repositories with the most important ones being:

-   A package with documented functions, with functioning unit tests for
    the package
-   An inclusive vignette
-   A shiny application capable of reading accident data from a chosen
    data set along with numerous variables
-   A detailed ui allowing for the toggling of filters using reactive
    code.
    -   Display precipitation, weather, severity on a color scale
    -   Variable sizes of data clusters depending on zoom level (city,
        county, state).
-   Implementation of different tabs to increase functionality
    -   i.e. Graphical elements to highlight relationships between
        certain variables.

**Package main repo**

<https://github.com/ahvickers/accidenttracker>

git Tag to view is v0.1.1, commit is 7a69c69

**Application main repo**

<https://github.com/ahvickers/accident_shiny_project> (app main repo)

git Tag to view is v0.1.1, commit is 835cbf7
