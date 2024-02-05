---
layout: post
title: So You Want To Be A PV Performance Engineer
description: Blog
summary: So You Want To Be A PV Performance Engineer
tags: [pv performance modeling, career]
---

### A step-by-step guide for students and engineers early in their non-academic career.

![SandiaImage]({{ site.url }}/assets/images/sandia.png#center)
Figure 1:  PV Performance Modeling Steps from https://pvpmc.sandia.gov/

# Step 0 - Decide if this is a good path for you

In one of my earliest interviews, a future boss of mine asked if I understood what the job I was applying
for entailed.  I told him I would be designing solar energy systems at grid scale.  I would be doing
some basic systems design calculations like voltage drop, string sizing, loading ratios, etc.  
and in a way, I was right.  In most other ways I was wrong, or at best I was very incomplete.  

The goal of a photo-voltaic (PV) performance engineer is to determine how much energy a given solar plant
is going to create.  In order to do so, engineers interact with a set of mathematical models.  These
algorithms model all of the physical effects a solar plant might experience from humidity in the air
changing the spectrum of the incoming light, to light being refracted at an angle off the glass, 
to voltage drop in the line.  The amount of things that affect the output of a solar plant is so enormous
and spans so many different academic disciplines that no person could possibly be an expert in 
all of them straight out of school.

This career path might be a good fit for you if you:
* Have or will have a four year degree in an engineering related field.
* A feeling that your career should positively affect the world.
* Enjoy learning about a broad set of topics.
* Would like a decent chance at working hybrid/remote.

This career path might be a bad fit for you if you:
* Do not want to be on the computer 40 hours per week.
* Want to work with something less abstract than a model.

# Step 1 - Get a Job

So, you have decided that PV Performance modeling seems like a good enough career for you.  
The next step is to find a job.  Entry level jobs in the U.S. will pay anywhere from 60k to 100k USD
and starting out, it is likely a good idea to try to find a job that is in person
which likely means a move to some metropolitan area.

The terms:
 * Solar Engineer
 * Solar Resource Analyst
 * PV Performance Engineer

and any permutation of the words used in those titles are basically equivalent to one another,
and I encourage you to search for job titles that are similar to the ones listed above. 

If you are in this step, you have my sympathy.  Hiring managers receive hundreds of applications
for a given role, and the competition can be fierce.  I have posted internship positions that have
received over 400 applications, some of whom had PhD's.  

Don't lose hope.  Your job is to send a strong signal to the hiring manager that 
you want to be in renewables, that you are curious and hard working, and that you can 
communicate effectively.  The university degree is the bar for entry, not a guaranteed ticket.  

Things that might help set your resume apart:
 * Volunteering at Grid Alternatives.
 * An internship in renewables.
 * A masters degree related to renewable energies. 
 * Past experience in another renewables field such as wind.
 * Past experience in another solar sub-field such as residential, or commercial.
 * Experience with AutoCAD and Excel.
 * Experience with Python.
 * A contribution to the open source pvlib library.
 * A well researched cover letter.


# Step 2 - Become a Super User

Congrats!  You have found gainful employment as a PV performance engineer.
If you are anything like I was, you are completely overwhelmed by the amount of topics
that go into a performance model.  You wonder if you time would be better spent
understanding how lake effect snow may affect your choice of weather station,
or the difference between P and N type solar cells. 

Let me tell you something.  I am, at the time of writing, 9 years into my career
and still asking myself that same question.  The topics at either end of the decision change
day to day and year to year, but the fact remains that this field is large enough
to humble you, even nearly a decade in. 

When learning something new, the approach that works best for me is to learn the same thing multiple times,
in increasing levels of detail.  First, I do a cursory read of the topic or watch a video on youtube.
This helps build a mental scaffolding of the topic that I am trying to learn.  Once you have a scaffold, 
it is much easier fill in the gaps until you have a good understanding of the topic at hand.

I suggest you build a mental scaffold of performance modeling topics by becoming a good user of whichever PV
performance modeling software you choose to use (probably PVSyst).  Don't just learn to press the right buttons
at the right time.  Figure out which models come before and after which other models and why. 
Export the time-step by time-step data and see if you can calculate the values yourself.  Try 
to use the software in a clever way to calculate something that it wasn't explicity 
programmed to do.  

Read all of the help files.  If PVSyst's help files don't give you enough information,
go find another performance modeling software and read their help files on the same topic.
You don't need a subscription to read PlantPredict or SolarFarmer's technical documentation.
System Advisor model (SAM) is literally free.  I put a lot of time into making sure that
the technical documentation at PlantPredict is detailed and easy to read,
and I can tell that the SolarFarmer and SAM teams are doing the same.  It also
goes almost without saying that there is nothing better than pvlib in terms of seeing
exactly what a model does, down the code level.

The field of PV performance modeling has no golden book which encompasses everything you need to know,
but it does have free resources everywhere if you know where to look.

# Step 3 - Find your niche

At this point, the paths that your career can take diverge in many different directions.  
You can go into management, become even more technically proficient, or even move into 
software like I have.  If you are this point, it's likely you don't need
my advice.  On the contrary, if you have any advice that you think I missed, feel free
to comment on Linkedin or send me an email.  I would be happy to include an addendum here!