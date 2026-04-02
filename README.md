<br>

<img src="man/figures/bioconductor.png" align="right" alt="Bioconductor logo" width="150"/>

<br>

# BiocHowTo

<br>

Welcome to the Bioconductor HowTo collection! The HowTos are short, 
stand-alone documents, each focused on how to address a well-defined question 
using one or more Bioconductor packages. 

For a list of the available HowTo documents, see the [Articles](articles/index.html) tab.

## How to contribute

1. [Fork this repository](https://github.com/Bioconductor/BiocHowTo/fork) and create a new branch. 
2. Copy and rename the `howto_template.qmd` file from `inst/templates` to 
the `vignettes` directory, and edit the copy accordingly. Please choose the 
name for the new vignette in such a way that it is unique and clearly indicates 
the content. Note that the title of the vignette should be added both to the 
`title` field and the `%\VignetteIndexEntry` in the YAML section.
3. Test the vignette locally in a fresh R session, to make sure that it is 
self-contained and runs without errors.
4. Add your name to the `Author` list in the `DESCRIPTION` file.
5. If your HowTo is using packages that are not already included among the 
dependencies of `BiocHowTo`, please add them to the list of `Imports` in the 
`DESCRIPTION` file. One way to do this is via the `usethis` package:

```
usethis::use_package("name_of_new_dependency")
```

6. Add the new vignette to a suitable section under 'Articles' in the 
`_pkgdown.yml` file.
7. Push the changes to your forked repository and open a pull request to 
the `devel` branch of the parent repository.
8. All submissions will be reviewed by the Bioconductor Training Committee. 


## How to suggest a new topic

To suggest a new topic for a HowTo, open an issue and provide some more
details of your suggestion. If you would like to take on one of the open 
issues, either assign yourself (if possible), or comment in the issue, and 
one of the administrators can assign you. 

## AI Skills for Bioconductor Tasks

The `inst/skills/` directory contains structured how-to guides ("skills") for
common Bioconductor tasks formatted for use as context with AI coding
assistants. See [USE_SKILLS.md](USE_SKILLS.md) for the full list of available
skills and usage instructions for Claude Code, GitHub Copilot / Copilot CLI,
and OpenAI Codex.
