# Clinical Informatics Fellowship Training

Welcome to the public repository for some of the training materials associated with the Children's Hospital of Philadelphia Clinical Informatics Fellowship.  These materials are publicly available for anyone to use, but are built with this particular use case in mind.

This repository is **public** and cannot contain trade secrets, PHI, PII, or commercializable intellectual property belonging to CHOP.  Not all training materials can be hosted here because of these limitations.  Links to other training materials in secure locations will be provided to program participants.

## Links to Repository Contents

||||
|--|--|--|
|Reproducibility, Generalizability, and Reuse: How Technology Can Help |[Training Course](https://liascript.io/course/?https://raw.githubusercontent.com/arcus/education_fellows/main/reproducibility.md) | [Markdown](reproducibility.md)|
SQL 101: Using Arcus Labs to Explore Clinical Data at CHOP|||
Introduction to R and RStudio||||
R Markdown for Scientific Communication |||
Introduction to Arcus for the Clinician Researcher |||


## About These Materials

The training materials here are built in [markdown](https://www.markdownguide.org/), and you can use the Liascript markdown renderer to make them into attractive, configurable training modules.  To use Liascript, go to https://liascript.io and enter the url of the (raw) markdown.

For example, to use the training included in [reproducibility.md](reproducibility.md), navigate to https://liascript.io add the url for the "raw" version of [reproducibility.md](reproducibility.md), which is https://raw.githubusercontent.com/arcus/education_fellows/main/reproducibility.md.  Together, that gives you this combined url: https://liascript.io/course/?https://raw.githubusercontent.com/arcus/education_fellows/main/reproducibility.md

## A Few Notes and Caveats

If you use .css or .js scripts, it's important to know that you can't supply these scripts directly from GitHub.  GitHub does not provide the appropriate metadata to indicate that the content of the files is of the correct type, which means that browsers won't include them.

One solution is to use a CDN that re-packages GitHub contents for use in a web page.  For example, [Liascript itself suggests using a CDN](https://github.com/liaScript/custom-style/) to provide a CSS script from GitHub.

Since CDNs generally have a refresh rate that doesn't meet the "let's try this really quickly" pace of development in GitHub, another option is to host your script in another publicly accessible location.  In our case, we've made use of the AWS S3 service to host a custom css file.  This means updating the S3 version of the file as well as the version hosted here, in this GitHub, but the extra headache is worth it.

Custom css is what allows us extra bells and whistles such as the CHOP Research Institute logo, custom "question" and "hint" boxes and enhanced block quote sections.
