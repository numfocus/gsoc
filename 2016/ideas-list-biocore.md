# biocore

## QIIME 2: a general purpose next-generation sequence pre-processor

Various quality control and other sequence "pre-processing" steps are required of microbiome "next-generation" sequencing data before it can used in a QIIME analysis. We propose the development of a general purpose sequence pre-processor that would be developed as a [QIIME 2 plugin](https://github.com/biocore/qiime2). This is described in detail in an issue [here](https://github.com/biocore/qiime/issues/1954).

## scikit-bio: graphical file format converter

In bioinformatics, there are many defined file formats that represent very similar data. scikit-bio has a powerful I/O framework that enables users to load diverse formats into their relevant in-memory representations, regardless on input file format. It would be useful for many bioinformatics end-users (who are often not comfortable working with APIs) to have a file format converter for formats supported in scikit-bio. We'd want to develop a scikit-bio Python API for file format conversions and then develop a simple CLI and/or GUI app to wrap this (this app would likely be separate from scikit-bio). This project would also likely involve the development of additional file format readers and writers for scikit-bio, to increase the power of this application and of scikit-bio.
