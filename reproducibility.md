<!--
author:   Joy Payton, Arcus Education, Children's Hospital of Philadelphia
email:    paytonk@chop.edu
version:  0.0.1
language: en
narrator: US English Female
comment:  Reproducibility is essential to scientific efforts.  Generalizability and data reuse are also important for the application and expansion of scientific inquiry. Technology can help achieve these goals.
-->

# Reproducibility, Generalizability, and Reuse

<div style="background-color:#e0ffe0;
padding: 1em;
border: 1px solid grey;">

## Overview

This module provides learners with an approachable introduction to research reproducibility, generalizability, and reuse, and how technical approaches can help.

**Estimated time to completion**: 1 hour

**Pre-requisites**: It is helpful if learners have conducted research, are familiar with peer-reviewed literature, and have experience using data and methods developed by other people.    

**Format**: This module uses text and video and is intended to accompany an in-person or otherwise synchronous presentation.  Materials contained here will allow for review after a live session.

**Learning Objectives**:  After completion of this module, learners will be able to:

* Explain the importance of reproducible methods
* Give an example of a reproducible method in data acquisition and analysis
* Give an example of a reproducible method in data management and metadata

</div>

### Contents

* [Instructor Introduction](#Instructor-Introduction)
* [Reproducibility Crisis](#Reproducibility-Crisis)

### Instructor Introduction

Your live session is facilitated by [Joy Payton](https://linkedin.com/in/joypayton), who leads [data education efforts](https://education.arcus.chop.edu) within the [Arcus program](https://arcus.chop.edu) within the [Research Institute](https://www.research.chop.edu) of the [Children's Hospital of Philadelphia](https://www.chop.edu).

## Concepts

### Reproducibility

The **"reproducibility crisis"** refers to the problem in peer-reviewed research in which studies *cannot* be reproduced or replicated because of insufficient information, or in which studies *fail* to be reproduced or replicated because of preventable problems in the initial research.  This is problematic because it means wasted time and money, reduced public trust in science, unverifiable claims, and lost chances for scientific consensus.

Examples of reproducibility problems exist at small and large scale:

* Experiencing dread at trying to reproduce your own findings a few months after doing it the first time
* Being stymied by a collaborator's cryptic notes that don't explain how to do a particular analysis step
* Being unable to perform required computation because of reliance on expensive, deprecated, or proprietary software or hardware
* Results that don't replicate due to poor statistical practices, such as "p-hacking", "HARKing", or multiple tests without correction

### Generalizability

Research is **generalizable** if findings can be applied to a broad population.  

Historically, biomedical and social science research projects have struggled with generalizability due to unrepresentative data.  For example, the acronym **"WEIRD"** refers to the tendency of psychological studies to rely on subjects (often undergraduate students) who are disproportionately from **W**estern, **E**ducated, **I**ndustrialized, **R**ich, and **D**eveloped cultures -- cultures which, compared with the global population as a whole, are indeed weird.  Until recently, many biomedical studies were conducted on disproportionately male populations and ignored disease presentation or pharmacodynamics in women and girls (or even female lab animals).  In wearable sensor and computer vision development, overwhelmingly white engineers using skewed samples have failed to realize that the optical sensors and computer vision algorithms they created perform less well on dark skin.

The challenge of generalizability is closely linked to reproducibility.  For example, a study that demonstrates the effectiveness of exercise to improve functioning in depressed suburban teenagers may not generalize to city-dwelling adults.  In order to gain broader generalizability, this promising experiment on a limited population should be reproduced in a broader or different population.  If the original study is difficult to reproduce, however, such broader application may prove impossible.

Technological solutions cannot correct recruitment bias or white overrepresentation in research personnel.  Careful use of technology can, however, add to research transparency and reproducibility and promote honest disclosure of challenges to generalizability.

### Reuse

In addition to reproducibility, another important element of research is the ability to reuse assets such as data and methods to related research that may not be a direct replication.  Researchers may hypothesize that a computer vision approach used to analyze moles might be useful as well in other areas of medicine that need edge detection, such as tumor classification.  Longitudinal data that provides rich phenotyping of a cohort of patients with hypermobility syndromes may be useful not just to the original orthopedic researcher community but also to cardiologists interested in comorbid vascular and ANS conditions.  Data reuse allows researchers to collaborate in ways that advance cross-domain knowledge and professional interaction, as well as honoring the time and energy of human subjects whose data can be leveraged to get as much scientific value as possible.

The reuse of data and other research assets has numerous challenges.  You may have experienced problems in this area such as:

* Encountering resistance to sharing data, methods, scripts, or other artifacts
* Data that is not well described or labeled, or is stored in a "supplemental materials" page without context
* Overly strict informed consent documents that prevent researchers from reusing their own data or sharing it with colleagues

### A Data Management and Sharing Snafu

This is an approachable and humorous introduction to the practical impact of poor research practices leading to downstream impact.  As you listen, try to identify problematic research practices which could have been prevented by more careful use of technology.  Which of these mistakes have you encountered personally?  Which have you committed?


<iframe width="560" height="315" src="https://www.youtube.com/embed/66oNv_DJuPc?cc_load_policy=1" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Tools for Better Practices

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
Pointing and clicking generally leaves no traces of important decisions made during data analysis. Mouse-driven analyses leave the researcher with a final result, but none of the steps to get that result is saved. This makes it difficult to retrace the steps of an analysis, and check the assumptions made by the researcher.
...
It's easy to understand why many researchers prefer point-and-click over writing scripts for their data analysis. Often that's what they were taught as students. It's hard work and time-consuming to learn new analysis tools among the pressures of teaching, applying for grants, doing fieldwork and writing publications. Despite these challenges, there is an accelerating shift away from point-and-click toward scripted analyses in many areas of science.


### Data management and metadata

Data management is a critical pain point for many data users.

### Version control

Version control is the discipline of tracking changes to files in a way that captures important information such as who made the change and why.  It also allows for reversion to previous versions of a file.

Many of us use "home grown" version control systems, such as using file names to capture who most recently added comments to a grant proposal or the date of a particular data download.  The difficulty with this is that each member of a team or lab may use different file naming protocols, and within a few months, the number of files can proliferate wildly.  Collaborators may feel unsure about deleting old versions of files, and data and file hoarding leads to delays and confusion.

Technological solutions for version control have been around for decades, and one system in particular has won the bulk of the market share in version control -- **git**.  Git is free, open source version control software.  Part of the Clinical Decision Support rotation includes a brief training in the use of git, especially in the context of the use of GitHub software, which is used by many software and data workers because of how well it integrates with git and expands its capabilities by way of proprietary add-ons.

### Dependencies

If you've ever created a slide show in one computer only to have it look terrible in another, you know the problem that dependencies can cause.  Dependencies are requirements such as (in our example) having fonts installed, having a default group of settings turned on, and having access to particular files.  Dependencies that are well-documented and understood will help make research more reproducible.  Dependencies that are undocumented or not known about will inevitably cause problems.  Sometimes it isn't clear whether something is a dependency (this value or program *must* be the same as what you used) or just a circumstance (you used a particular version of Python but there's no reason to think that previous or subsequent versions wouldn't work just as well).  For this reason, recording both known and possible dependencies is a helpful practice. Common dependencies in research and data analytics include:

* Operating system: does your use of particular programs require the use of Microsoft Windows 10 or later?
* Regional data formatting: does your analysis assume that decimal values use a period, not a comma, to set off the decimal value?
* Program versions: did you conduct your analysis in R 3.6?  Have you tried it in 3.7?

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
