---
layout: post
title:  "Why Jupyter Notebooks"
---
<figure style="float: right; padding-left: 15px">
<img src="/assets/garden_work.jpeg" 
alt="Me Working in the Garden" width="250"/>
<figcaption>Working in the Garden</figcaption>
</figure>

In the last posts I discussed the workflow I developed around the Jupyter Notebooks that are provided as tutorials for QuTiP. I will probably modify this workflow during this project, but its main functionality will stay the same.

Core part of the QuTiP tutorials are the Jupyter notebooks. In the past two weeks I've been updating a lot of the notebooks in the [qutip-notebooks repository](https://github.com/qutip/qutip-notebooks) and added them to the new [qutip-tutorials repo](https://github.com/qutip/qutip-tutorials). While working on these notebooks, I found that the largest problem that most notebooks have is a missing documentation. In this post I describe what, in my opinion, makes a good Jupyter notebook.

### Notebooks - Explorative Science

Working on so many notebooks reminded me of my first interactions with Python, when working on assignments of a quantum mechanics course. Back then I got annoyed by Mathematica (which was the standard recommended software...) and came across Python and Jupyter Notebooks as an alternative. Coming from Mathematica the notebook style was somewhat natural and I only had to change the language I used for programming.
 
I think notebooks did and do a great job when it comes to mixing documentation with actual code. For example, we had to do assignments for different course and I always used the annotation functionality to describe the code, explain my steps or add some equations to the code I wrote. Having the documentation and the code in one file was very helpful for exploring physics using computational methods.

The other great feature of notebooks is the inline plotting. Presenting the results of some simulation together with the code is very convenient, and it is also easier to understand the outputs when the code is directly above it. 

I understood that notebooks are great for presenting results and explaining approaches to a particular task. For larger project, including complex functionality I think a modular code base should be chosen (for the reason I write down in the next section).
 
When it comes to QuTiP, I have the following principle in mind: all functions are developed, tested and published in the `qutip` package. The provided notebooks demonstrate how to use these functions with the help of the annotation cells and the inline plotting.

### Messy Notebooks and Scripts

I worked on various research projects, which all mainly relied on data analysis or simulations: mostly writing code. While working on such code I have seen (and probably produced) a lot of messy code. It's the type of scripts that you can't understand after a few weeks and that anyone else would rewrite if they got it from you.

The problem with notebooks is that they can become even more messy: Unordered execution of cells and hidden states add additional complexity and confusion to the code. Besides that, the code in a notebook is not reusable: If you define a complex function in a notebook it is only available in that particular notebook (unless you use some weird tricks). Furthermore, people tend to not use MarkDown cells, which (in my view) is the whole point of using notebooks over python scripts.

There is a long list of bad coding styles that notebooks encourage, which was also collected by [Joel Grus](https://joelgrus.com/) in his talk [*"I don't like notebooks"*"](https://www.youtube.com/watch?v=7jiPeIFXb6U). I do agree with him on many points, notebooks are generally not great for software development. But for demonstrating the usage of some package or presenting results, I think they are a great tool.

### Notebook approach for QuTiP Tutorials

The notebooks I provide in `qutip-tutorials` are thought for demonstrating the capabilities of the `qutip` package. So I will try to reduce the complexity of the code in the notebooks by using as many built in `qutip` functions as possible.

As discussed in previous blog post, we store the notebooks in a MarkDown format for a better experience when using `git` with notebooks. Furthermore, we can use these MarkDown files to present the code in a documentation-style. The cells are not executable, but users can copy the relevant code snippets and don't even have to think about Jupyter notebooks. 

All that requires that there are enough annotations in the notebooks, which is what I'm working on from now on (mostly). The correct functioning of the tutorials is verified by the workflow I discussed in previous posts. 

## Contributions to QuTiP since last post

* [PR-15](https://github.com/qutip/qutip-tutorials/pull/15) - Large rework of
  GitHub Actions workflow
* [PR-17](https://github.com/qutip/qutip-tutorials/pull/17) - Fix the usage
  of `qutip.expect` in a tutorial
* [Help Group](https://groups.google.com/g/qutip/c/MY4NekP18L4) -Help someone
  with a projecting problem
* [Issue-1937](https://github.com/qutip/qutip/issues/1937) - Reported issue in
  qutip.groundstate`
* [PR-19](https://github.com/qutip/qutip-tutorials/pull/19) - Add the
  Lectures to qutip-tutorials and update them to work with the current release
* [PR-22](https://github.com/qutip/qutip-tutorials/pull/22) - Format the
  existing notebooks for PEP8, add check to pipeline and convert two notebooks for v5


