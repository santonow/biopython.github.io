---
title: Continuous integration
permalink: wiki/Continuous_integration
layout: wiki
---

This is a temporary page documenting efforts to provide a continuous
integration platform for Biopython. This effort is ad-hoc for now and
this page will probably be deleted in the future.

Currently using [Buildbot](http://buildbot.net). The main caveat is the
requirement of a public accessible server running buildbot (twister
based)

Workflow
--------

[Buildbot's
architecture](http://buildbot.net/buildbot/docs/current/full.html) might
be important to understand the next paragraphs.

The starting point for any continuous integration will be any change
made to the main repository. In Biopython's case, this is based on
github.

github can inform buildbot that a change was committed using [Post
receive service hooks](http://help.github.com/post-receive-hooks/)
(Admin&gt;Service Hooks on the Biopython github interface). This means
that whenever there is a change, github can POST that to a list of
specified URLs. One of those URLs will be a script that will call the
buildbot master, informing that there are changes to the source.
Buildbot can now act.

Buildbot is split in two parts: The master and (potentially several)
slaves.

The master:

1.  Is informed of repository changes
2.  Makes decisions on what should be tested and when (schedules work
    for the slaves)
3.  Informs the outside world of results (via email, web, irc, ...)
4.  Has to be visible as a server with a public address

The slaves:

1.  Do the actual integration testing
2.  Can be donated by volunteers that want to help test for a specific
    platform
3.  Can be run anywhere
4.  We need at least 3 (one per major OS)

Before Buildbot
---------------

Before buildbot we have git. We need to inform buildbot of git changes.
Recommended read is: [github/buildbot
integration](http://www.apparatusproject.org/blog/2009/06/github-and-buildbot-continuous-integration/).

While, from an architectural point of view it might seem
convoluted/complex. The implementations is actually quite simple: a
simple entry in github, a small cgi script and configuring buildbot to
accept github's source.

The only problem is that Buildbot has no native support for github (at
least to be informed of changes -- buildbot can download github code),
so the solution is the aforementioned cgi script. For reference the
correct source in buildbot is the generic PBChangeSource (see below)

Builbot: general considerations
-------------------------------

Typical usage. Command line (configure, start, reconfig). Different
versions, different configurations

Buildbot master
---------------

Buildbot master is arguably the most complex part for configuration.
Most decisions are made at this level. The suggestions below are just
that, suggestions. A possible starting point.

The configuration is divided in:

### Slaves

The list of slaves. Only listed slaves will be distributed work. Each
slave has a password. Not a serious security issue (the probable worse
thing that can happen is bogus reports from slaves)

### Sources

The build sources. In our case Biopython's github. As there is no native
support, we use the standard PBChangeSource.

### Schedulers

Scheduling builders is where many decisions will fall upon. There are
many options like a fast scheduling to test very recent changes with
simple updates as soon as possible, or a daily build, doing a complete
download.

Schedulers also seem to change from version to version of buildbot.

Currently there is a single periodic scheduler (once a day).

### Builders

A builder is responsible for testing the source in a certain
environment. The environment can be defined by things like the
Python/Jython version, the OS, but also if it is a fast or slow build.
The number of builders can explode quite easily: lets say 5 Python
versions (2.5, 2.6, 2.7, 3.1 plus Jython 2.5.2) times 3 OSes times 2
build types (fast and slow) and where are at 30 builds.

The two most important parameters of a build are: the slave that
implements the build and the list of steps the slave has to do. There
are typically:

1.  Get the source (either a light update or a full download), for this
    buildbot has github support.
2.  Compile the source
3.  Run the tests

### Status targets

### Identity

### Debugging