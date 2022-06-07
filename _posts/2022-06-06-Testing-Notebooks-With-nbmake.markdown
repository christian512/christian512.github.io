---
layout: post
title:  "Testing Notebooks with nbmake"
---

After discussing a clean way to develop Jupyter Notebooks in Git in the last
post, the next step is to verify the correct execution of the notebooks. As
the notebooks
are used as introductory material for new QuTiP users, they must run without errors using the latest version of QuTiP. In this blog post
I discuss the possibility of testing the notebooks for errors using GitHub
Actions to automate the workflow.

## `nbmake` module

Jupyter Notebooks can be executed from a command line, and we can simply
write a script that checks if the execution of the notebook was successful.
For example, this is what I initially wrote in my project proposal, where I
referred to
[this blog post](https://www.blog.pythonlibrary.org/2018/10/16/testing-jupyter-notebooks/)
.
But why reinvent the wheel? One of my mentors recommended using the
[`nbmake` plugin](https://github.com/treebeardtech/nbmake)
for [`pytest`](pytest.org). `pytest` is a software test framework also
used in
the main QuTiP repository. Hence, `nbmake` is a perfect fit for using it with 
the notebooks for QuTiP.

After the installation, we can test all notebooks contained in a certain
directory by executing:

```shell
pytest --nbmake notebook_dir/*.ipynb
```

`nbmake` tests interpret each notebook as an individual test. Hence, the
number of tests is equal to the number of notebooks. The summary shows a
count of the failed and passed tests (i.e., notebooks).

By default, `nbmake` does not overwrite the notebooks. I.e., the notebooks
are not modified by running the tests. If you want to overwrite (which is
what we want later), you can simply use the `--overwrite` flag.
Furthermore, you can define a timeout for each cell by setting
`--nbmake-timeout=<time_in_secs>`. This way, we can prevent the tests from
getting stuck in a notebook with a large execution time.

## Test QuTiP tutorials with GitHub Action

As discussed in the previous blog post, the notebooks in the
`qutip-tutorials` repository are stored in a `markdown` format. But `nbmake`
requires `.ipynb` files to execute the tests. Hence we need to convert the
notebooks from `qutip-tutorials` before executing the tests. As discussed
before, this is done by:

```shell
jupytext --to notebook tutorials/*.md
pytest --nbmake tutorials/*.ipynb
```

This is an easy and reproducible setup for testing locally before committing to the repository. Also, contributors who add a new notebook will execute
the notebook before adding it to the repository, so running tests locally
might not be needed.

However, we want to prevent the addition of notebooks with errors to the
repository. Therefore, we want to automate the testing of notebooks. This is
a perfect use-case of [GitHub Actions](https://github.com/features/actions),
which provides runners that we can utilize for this workflow. A GitHub
Action is defined by a `YAML` file. GitHub Actions can be used for
testing and any type of operation you can perform on your local computer.
A good place to start with GitHub Actions is [here](https://docs.github.
com/en/actions).

For the `qutip-tutorials` repository, we first install all required
dependencies for `qutip` and `qutip-qip` and then install the current
development version. This way, we can run the tests against the
current state of QuTiP. The notebook tests are performed as above with an
additional step of moving the `.ipynb` files to a different directory. There
we overwrite the notebooks while testing to have a version with outputs. We
will use these *executed notebooks* for sharing the notebooks as guides.
The excerpt of the `YAML` file for the test looks like the following:

```yaml
    - name: Convert Notebooks
      run: |
        rm -rf notebooks
        mkdir notebooks
        jupytext --to notebook tutorials/*.md
        mv tutorials/*.ipynb notebooks

    - name: Run tests
      run: |
        jupyter kernel &
        pytest --nbmake notebooks/*.ipynb --overwrite --nbmake-timeout=500
```

## Store Notebooks as Artifacts

We want to publish the executed notebooks later on. Therefore, we must get
them out of the GitHub Actions test run. A tool provided by GitHub Actions
for making outputs from an action available is called
[*Artifact*](https://docs.github.
com/en/actions/using-workflows/storing-workflow-data-as-artifacts)
. We can simply use a pre-defined action to upload any set of files. They
are compressed and made available on the GitHub webpage. Furthermore, we can
also download them via an API and use them in a different action (as we
will do for publishing). The corresponding `YAML` excerpt for creating the 
artifact is the following: 

```yaml
    - name: Create Notebook Artifact
      uses: actions/upload-artifact@v3
      with:
        name: executed-notebooks
        path: notebooks/*.ipynb
```



## Contributions to QuTiP since last post

* [PR-2](https://github.com/qutip/qutip-tutorials/pull/2) - Setup test
  workflow for notebooks in markdown format.

* [PR-5](https://github.com/qutip/qutip-tutorials/pull/5) - Update `readme.
  md` and dependencies for `qutip-tutorials`

* [PR-7](https://github.com/qutip/qutip-tutorials/pull/7) - Automatic
  publishing of executed notebooks to GitHub pages

* [Help Group](https://groups.google.com/g/qutip/c/F7tOoZzMjto) - Proposed a
  quick fix for calculating the entropy of a system represented in the
  *Dicke basis*

* [Issue-1919](https://github.com/qutip/qutip/issues/1919) - Reported on the
  problem with calculating entropy in *Dicke basis* and tracked down the error
  to numerical uncertainty in eigenvalue calculation

