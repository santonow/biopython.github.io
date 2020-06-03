---
title: Bayesian phylogenetic inference in BioPython
permalink: wiki/PhyloBayes
layout: wiki
tags:
 - Cookbook
---

Introduction
------------

In this cookbook entry I will briefly explain how bayesian phylogenetic inference works.
Then I will show how one can perform it in BioPython on a simple example: mitochondrial sequences of primates.

Bayesian inference
------------------

All of the bayesian inference is built upon Bayes theorem:

![P(T|X)=\frac{P(X|T)P(T)}{P(X)}](bayes_theorem.png)