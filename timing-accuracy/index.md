---
Title: On the timing accuracy of Expyriment
Description: A commentary on Bridges et al. (2020)
Author: Florian Krause & Oliver Lindemann
Date: 2021-06-28
---

**In the article "The timing mega-study: comparing a range of experiment
generators, both lab-based and online" by [Bridges et al. (2020)](https://doi.org/10.7717/peerj.9414), the authors attempted to compare the timing-accuracy of their own stimulus and response time recording software _[PsychoPy](https://www.psychopy.org)_ to that of a variety of other free as well as commercial software solutions. [_Expyriment_](https://www.expyriment.org) was amongst those tested, and not only did it apparenlty perform very poorly, the authors also explicitly advised their readers against using it. We see it as our obligation to our users and fellow researchers to clarify that these results are neither representative nor generalizable.**

---

_A more comprehensive article on this topic is also available as a [preprint](https://psyarxiv.com/k5vd9)._

---


Unlike most of the tested software solutions which constitute full desktop applications with graphical user interfaces, _Expyriment_ is a lightweight and flexible library for the Python programming language. Its purpose is not to provide researchers with an “experiment generator” application, but with the building blocks to programme timing-critical experiments in Python or to develop such applications themselves (e.g. _[OpenSesame](https://osdoc.cogsci.nl)_ or _[TrajTracker](https://www.trajtracker.com)_). While robustness against erroneous usage is an important quality criterion for an experiment generator application, such a criterion is difficult to apply in a programming library, which gives users maximal flexibility, but also responsibility for their implementation and final product. Upon closer examination of the described test procedures as well as the publicly available [supplementary material of Bridges et al. (2020)](https://osf.io/3kx7g/), we identified several issues on both, the conceptual as well as the implementation level, which artificially produced the reported results:

1. Being the main developers and maintainers of one of the tested software solutions constitutes a conflict of interest, even when that software is non-commercial, as it introduces an inherent imbalance in the degree of experience one has with each solution. The authors unfortunately seem to have been particularly unfamiliar with _Expyriment_.

2. Despite the authors’ claims of using each tested software solution according to their respective documentation, the publicly available test scripts, results and raw data show no signs that the authors attempted to verify that the system is set up to allow for timing-accurate visual stimulus presentation in the first place:

    * The reported visual duration times for _Expyriment_ on Ubuntu Linux and MacOS are around half the refresh rate of the corresponding test systems (60 Hz). This indicates that code execution after stimulus presentation was not blocked until the vertical retrace has occurred - an important requirement for a system’s ability to accurately report when exactly a stimulus was being drawn on the screen (see also [Krause & Lindemann, 2014](https://doi.org/10.3758/s13428-013-0390-6)). Notably, _Expyriment_ provides the functionality to explicitely test this as part of its [test suite](https://docs.expyriment.org/Testsuite.html), and we actively encourage users to verify this in the lab.
    
    * The event files written by _Expyriment_ during the reported tests contain the information that, despite the obvious inability of the system to block on the vertical retrace, the authors continued to test with the default method of attempting to block on the vertical retrace, rather than switching to the alternative method as suggested in the official [documentation](https://docs.expyriment.org/Timing.html#visual), which usually solves this issue on systems that fail to block with the default method.
    
    * The article does also not mention any other steps taken to verify, before testing, that the system has been set up as advised, and it is to be expected that the results would have differed significantly, had the authors consulted the documentation, as can be expected from a regular _Expyriment_ user.
 
3. In addition to the apparently non-optimised test setup, the publicly available implementations of the tests themselves unfortunately reveal further critical oversights that actively introduced timing inaccuracies:
   
    * Time-consuming procedures (e.g. stimulus preloading) were performed at timing-critical moments, without compensating for their computation time (in both the stimulus and response tests).
    
    * Presentation of a new visual stimulus was initiated after the full interstimulus interval (e.g. 300 ms) has already passed, while the goal seemed to have been having the new stimulus already on the display at the end of said interval (in the response test).
    
    * The implementation of the response test differed significantly in aspects that directly affected the measurement outcome, with the visual stimulus being presented first, followed by the TTL pulse, followed by the auditory stimulus, compared to other implementations (e.g. the one for _Open Sesame_), where the auditory stimulus was presented first, followed by the visual stimulus, followed by the TTL pulse.
 
    * The measurements of the timing accuracy of auditory stimulus presentation used vastly different settings, with the audio buffer kept at the conservative default of 2048 samples, compared to 1024 samples in _OpenSesame_ and even 128 samples in _PsychoPy_. Since the size of that buffer is directly related to audio latency and jitter, this renders the reported results misleading at best.

4. The authors incorrectly conclude that timing accuracy in _Expyriment_ is generally hindered by its usage of the _[Pygame](https://www.pygame.org)_ library, which has not been optimized for low-latency and high-precision timing. Contrary to that statement, the following is true:

    * Visual stimulus presentation in _Expyriment_ is by default based on [_PyOpenGL_](http://pyopengl.sourceforge.net).
    
    * Both stimulus presentation as well as response monitoring with _Expyriment_ can [demonstrably](https://link.springer.com/article/10.3758%2Fs13428-013-0390-6#Sec9) be millisecond-accurate.

It goes without saying that we would have gladly provided support to the authors, as we do for all our users, and would have assisted with the correct implementation of the test scripts. Unfortunately, we were not approached by the authors at any point in time, and only learned about their efforts through the published article.
