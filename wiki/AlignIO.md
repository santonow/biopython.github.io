---
title: AlignIO
permalink: wiki/AlignIO
layout: wiki
---

This page describes Bio.AlignIO, a new multiple sequence Alignment
Input/Output interface for BioPython which is currently only in our
source code repository. It should be included in the next release of
Biopython.

Aims
----

You may already be familiar with the [Bio.SeqIO](SeqIO "wikilink")
module which deals with files containing one or more sequences
represented as [SeqRecord](SeqRecord "wikilink") objects. The purpose of
the SeqIO module is to provide a simple uniform interface to assorted
file formats.

Similarly, Bio.AlignIO deals with files containing one or more sequence
alignments represented as Alignment objects. Bio.AlignIO uses the same
set of functions for input and output as in Bio.SeqIO, and the same
names for the file formats supported.

Note that the inclusion of Bio.AlignIO does lead to some duplication or
choice in how to deal with some file formats. For example, Bio.SeqIO and
Bio.Clustalw will both read sequences from Clustal files - but
Bio.Clustalw also includes a command line wrapper to call the program.

My vision is that for manipulating sequence alignments you should try
Bio.AlignIO as your first choice. In some cases you may only care about
the sequences themselves, in which case try using
[Bio.SeqIO](Bio.SeqIO "wikilink") on the alignment file directly. Unless
you have some very specific requirements, I hope this should suffice.

Peter