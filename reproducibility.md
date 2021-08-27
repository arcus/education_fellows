<!--
author:   Joy Payton, Arcus Education, Children's Hospital of Philadelphia
email:    paytonk@chop.edu
version:  0.0.2
language: en
narrator: US English Female
comment:  Reproducibility is essential to scientific efforts.  Generalizability and data reuse are also important for the application and expansion of scientific inquiry. Technology can help achieve these goals.
link:     https://chop-dbhi-arcus-education-website-assets.s3.amazonaws.com/css/custom.css
-->

# Reproducibility, Generalizability, and Reuse: How Technology Can Help

<div class = "hint">

## Overview

This module provides learners with an approachable introduction to research reproducibility, generalizability, and reuse, and how technical approaches can help.

**Estimated time to completion**: 1 hour didactic instruction, 1 hour personal work.

**Pre-requisites**: It is helpful if learners have conducted research, are familiar with peer-reviewed literature, and have experience using data and methods developed by other people.    

**Format**: This module uses text and video and is intended to accompany an in-person or otherwise synchronous presentation.  Materials contained here will allow for review after a live session.

**Learning Objectives**:  After completion of this module, learners will be able to:

* Explain the importance of reproducible methods
* Give an example of a reproducible method in data acquisition and analysis
* Give an example of a reproducible method in data management and metadata

</div>


Instructor Introduction
=======

Your live session is facilitated by [Joy Payton](https://linkedin.com/in/joypayton), who leads [data education efforts](https://education.arcus.chop.edu) within the [Arcus program](https://arcus.chop.edu), which is an initiative of the [Research Institute](https://www.research.chop.edu) of the [Children's Hospital of Philadelphia](https://www.chop.edu).


Contents
========

* [Concepts](#Concepts)

  * [Reproducibility](#Reproducibility)
  * [Generalizability](#Generalizability)
  * [Reuse](#Reuse)
  * [A Data Management and Sharing Snafu](#A-Data-Management-and-Sharing-Snafu)
* [Tools for Better Practices](#Tools-for-Better-Practices)

  * [Scripts](#Scripts)
  * [Data management and metadata](#Data-management-and-metadata)]
  * [Version Control](#Version-Control)
  * [Dependency Management](#Dependency-Management)
* [Additional Materials](#Additional-Materials)
* [Homework](#Homework)

## Concepts

The concepts of **reproducibility**, **generalizability**, and **reuse** will frame the problem space that we'll describe in this module.  We'll define these terms and give some examples.  

These concepts will be illustrated in a (charming if rage-inducing) YouTube video, *A Data Management and Sharing Snafu*.

After exploring these concepts, we'll go over some methods to address these challenges, using technology.

### Reproducibility

You may hear the terms **"reproducibility"** and/or **"replicability"**, depending on context.  Jargon varies by field and you may see either or both terms used to refer to similar goals: the ability to (1) precisely redo analyses on original data to confirm the original findings or (2) to carefully apply the original methods to new data to confirm findings in a different dataset.  Here, I follow what is becoming more customary and using the term "reproducible" to refer to both efforts.

The **"reproducibility crisis"** refers to the problem in peer-reviewed research in which studies *cannot* be reproduced or replicated because of insufficient information, or in which studies *fail* to be reproduced or replicated because of preventable problems in the initial research.  This is problematic because it means wasted time and money, reduced public trust in science, unverifiable claims, and lost chances for scientific consensus.

<div class = "hint" style = "align-items: center; display: flex;">

<div style = "margin: 2rem; max-width: 50%"> If you've ever tried to replicate an analysis or study procedure from just the methods section of a paper, you probably experienced it as something like this! </div>

<div style = "margin: 2rem; max-width: 50%; float:left;">
![Humorous illustration of "how to draw a horse"](img/horse.png)
</div>

</div>

Examples of reproducibility problems exist at small and large scale:

* Experiencing dread at trying to reproduce your own findings a few months after doing it the first time
* Being stymied by a collaborator's cryptic notes that don't explain how to do a particular analysis step
* Being unable to perform required computation because of reliance on expensive, deprecated, or proprietary software or hardware
* Results that don't replicate due to poor statistical practices, such as "p-hacking", "HARKing", or multiple tests without correction


<div class = "hint">

Think about it: when have you been frustrated by a process or study that had poor reproducibility?  Have you ever put **yourself** in a bad situation because you didn't think ahead to how you'd need to replicate your actions?

</div>

### Generalizability

Research is **generalizable** if findings can be applied to a broad population.  

Historically, biomedical and social science research projects have struggled with generalizability due to unrepresentative data.  For example, the acronym **"WEIRD"** refers to the tendency of psychological studies to rely on subjects (often undergraduate students) who are disproportionately from **W**estern, **E**ducated, **I**ndustrialized, **R**ich, and **D**eveloped cultures -- cultures which, compared with the global population as a whole, are indeed weird.  

<div class = "hint">

Read more: in 2010 Joseph Henrich and others published a brief in *Nature* coining "WEIRD" to describe skewed participation in psychological studies.  The citation below includes a link to this piece in Penn libraries.

<div style = "margin-left: 40px; text-indent: -40px; font-size:0.8em;">

Henrich, Joseph, et al. "Most people are not WEIRD: to understand human psychology, behavioural scientists must stop doing most of their experiments on Westerners." *Nature*, vol. 466, no. 7302, 2010, p. 29. *Gale In Context: Science*, https://link.gale.com/apps/doc/A230766048/SCIC?u=upenn_main&sid=summon&xid=b438bdf6.

</div>

</div>


Until recently, many biomedical studies were conducted on disproportionately male populations and ignored disease presentation or pharmacodynamics in women and girls (or even female lab animals).  In 1993, the [NIH Revitalization Act](https://www.ncbi.nlm.nih.gov/books/NBK236531/) began requiring NIH-funded clinical research to include women as subjects.  Two decades later, Janine Clayton and Francis Collins wrote a pointed call to action, again in *Nature*, indicating that bench researchers had not willingly followed best practices and needed NIH to force them to use female animals and cells:

> There has not been a corresponding revolution in experimental design and analyses in cell and animal research — despite multiple calls to action. Publications often continue to neglect sex-based considerations and analyses in preclinical studies. Reviewers, for the most part, are not attuned to this failure. The over-reliance on male animals and cells in preclinical research obscures key sex differences that could guide clinical studies. And it might be harmful: women experience higher rates of adverse drug reactions than men do. Furthermore, inadequate inclusion of female cells and animals in experiments and inadequate analysis of data by sex may well contribute to the troubling rise of irreproducibility in preclinical biomedical research, which the NIH is now actively working to address.

In early 2016, a policy requiring the consideration of sex as a biological variable (SABV) went into effect, and applications for NIH funding were required to comply with best practices related to sex-inclusive experimental design.  Progress in NIH's SABV efforts were [recently reported in a 2020 article](https://orwh.od.nih.gov/in-the-spotlight/all-articles/orwh-releases-progress-report-sex-biological-variable-policy).

<div class = "hint">

Listen to Janine Clayton speak about scientific rigor and female animal inclusion (5 minute listen): https://www.wbur.org/hereandnow/2014/05/20/nih-female-animals

</div>

In wearable sensor and computer vision development, engineers using skewed samples have failed to realize that the optical sensors and computer vision algorithms they created may perform less well on dark skin. See, for example, https://www.statnews.com/2019/07/24/fitbit-accuracy-dark-skin/ and https://www.nytimes.com/2018/06/21/opinion/facial-analysis-technology-bias.html.

The challenge of generalizability is closely linked to reproducibility.  For example, a study that demonstrates the effectiveness of exercise to improve functioning in depressed suburban teenagers may not generalize to city-dwelling adults.  In order to gain broader generalizability, this promising experiment on a limited population should be reproduced in a broader or different population.  If the original study is difficult to reproduce, however, such broader application may prove impossible.

Technological solutions cannot correct recruitment bias or white overrepresentation in research personnel.  Careful use of technology can, however, add to research transparency and reproducibility and promote honest disclosure of challenges to generalizability.

### Reuse

In addition to reproducibility, another important element of research is the ability to reuse assets such as data and methods to related research that may not be a direct replication.  Researchers may hypothesize that a computer vision approach used to analyze moles might be useful as well in other areas of medicine that need edge detection, such as tumor classification.  Longitudinal data that provides rich phenotyping of a cohort of patients with hypermobility syndromes may be useful not just to the original orthopedic researcher community but also to cardiologists interested in comorbid vascular and ANS conditions.  Data reuse allows researchers to collaborate in ways that advance cross-domain knowledge and professional interaction, as well as honoring the time and energy of human subjects whose data can be leveraged to get as much scientific value as possible.

The reuse of data and other research assets has numerous challenges.  You may have experienced problems in this area such as:

* Encountering resistance to sharing data, methods, scripts, or other artifacts
* Data that is not well described or labeled, or is stored in a "supplemental materials" page without context
* Overly strict informed consent documents that prevent researchers from reusing their own data or sharing it with colleagues

Survey results from a recent poll conducted by Arcus librarians and archivists (experts in data reusability) appear to indicate that CHOP researchers generally want to make their data reusable, but believe that regulatory, ethical, or technical constraints prevent them from doing so.  Planning ahead for data reuse is an important part of experimental design, IRB interaction, subject consent, and documentation of data and methods.

<div class = "hint">

Read more: Want to get a quick overview of some of the privacy practices that regulate responsible data sharing?  Check out this brief article: https://education.arcus.chop.edu/privacy-overview/

<div class = "hint">

### A Data Management and Sharing Snafu

This is an approachable and humorous introduction to the practical impact of poor research practices leading to downstream impact.  As you listen, try to identify problematic research practices which could have been prevented by more careful use of technology.  Which of these mistakes have you encountered personally?  Which have you committed?

<iframe width="560" height="315" src="https://www.youtube.com/embed/66oNv_DJuPc?cc_load_policy=1" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Tools for Better Practices

Here we aim to provide a broad overview of how some tools can provide help for

### Scripts

**Scripts**, in this context, are a series of computer code instructions that handle elements of research such as:

* Ingesting data (for example, accessing a .csv file or downloading the latest data from REDCap)
* Reshaping and cleaning data (such as removing rows that don't meet given conditions for completeness or correctness, combining data from two or more sources, or creating a new field using two or more existing fields)
* Reporting statistical characteristics (for example, finding quartiles, median values, or standard deviations)
* Conducting statistical tests (e.g. ANOVA, two-sample t-tests, effect size)
* Creating models (such as a linear or logistic model or a more complex machine learning algorithm like clustering or random forest classification)
* Saving interim datasets (e.g. storing a "cleaned" version of data for use in later steps or creating a deidentified version of data)
* Creating data visualizations (such as boxplots, Q-Q plots, ROCs, and many more)
* Communicating methods and findings in a step-by-step way (e.g. writing a methods section from within the steps of analysis)
* And more....

Scripts may be written in free, open source tools like R, Python, Julia, and Octave, or, with care, can be extracted from commercial tools (such as a syntax file).  It's important to realize that good scripts are complete and don't rely on human memory of steps that aren't recorded in the script.  For example, using a point-and-click solution like Excel to clean data prior to analyzing it using code relies on human memory of what was done in Excel.  

We can contrast scripts with tools that don't record every step explicitly.  Excel is one example we've already touched on.  You may also have been exposed to SAS, SPSS, and Stata, all of which have a point-and-click element as well as the possibility of scripted analysis.  However, most users of these tools depend on un-scripted actions such as cleaning data beforehand in a separate program and the use of point-and-click, menu driven selections.  For this reason, we suggest the use of R and Python for most research purposes.  These are widely used, well-documented and tested, and have a scientific and medical user base that is friendly for beginners.  Additionally, these tools are free and open-source, which allows for greater reproducibility, including in lower-resourced settings.

It's worth considering the words of an archaeological team that wrote an article about reproducible research for a lay audience in a [2017 *Slate* article](https://slate.com/technology/2017/07/how-to-make-a-study-reproducible.html):

>However, while many researchers do this work by pointing and clicking using off-the-shelf software, we tried as much as possible to write scripts in the R programming language.
Pointing and clicking generally leaves no traces of important decisions made during data analysis. Mouse-driven analyses leave the researcher with a final result, but none of the steps to get that result is saved. This makes it difficult to retrace the steps of an analysis, and check the assumptions made by the researcher. <br/> ... <br/>It's easy to understand why many researchers prefer point-and-click over writing scripts for their data analysis. Often that's what they were taught as students. It's hard work and time-consuming to learn new analysis tools among the pressures of teaching, applying for grants, doing fieldwork and writing publications. Despite these challenges, there is an accelerating shift away from point-and-click toward scripted analyses in many areas of science.


### Data management and metadata

**Data management** includes the organization, annotation, and preservation of data and metadata related to your research or clinical project.

Data management is a critical pain point for many data users.  What's the best way to wrangle the data files needed to carry out a project?  Should documents be stored in a common drive?  Using what kinds of subfolders?  How should researchers deal with emails that are sent back and forth between researchers to define a specific cohort?  How should analysts describe and how data is analyzed?  Even a small project organized by a single researcher can be complex, and when a team of several researchers and supporting staff are involved, individual data management practices can collide.  A few topics that fall under the category of data management include:

* File naming standards for project files
* The format in which data is collected, and where it is stored
* How data is backed up and kept private
* Where regulatory files such as protocols are kept
* How processes and procedures are stored and kept up to date
* Who has access to what assets and when that access expires

Importantly, NIH will require data sharing & management plan for all grants starting Jan 2023, and it's worth practicing the skills for developing a robust plan.

**Metadata** is, in its simplest definition, data about data. Some examples of metadata might include:

* Who collected the data?
* When was the data collected?
* What units does the data use?
* What kind of thing is the data measuring?
* What are the expected values?
* Are there any codes for specific cases (e.g. missing vs. unwilling to answer)?

Metadata can be found in many places. Sometimes it's implicit, as when it appears in variable names.  The variable "weight_kg", for example, discloses both the measure and the units. Often metadata is found more fully explained in **data dictionaries** or **codebooks**, where variables in a dataset are described more completely. Sometimes metadata can be found almost in passing, mentioned in an abstract or in the methods section of a paper, or in some descriptive text that accompanies a data download or a figure.

Creating useful metadata is a crucial step in reproducible science.  It's essential in helping run an efficient project involving multiple people, since helpful metadata can help reduce incorrect data collection, recording, and storage.  Metadata can help explain or contextualize some findings (e.g. when the time of day of a blood draw affects lab results).  It can also support the use, discovery, and access of data over time.

Metadata can exist at various levels of a project. Some metadata is overarching and describes an entire project (e.g. the institution that oversaw the data collection), some metadata catalogs and classifies the tools or products of research like datasets, code files, and auxiliary files, while other metadata adheres to a specific data field (e.g. the make and model of a medical device that gave a certain measurement).  

REDCap is one example of software that explicitly creates a data dictionary that includes information such as the name and description of a variable, the kind of input that can appear there (alphanumeric, numeric, email format, etc.), and whether a field could identify a subject or not.  

> Discover:  Arcus has a team of librarians and archivists who have created materials to help CHOP scientists with data management.  See "Research Data Management Resources" at https://www.research.chop.edu/arcus/resources.  These materials tend to be very practical and can be used right away to improve data management!

### Version Control

Version control is the discipline of tracking changes to files in a way that captures important information such as who made the change and why.  It also allows for reversion to previous versions of a file.

Many of us use "home grown" version control systems, such as using file names to capture who most recently added comments to a grant proposal or the date of a particular data download.  The difficulty with this is that each member of a team or lab may use different file naming protocols, and within a few months, the number of files can proliferate wildly.  Collaborators may feel unsure about deleting old versions of files, and data and file hoarding leads to delays and confusion.

Technological solutions for version control have been around for decades, and one system in particular has won the bulk of the market share in version control -- **git**.  Git is free, open source version control software.  Part of the Clinical Decision Support rotation includes a brief training in the use of git, especially in the context of the use of **GitHub** software, which is used by many software and data workers because of how well it integrates with git and expands its capabilities by way of proprietary add-ons.

Git and GitHub are distinct organizations with different products.  Git is a free, open-source version control system, and GitHub is a company that provides services to make it easier to use git for software development and related uses.  GitHub has free tier use as well as paid services.  At CHOP, we have an enterprise version of GitHub that is accessible only on the CHOP network.  Additionally, some CHOP users use GitHub.com, which is a public website run by GitHub.  Finally, many GitHub users find that the Github Desktop software is useful for using git.

### Dependency Management

If you've ever created a slide show in one computer only to have it look terrible in another, you know the problem that dependencies can cause.  Dependencies are requirements such as (in our example) having fonts installed, having a default group of settings turned on, and having access to particular files.  Dependencies that are well-documented and understood will help make research more reproducible.  Dependencies that are undocumented or not known about will inevitably cause problems.  Sometimes it isn't clear whether something is a dependency (this value or program *must* be the same as what you used) or just a circumstance (you used a particular version of Python but there's no reason to think that previous or subsequent versions wouldn't work just as well).  For this reason, recording both known and possible dependencies is a helpful practice. Common dependencies in research and data analytics include:

* Operating system: does your use of particular programs require the use of Microsoft Windows 10 or later?
* Regional data formatting: does your analysis assume that decimal values use a period, not a comma, to set off the decimal value?
* Program versions: did you conduct your analysis in R 3.6?  Have you tried it in 3.7?
* Technical data formatting: does your analysis expect a .csv of data with specified columns holding certain measures?

Dependency management is an approach that ideally makes it easy to determine the precise set of tools and inputs required by your analysis, provides examples where applicable, and potentially includes installation information or a file (like a Dockerfile) that includes the exact versions of code used in the original research, in order to create a container (e.g. a Docker container) that can perform the analysis under the original conditions.  At the very least, every analysis should contain information about what tools were used and which version of each was employed.

## Additional Materials

### John Oliver

A 20-minute long clip from John Oliver's infotainment show "Last Week Tonight" includes off-color language and adult references but is a great introduction to topics including:

* p-hacking
* non-publication of null findings (the "file drawer" problem)
* replication studies
* incentivization problems in research
* scientific communication in mass media
* generalizability
* public support of science
* industry funding

Among many other quotable moments, Oliver drives home the point of how research methods have important public funding and public policy implications.  

>"In science, you don't just get to cherry-pick the parts that justify what what you were going to do anyway! ... And look, this is dangerous... that is what leads people to think that manmade climate change isn't real or that vaccines cause autism...."

To watch this (intermittently NSFW) segment, [watch it directly in YouTube](https://www.youtube.com/watch?v=0Rnq1NpHdmw).

## Homework

1.  Consider a project you have now or are considering as part of your fellowship.  This could be actual research or clinical quality improvement work, or even personal research (such as "determining the best place to move when I retire").  

Assume that you need to be prepared to pass this work off to someone else and don't relish being barraged with daily emails asking questions about what you meant or how to interpret a value.  Start drawing / jotting down a data management plan that includes:

* An overall template of what files will go in what folders, with some sample file names
* A small data dictionary that captures a few important variables of interest
* A plan for how to update documents -- which ones need to have all versions maintained, and which can be overwritten and old versions discarded?

2.  If you don't already have GitHub accounts, please do the following:

* While behind the CHOP firewall, log in to https://github.research.chop.edu and log in (upper right) using your typical CHOP credentials.
* Navigate to https://github.com and create a new account (it's free).
* Consider [downloading Github Desktop](https://desktop.github.com/)

3.  Search online to compare the license costs of tools like Matlab, SAS, SPSS, and Stata.  Are these per-user costs?  

4.  
