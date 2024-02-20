
<!-- README.md is generated from README.Rmd. Please edit the README.Rmd file -->

# R Project Template

This is a template repository for data analysis projects mainly
developed in R language.

## Introduction

The aim of this repository is to provide a template for data analysis
projects.

The idea is to have a common starting point in order to improve code
quality and reproducibility.

## Project Structure

Every project should follow more or less the following structure:

    MyProject
    ├── data
    │   ├── raw
    │   │   ├── sourceX
    │   │   │   ├── code
    │   │   │   ├── data
    │   ├── processed
    ├── docs
    ├── output
    │   ├── figs
    │   ├── reports
    │   ├── results
    ├── src
    │   ├── R
    │   │   ├── *.R
    │   ├── Rmd
    │   │   ├── *.Rmd
    ├── tests
    │   ├── testthat
    │   │   ├── test_*.R
    ├── README.md
    ├── MyProject.Rproj
    └── .gitignore

### Data

The `data` folder contains two subfolders: `raw` and `processed`.

#### Raw

The `raw` folder contains the raw data used in the project.

This folder should be organised per data source, meaning that each data
source should have its own subfolder.

Inside each data source folder there should be two directories:

- `code`: The `code` directory should contain the code used to download
  or pre-process the raw data.
- `data`: The `data` directory should contain the actual raw data.

#### Processed

The `processed` folder contains the processed data used in the project.

### Docs

The `docs` folder should contain any documentation produced with the
project (e.g., summaries).

### Output

The `data` folder contains what is considered to be the output of the
project.

It is currently has three subfolders:

- `figs`: The `figs` folder should contain the generated figures.
- `reports`: The `reports` folder should contain the reports generated
  with the R Markdown files.
- `results`: The `results` folder should contain the result files of the
  analyses.

#### Results

The `results` folder plays a crucial role in tracking project progress
by storing each analysis run in a separate sub-directory. An analysis
run refers to the complete execution of a specific script or workflow.

We decided to adopt the common practice of naming the sub-folders by the
date of the run in the format `YYYY_MM_DD`.

### Src

The `src` folder should contain the source files used in the project. It
is currently organised per file type.

### Tests

The `tests` folder contains the different tests for the code.

#### R Tests

R test files live in `tests/testthat/`, start with `test_`, and end with
`.r` or `.R`. We decided to adopt the
[testthat](https://testthat.r-lib.org/articles/special-files.html)
naming convention, so that there’s a one-to-one correspondence between
the files in `src/R/` and the files in `tests/testthat/` (e.g.,
`src/R/myfile.R` has a matching `tests/testthat/test_myfile.R`).

The test files must contain the following lines of code to be executed.

``` r
# Load testthat library
library(testthat)

# Source file to test
source(file = "../../src/R/myfile.R")
```

For example, if `/src/R/myfile.R` contains a function `compute_sum`:

``` r
# Load testthat library
compute_sum <- function(a, b){
  # Compute sum
  out = sum(a, b)
  # Return
  return(out)
}
```

A possible test file `tests/testthat/test_myfile.R` could be:

``` r
# Load testthat library
library(testthat)

# Source file to test
source(file = "../../src/R/myfile.R")

# Test compute_sum returns the expected value
test_that("compute_sum returns the sum of two numbers", {
  # Test compute_sum
  expect_equal(compute_sum(2, 3), 5)
})
```

The tests can be executed with the following command:

``` r
testthat::test_dir(path = "tests/testthat")
```

### README.md

The project must have its own README file stored as `README.md`
markdown.

The README file should contain (all or some of) the following sections:

- `Introduction`: A general overview of the project.
- `Project Structure`: An overview of the structure of the project.
- `Data`: Information on the available data. If data has been already
  pre-processed (e.g., RNA-seq data aligned and summarised), this
  section should contain information on the used pipelines.
- `Analysis`: Overview of the analysis done for the project.
- `People`: Who is working on the project.
- `Getting Started`: A short description of where to start to look at
  inside the project folder.
- `Credits`: Recognize people who provided assistance, data, etc.
- `Acknowledgements`: Acknowledgement of source of funding.

## Dependency Management System for R

To ensure reproducibility, we want to use a dependency management system
for R.

We decided to use [renv](https://rstudio.github.io/renv/index.html),
which is based on the [packrat](https://rstudio.github.io/packrat/) R
package and is integrated in [R
Studio](https://posit.co/download/rstudio-desktop/).

Packrat lets you snapshot the state of your private library, which saves
to your project directory whatever information packrat needs to be able
to recreate that same private library on another machine. The process of
installing packages to a private library from a snapshot is called
restoring.

***NOTE***: Unfortunately, private libraries don’t travel well; like all
R libraries, their contents are compiled for your specific machine
architecture, operating system, and R version. For improved
reproducibility, in the future we will look into containerization and
virtual environments.

### Packrat

`Packrat` enhances your project directory by storing your package
dependencies inside it, rather than relying on your personal R library
that is shared across all of your other R sessions. We call this
directory your private package library (or just private library). When
you start an R session in a `packrat` project directory, R will only
look for packages in your private library; and anytime you install or
remove a package, those changes will be made to your private library.

`Packrat` helps to make R projects more:

- **Isolated**: Installing a new or updated package for one project
  won’t break your other projects, and vice versa. That’s because
  `packrat` gives each project its own private package library.
- **Portable**: Easily transport your projects from one computer to
  another, even across different platforms. `Packrat` makes it easy to
  install the packages your project depends on.
- **Reproducible**: `Packrat` records the exact package versions you
  depend on, and ensures those exact versions are the ones that get
  installed wherever you go.

### renv

The [renv](https://rstudio.github.io/renv/index.html) package helps us
create reproducible environments for our R projects.

We can install the latest version of
[renv](https://rstudio.github.io/renv/index.html) from CRAN with:

``` r
# Install renv from CRAN
install.packages("renv")
```

#### Create a project library

We can set up a project library, containing all the packages you’re
currently using, with:

``` r
# Initialize renv in a new or existing project
renv::init()
```

The packages (and all the metadata needed to reinstall them) are
recorded into a lockfile, `renv.lock`, and a `.Rprofile` ensures that
the library is used every time you open that project.

The updated project structure should be:

    MyProject
    ├── data
    │   ├── raw
    │   │   ├── sourceX
    │   │   │   ├── code
    │   │   │   ├── data
    │   ├── processed
    ├── docs
    ├── output
    │   ├── figs
    │   ├── reports
    │   ├── results
    ├── src
    │   ├── R
    │   │   ├── *.R
    │   ├── Rmd
    │   │   ├── *.Rmd
    ├── tests
    │   ├── testthat
    │   │   ├── test_*.R
    ├── renv
    ├── renv.lock
    ├── README.md
    ├── README.Rmd
    ├── MyProject.Rproj
    ├── .gitignore
    └── .Rprofile

#### Update a project library

As you continue to work on your project, you will install and upgrade
packages, either using `install.packages()` and `update.packages` or
`renv::install()` and `renv::update()`.

After you’ve confirmed your code works as expected, use
`renv::snapshot()` to record the packages and their sources in the
lockfile.

#### Restore a project library

To reinstall the specific package versions recorded in the lockfile we
can use:

``` r
# Restore a project library
renv::restore()
```

This is particularly helpful if you need to share your code with someone
else or run your code on a new machine.

## Credits

This template repository is maintained by Alessandro Barberis
([@alebarberis](https://github.com/alebarberis)).
