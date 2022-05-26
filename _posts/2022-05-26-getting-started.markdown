---
layout: post
title:  "Getting Started"
---
Hey there.
This blog intends to keep you updated about the progress of my Google Summer 
of Code project. I am very pleased for the opportunity to contribute to 
[QuTiP (Quantum Toolbox in Python)](qutip.org) this summer and for the 
support from Google to pursue this project. Contributing to an open-source 
project was something I wanted to do for a long time, but I could never 
really find the time. QuTiP participates in GSoC under the [NumFOCUS](numfocus.org) umbrella organisation. I personally used many libraries that 
are supported by NumFOCUS during my studies and different projects. Therefore,
I am looking forward to contributing to one of their affiliated projects.

In this blog post I will shortly summarise the project I have in mind and 
the contributions I have already made to QuTiP. Please remember that this is 
my first-ever public blog, and I am far from a professional writer.

### Project
My project will be focused more on the infrastructure around QuTiP and not 
primarily on the implemented functionality itself. However, I will not limit 
my contributions to this particular project. For the GSoC application, I 
wrote the following summary of the project, which can also be found on the 
[GSoC website](https://summerofcode.withgoogle.com/programs/2022/projects/Acz1NBSg).

> QuTiP uses Jupyter Notebooks to demonstrate its functionality in the field of quantum physics and offers guidance to new users. While QuTiP is constantly improved, some notebooks might break due to a change in the QuTiP package. These errors are aggravating, especially for new users, as they appear in notebooks built especially for informative purposes. This Google Summer of Code (GSoC) project will utilize the already provided notebooks in an automated test pipeline to notify developers if recent changes break some notebooks. The test infrastructure will serve as additional integration tests to QuTiP functionality and makes it easier to keep the notebooks up to date. Furthermore, I will add an infrastructure regarding automated formatting of the notebooks and their deployment to QuTiP's website. In general, this project aims to improve the quality of the notebooks and their maintainability.

I will discuss the details of the projects in the coming weeks with my mentors, and we will define clear goals and tools that I will use on the way. This is what I will keep you updated about in this blog.

### Preparation

To prepare for this project, I familiarized myself with the QuTiP repository, the workflow of committing code, and the community in general. While doing so, I solved some open issues from the GitHub repository. These contributions are already merged in the development branch of QuTiP, and some of these are already included in the current QuTiP version (4.7.0). I list them here mainly to keep an overview for myself.

* [PR-1843](https://github.com/qutip/qutip/pull/1843) - Bugfix for the *Nonmarkov Transfer Tensor Method*
* [PR-1844](https://github.com/qutip/qutip/pull/1844) - Change implementation of *Spherical Harmonics*
* [PR-1847](https://github.com/qutip/qutip/pull/1847) - Bugfix in plotting functionality for quantum circuits
* [PR-1883](https://github.com/qutip/qutip/pull/1883) - Automatic building of the PDF documentation using a GitHub Action
* [PR-141](https://github.com/qutip/qutip-notebooks/pull/141) - Example for testing of the Notebooks
