---
layout: post
title:  "Publishing Jupyter Notebooks to GitHub Pages"
---

In the previous notebooks, I wrote about handling Jupyter notebooks in a Git
repository using the Markdown format and testing the notebooks automatically
using the GitHub Actions feature. The next step in this workflow is
publishing the notebooks, which I will describe here.

By publishing, I mean making the notebooks available via an URL. This
way, they can be embedded into another website (like the [QuTiP website](qutip.
org)). We can also use services like [nbviewer](nbviewer.org) to render the
notebooks on a webpage.

## GitHub Pages setup

Every GitHub repository (at least public ones) comes with the option of
activating [GitHub Pages](https://pages.github.com/). You can activate it
under *Settings -> Pages* in your repository. You have to
select a source for the activation, which can be a particular branch of your
repository. For convenience, many projects name their specific branch 
*gh-pages*. 
Everything in this branch will automatically be shared on your GitHub Page. The
URL of your website is also shown in the Settings and follows this pattern:

`https://<username>.github.io/<repository-name>`

If you push changes to your *gh-pages* branch, the website will be
automatically updated to the newest version of the branch.

## Automatic Index of Notebooks

If you place an `index.html` file in the *gh-pages* branch, the contents
will be shown if you visit your URL. Since we only want to share files
(notebooks) on our website, a simple list of all files is enough as our
`index.html` (no user will look at this anyway). To create this index we use
the tool called `tee`, which basically generates a nicely formatted list of
the contents in a directory. It can be installed by:

```shell
sudo apt-get tee
```

We create the index when we convert the notebooks from markdown to `.ipynb`
files (described in the previous blog post) and place the generated
`index.html` in the same directory as the notebooks. We can do so by adding
the following command to our `YAML` file:

```shell
tree -H '.' -L 1 --noreport --charset utf-8 -T "QuTiP Tutorial index" -P "*.ipynb" -o index.html
```

Since the `index.html` is now in the same directory as the notebooks, it
will also be part of the *Artifacts* that we create in the workflow.

## Publishing of Notebooks

All files we need to publish on the website are now contained in
the *Artifact*. So we have to take the Artifact and push its
contents to the *gh-pages* branch, and GitHub will do the rest. However, we
need to specify some conditions when the notebooks are published.

First of all, the tests of the notebooks should pass. We can assure this by
setting the `needs: pytests` keyword in the `YAML` file, where `pytests` is
the name of the tests workflow. Furthermore, we only want the publishing to
occur when the repository is the original `qutip/qutip-tutorials` repository.
This way, the publishing does not occur on some forks, which might not have
their GitHub pages set up. Furthermore, we only want to publish notebooks
that were added to the *main* branch of the repository. We can use the `if`
keyword to add these conditions:

```yaml
if: ${ { github.repository == 'qutip/qutip-tutorials' && github.ref == 'refs/heads/main' }}
```

The publish workflow now only runs if all conditions are met. We proceed
with downloading the Artifact, which we can do by:

```yaml
  - uses: actions/download-artifact@master
    with:
      name: executed-notebooks
      path: notebooks
```

Using the above statement, the contents of the *Artifact* are extracted to
the `notebooks` directory. The contents of this directory now have to be
pushed to the *gh-pages* branch. We use the `ghp-import` tool, which
was designed for this particular task. Additionally, this tool removes the
history in the *gh-pages* branch, i.e., there is always just one
commit in the git history of this branch. I like this clean style and the
tool's simplicity (you don't have to deal with basic git commands in
the workflow). In our case, we can publish the contents of the
notebooks directory by running:

```yaml
ghp-import -m "Automatic push by ghp-import" -f -n -p -o -r origin -b gh-pages notebooks
```

The complete workflow for publishing is now defined by this short snippet of
`YAML` code:

```yaml
  publish:
    needs: pytests
    runs-on: ubuntu-latest
    if: ${ { github.repository == 'qutip/qutip-tutorials' && github.ref == 'refs/heads/main' }}
    steps:
      - uses: actions/checkout@v3
      - uses: actions/download-artifact@master
        with:
          name: executed-notebooks
          path: notebooks

      - name: Publish Notebooks
        run: |
          python -m pip install ghp-import
          ghp-import -m "Automatic push by ghp-import" -f -n -p -o -r origin -b gh-pages notebooks
```

## Contributions to QuTiP since last post

* [PR-8](https://github.com/qutip/qutip-tutorials/pull/8) - Automatic index
  of notebooks on GitHub pages

* [PR-9](https://github.com/qutip/qutip-tutorials/pull/9) - Template file
  for notebooks and contribution tutorials

* [PR-10](https://github.com/qutip/qutip-tutorials/pull/10) - Example for
  using `qutip.sesolve` for simulating Larmor Precession

* [PR-12](https://github.com/qutip/qutip-tutorials/pull/12) - Add
  descriptions to notebooks for Master Equation Solver

* [PR-1921](https://github.com/qutip/qutip/pull/1921) - Improving the docs
  of qutip regarding `qutip.sesolve`


