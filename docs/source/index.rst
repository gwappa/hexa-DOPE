.. hexa-DOPE documentation master file, created by
   sphinx-quickstart on Fri Nov 15 16:27:34 2019.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

.. role:: raw-html(raw)
  :format: html

Welcome to hexa-DOPE!
======================

"hexa-DOPE" (or simply "DOPE"), standing for "hierarchial, extensible and
analysis-ready data organization for physiology experiments".

hexa-DOPE is part of the `physiology data organization project <https://physiology-data-organization.readthedocs.io/>`_
(`project page <https://github.com/gwappa/physiology-data-organization>`_).

.. toctree::
   :maxdepth: 2
   :caption: Contents:

.. contents:: In this page:
    :local:

Introducing hexa-DOPE
----------------------

.. contents:: In this section:
    :local:

Objectives
^^^^^^^^^^^

To share datasets with others (including the future self), it is important that
*the data organization makes sense to the others no matter the type of experiments*.

On the other hand, especially in the field of physiology, experimental procedures,
acquisition setups, or even data structures, are typically custom-made by the experimenter(s).
As a consequence, data files can get scattered over time, with an ad-hoc naming convention
that "works well for a person", using yet-to-be-defined data structures.

Here, we set the aims of defining hexa-DOPE as generation of an open, generic data structure,
such that it:

1. **helps experimenters organize their own datasets.**
2. **helps analysts perform their analytical procedures.**
3. **helps researchers understand the others' datasets.**
4. **can be read and written by as many programs as possible.**
5. **can be modular, being able to be adopted partially.**
6. **allows imperfection of everybody involved.**

As such, it may potentially be used as a foundation for open-data physiology.

Related work
^^^^^^^^^^^^^

.. _BIDS:

Brain-Imaging Data Structure (BIDS)
....................................

Development of hexa-DOPE is under a huge influence of
`Brain-Imaging Data Structure (BIDS) <https://bids-specification.readthedocs.io/en/stable/>`_.

The concepts behind BIDS can be easily grasped by reading the original paper
(`Gorgolewski et al., 2016 Sci Data`_;
also see the figure below).
It seems to have nicely conceptualized the essence of the experiments in the field
of functional brain imaging (i.e. fMRI), and mapped them onto a simple directory structure.
By now, the BIDS format has been widely adopted by the people in the field,
and renders meta-analyses in this field easier.

.. figure:: /_static/images/ref/BIDS.png
    :scale:    60%
    :align:    center
    :figwidth: 70%

    **Figure 1. A typical file structure in BIDS.**
    The root dataset directory is comprised of the "subject directory"
    and a table file containing metadata on each of the subjects (participants).
    Located under the subject directory are data directories dedicated for
    experiments of specific purposes (e.g. "anat" for anatomical imaging, and
    "func" for functional imaging). The figure is translated from `Gorgolewski et al., 2016 Sci Data`_.

.. _Gorgolewski et al., 2016 Sci Data: https://dx.doi.org/10.1038/sdata.2016.44

Care was taken during the attempts to "translate" the BIDS format into the field of physiology,
to define hexa-DOPE. In particular, the structure of experiments may be more complex in the
case of physiology experiments:

- **Dominant data formats differ from experiments and experiments**. May be an image,
  may be a text file, may be a spreadsheet, or a binary file in a proprietary format.
- **An experimenter typically makes use of more than one program simultaneously** during
  data acquisition. Different programs not only engage different file formats but also
  usually require data alignment between each other during the analytical procedures.
- **Data-deriving procedures can be diverse**. Many of recent behavioral experiments have
  the "training" and "acquisition" phases, in addition to the "surgery" phase, where one
  may utilize some program to locate e.g. brain areas of interest.
- **The unit of "an experiment" can differ between projects**. Some projects may be
  interested solely on the behavior of single neurons, whereas others may be interested
  more on the behavior of animals, and even both.

Open metadata markup language (odML)
.....................................

Another approach of open-data physiology was made by
`Open metadata markup language (odML) <http://g-node.github.io/python-odml/>`_.
As its name suggests, odML is a toolkit for defining metadata during experiments
(`Grewe et al., 2011 Front Neuroinform`_). Although it is not strictly a format
for data organization, it would be of relevance when it comes to *how one can
understand a given dataset*, as odML makes a stark contrast from what BIDS does.

Understanding the diversity of physiological metadata (i.e. parameters and conditions
an experimenter sets during experiments), the authors started off by defining the
basic ontology that any metadata must conform to (the figure below, top). By defining a "tree of metadata",
by abstracting the tree of hardwares and softwares in one's setup, one can efficiently
describe a set of metadata during physiology experiments (the figure below, bottom).

.. figure:: /_static/images/ref/odML_markup.jpg
    :scale:    75%
    :align:    center
    :figwidth: 70%

    **Figure 2. The basic structure defined in odML**. The *root section* contains the
    information about the author(s) of this metadata, as well as its child metadata sections.
    The metadata is organized as a *tree of sections*, each of which containing a
    set of dedicated *property*. The relationship between a section and a property
    may be though of as the one between a device and one of its parameters.
    Each property can hold a *value* consisting not only of the value itself
    but also of its precision (*uncertainty*) its physical unit etc.
    The figure is translated from `Grewe et al., 2011 Front Neuroinform`_.

.. figure:: /_static/images/ref/odML_example.jpg
    :scale:    75%
    :align:    center
    :figwidth: 70%

    **Figure 3. An example metadata definition in odML**. The tree
    structure represents a hierarchy of domains of hardware metadata,
    whereas the items starting with minus signs represent their properties.
    The figure is translated from `Grewe et al., 2011 Front Neuroinform`_.

.. _Grewe et al., 2011 Front Neuroinform: https://dx.doi.org/10.3389/fninf.2011.00016

Whereas BIDS specifies the structure in a top-down manner, odML does the same thing
in a bottom-up manner, resulting in a flexible metadata organization. Another feature
of bottom-up definition is that **one can create your own metadata definitions (i.e. "ontology")
that suits your needs**. In principle, you can define whatever you need to describe your
experiments, without damaging the integrity of your metadata organization. If it is nicely
organized, as long as this piece of metadata is available besides your data, you will
not get lost finding out what was done during the experiment.

While its bottom-up nature provides flexibility to odML and the vocabulary used inside it,
odML often suffers from the lack of comprehensive structures. Structuring the data and
metadata is a sort of art, involving careful design decisions, so that they make sense
to as many people as possible. However, **the burden of creating metadata structure**
often comes to the shoulders of each experimenter. As a result, **metadata organization
can become too customized for one single type of experiments**, and does not generalize
to (even slightly) different types. We believe that some design decisions must be made
as to data organization strategy so that the datasets make sense to the other people.

Neurodata Without Borders for Neurophysiology (NWB:N)
......................................................

`Neurodata Without Borders (NWB) <https://www.nwb.org/>`_ is the most recent,
collective efforts to specify a generic format in the field of neurophysiology
(`Teeters et al., 2015 Neuron`_). Having reviewed advantages and disadvantages of
the previous attempts including BIDS and odML, the aim of NWB seems to be to
**contain the whole data environment from experiments to analysis inside one single file**
(see the figure below).

.. figure:: /_static/images/ref/NWB_overview.png
    :scale:    40%
    :align:    center
    :figwidth: 80%

    **Figure 4. An overview of NWB (more specifically, the NWB:N format).**
    It is capable of including all the information ranging from subjects,
    experimental designs, data acquisition setups, data, and post-hoc analytical
    procedures.
    The figure is translated from `the NWB website <https://www.nwb.org/nwb-neurophysiology/>`_.

.. figure:: /_static/images/ref/NWB_file.jpg
    :scale:    80%
    :align:    center
    :figwidth: 80%

    **Figure 5. An example structure inside a NWB:N file.**
    A single NWB:N file takes the form of a HDF5 file, containing entries (similar
    to files) and groups (similar to directories). As it is described in the figure,
    you can pack diverse information, categorized by its type.
    The figure is translated from `Teeters et al., 2015 Neuron`_.

.. _Teeters et al., 2015 Neuron: https://doi.org/10.1016/j.neuron.2015.10.025

One improvement from BIDS is that **any data can point to any metadata**, and vice versa.
This is because of the file format (HDF5) they use. The HDF5 format is so versatile that
one can virtually contain another "file system"-like structure inside a file, including
the "symbolic link" capability. By using this format, the linkage between data and metadata
will never be lost.

In addition to this capability, NWB can hold derived data (data being
generated as a result of analytical procedures), just like BIDS does. By using the scheme
of NWB, one can take control over all the transformation from data to the final results of
the analyses.

In comparison to odML, a NWB file has **the defined structure standards** (see the figure above).
As a result, a complete novice can identify which group represents what type of data / metadata.

Meanwhile, NWB files can suffer from **issues related to file handling**.

First, as a single HDF5 file contains virtually *all* the data and metadata,
its size tends to become *really* large. If you decide to include the raw imaging data
or the raw data from neuropixels, the file size can easily become at the order of
terabytes (TB), no matter whether you are interested in the raw data during
the analysis phase. You will need, then, to either download terabytes of data,
or to access the computation center through the VPN, every time you want to start
analyzing the datasets. In either case, it can restrict the way how data is analyzed.

Second, version-controlling of datasets is relatively complex.
For example, it is difficult to completely remove a dataset (an entry) from a HDF5 file,
except for re-generating the whole file from scratch. This is a troublesome issue
when it comes to generating a large file every time. In addition, version-control
system (such as Git) does not nicely work with binary files, including HDF5.
If you want to keep the history of evolution of your analytical methods,
all you can do is to store every different versions of this single HDF5 file.

While the data format itself may be portable and complete, it is **not trial-and-error-friendly**
when one wants to perform lots of explorative analysis with the datasets.

Design decisions
^^^^^^^^^^^^^^^^^

There are several important design decisions in specification of hexa-DOPE.
They stem from our intent of **keeping the format as simple and concise as possible**
so that it is easy for any researchers (experimenters and analysts, novice and expert)
to understand the dataset at first glance.
Despite the potential issues or problems by keeping the format simple, we believe
that they actually help the culture of open data thrive.

Minimally prescriptive
.......................

Without any top-down organization, experimenters can be lost from where to
start preparing their datasets. Besides, it will hinder future data-analysis
programs parse the structure of datasets.

On the other hand, organizing datasets too much results in reduced flexibility
and reduced expressiveness of the data-organization scheme.

Thus, as being adoped by :ref:`BIDS <BIDS>`, we need **a minimal set of
requirements for the organization, reflecting the common structure across
physiology experiments** (e.g. chronological organization; see later descriptions).
We believe that having these requirements help researchers organize their
own datasets, and thus, their experiments as well.
Having a common structure will also help people understand the others' datasets easier.

Based on the file system
.........................

Having all the datasets in one single file hinders one from download a fraction of it
and try a small off-line analysis. Therefore, we believe that an open-data
organization scheme must be built on top of the file system, rather than inside a
single file, so that anyone can start inspecting the others' datasets by downloading
a small number of files. In addition, it will help experimenters and analysts
update the dataset easier.

Having data files separately implies the risk for the files to scatter over time.
We would still argue that advantages of having data separately exceed its disadvantages.
We would also suggest several possible counter-measures, such as:

- maintaining the whole dataset under a version-control system (e.g. Git).
- including relevant metadata under each child directory.

Simple contextual description
..............................

Description of experimental conditions comprises the core of scientific experiments.
Any dataset will lose its meaning without these descriptions, in the same way
as any experimenter is careful about the experimental conditions of his/her subjects.
Normally, such conditions are carefully factorized into conceptually simple elements
so that the results of experiments lead to clear conclusions.

On the other hand, it is surprisingly difficult to formally define these experimental
contexts based on well-defined vocabulary (cf. `Zehl et al., 2016 Front Neuroinform`_
for an example; see the figure below). Even a simple experiment is based on a
lot of contextual information: what species and/or strains were used for the study,
what type of procedures were performed in what order, how stimuli were presented
to the subjects, and what type of responses from the subjects were recorded using
what configurations and wirings etc. Formal definition of a set of experimental conditions
means in reality to describe the experiment as a whole. For the time being,
it is a very difficult (if not impossible) work to achieve.

.. figure:: /_static/images/ref/Zehl2016fninf.jpg
    :scale:    50%
    :align:    center
    :figwidth: 80%

    **Figure 6. An attempt to describe the context of an experiment.**
    Note how much information is required to describe a (relatively simple)
    physiology experiment.
    The figure is translated from `Zehl et al., 2016 Front Neuroinform`_.

Recognizing the difficulty of describing all the connotations behind the experimental context,
we decided to **refer to experimental conditions by using abstract keywords** that may not be so
well-defined (albeit professionally adopted). This approach is better for experimenters
as this is exactly how they think of their experimental conditions.
Our approach will still lack a proper definition of what these abstract terms mean
(in a sense a program would "understand"), and we have to wait for another generic solution
that subsumes the hexa-DOPE format, and efficiently describes the whole experimental context
at the same time.

.. _Zehl et al., 2016 Front Neuroinform: https://doi.org/10.3389/fninf.2016.00026


hexa-DOPE specification
------------------------

.. contents:: In this section:
    :local:

Concepts
^^^^^^^^^

.. admonition:: TODO

    - Common structure: subject (animal), session, program (domain),
      run, channel, part vs. trial
    - Variables: subject-level, session-level, trial-level

Dataset organization
^^^^^^^^^^^^^^^^^^^^^

Directory organization
.......................

.. admonition:: TODO

    - reflect the common structure
    - naming conventions: based on BIDS

Metadata files
...............

.. admonition:: TODO

    1. info on repository (datasets) and the project: json is used
    2. tables reflecting the context of variables: tsv or csv is used
    3. metadata on experiments and stimulus protocols: use images

Helper programs
----------------

.. contents:: In this section:
    :local:

.. admonition:: TODO

    is any other program coming? e.g. writer program, library for Matlab / Igor

Python reader module
^^^^^^^^^^^^^^^^^^^^^

.. admonition:: TODO

    module name? what is missing still?


.. figure:: https://i.creativecommons.org/l/by/4.0/88x31.png
    :alt: License: CC-BY 4.0
    :align: center
    :figwidth: 60%

    This document is licensed under a :raw-html:`<br />`
    `Creative Commons Attribution 4.0 International License <http://creativecommons.org/licenses/by/4.0/>`_.

..
    Indices and tables
    ==================

    * :ref:`genindex`
    * :ref:`modindex`
    * :ref:`search`
