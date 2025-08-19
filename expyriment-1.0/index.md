---
Title: Expyriment 1.0 🚀
Description: At last! But why did it take so long?
Author: Florian Krause & Oliver Lindemann
Authorlink: https://expyriment.org#contact
Date: 2025-08-18
---

**We are excited to announce that a new release of Expyriment is finally here!**
After almost six years, Expyriment 1.0 comes with many new features, important
underlying changes and various bug fixes (see
[release notes](https://github.com/expyriment/expyriment/releases/tag/v1.0.0)
for the full list). As mentioned in our
[10-year anniversary blog post](https://blog.expyriment.org/10-years/),
we had plans for this release already back in 2019. So what happened? Allow me
to explain in more detail and give you a rundown of the timeline since our
last official release.

# COVID, lockdowns, home office
I guess this one is rather self-explanatory. Shortly after we had released 0.10,
in the beginning of 2020 a world-wide pandemic hit and introduced us to the
many challenges of working from home. For some of us (including me) the
multiple lockdowns also meant that we had to dual-task working with childcare
duties, due to daycare closures. Of course labs were also closed and all
research activities had to be paused for a significant amount of time. All this
generated a considerable backlog of tasks and an increased workload once things
started to normalize again many months later.

# Expyriment 0.10 incompatibility with Python > 3.7
Around 2021-2022 we then started to receive multiple reports of users
[having](https://github.com/expyriment/expyriment/issues/168)
[problems](https://github.com/expyriment/expyriment/issues/178)
[installing](https://github.com/expyriment/expyriment/issues/198)
[Expyriment](https://github.com/expyriment/expyriment/issues/197) 0.10. A
closer look quickly indicated what the problem was: The Pygame project had
completely committed to moving forward with Pygame 2 exclusively, and stopped
providing Python wheels of Pygame 1 (which Expyriment relies on) for new
Python version. This meant that from this point forward, Expyriment was only
compatible with Python versions up to and including 3.7. While Expyriment
continued to work perfectly fine if you didn't update your Python version (which
a lot of users hadn't done, and hence were not immediately affected by this),
with Python 3.7 reaching end-of-life in mid 2023, this became a more pressing
issue. Plans were made to move to Pygame 2 immediately, with an intermediate
0.11 release before the big 1.0, in order to mitigate this.

# More Pygame issues
Once we had started to implement the necessary changes in late 2022, initial
testing indicated another major problem: Under Pygame 2, visual stimulus
presentation was not timing-accurate anymore - even when using OpenGL! Given
our
[commitment to timing accuracy](https://blog.expyriment.org/timing-accuracy/),
this was of course unacceptable for us. We quickly discovered that the
underlying issue was an unfortunate combination of two
[implementation](https://github.com/pygame/pygame/issues/3619)
[choices](https://github.com/pygame/pygame/pull/3609) in Pygame 2. However, we
also learned that the Pygame community had since split up into two projects -
the original [Pygame](https://www.pygame.org/news) and a forked
[Pygame Community Edition](https://pyga.me) - which not only did not help in
getting our concerns heard, but also raised the question of which of the two
projects we wanted to move forward with. While we also
[addressed](https://github.com/pygame-community/pygame-ce/issues/2095)
[the](https://github.com/pygame-community/pygame-ce/issues/1778)
[issues](https://github.com/pygame-community/pygame-ce/issues/549) in the new
community edition project, the original Pygame project was eventually faster in
fixing them, and by late 2023 we had restored timing accuracy on most systems.

# Finalising the release
Since then, we were busy with testing, writing documentation, as well as fixing
bugs and implementing other necessary changes that resulted from this
transition (e.g. fixing a small inaccuracy reaction time acquisition, improving
the test suite to better detect estimate a system's stimulus presentation
timing accuracy, overhauling the audio and video playback, and many more).
Unfortunately, this process took also much longer than we had hoped, since both
Oliver and me have limited time to work on Expyriment (often in our free time).
To help with this, we decided that we want to move to more frequent granular
updates in the future. In order to achieve this, we adapted a more modern and
simplified [build system](https://flit.pypa.io/en/stable/) and a fully
automatised release mechanism, based on
[GitHub's continuous integration system](https://github.com/features/actions).
With the [changelog](https://docs.expyriment.org/Changelog.html) for the
initially planned as version 0.11 having grown extensively, and since we had in 
the meantime implemented most of the things we once envisioned for a 1.0
release, we eventually decided to release the new version exactly as such:
**[Expyriment 1.0](https://github.com/expyriment/expyriment/releases/tag/v1.0.0)**!


Thanks to everyone who played a role in making this happen - whether by
[contributing directly](https://github.com/expyriment/expyriment/graphs/contributors),
[reporting issues](https://github.com/expyriment/expyriment/issues?q=is%3Aissue),
or simply helping with testing! We are grateful to see the Expyriment community
continuing to grow, with more scientists actively contributing to the development
of the library. Your support means a lot to us and is what makes Expyriment thrive.
