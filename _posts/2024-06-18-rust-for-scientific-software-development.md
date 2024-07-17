---
layout: post
title: Rust for Scientific Software Development
description: Blog
summary: Rust for Scientific Software Development
tags: [pvlib]
---


![Rust and Python]({{ site.url }}/assets/images/rust_and_python.png#center)
**Figure 1:**  Ferris and Python hanging out

# Introduction

Python is the defacto standard for scientific software development, and for good reason.
It is flexible, has a huge ecosystem of libraries just one `pip install` away and generally
allows people who care about science to do science without having to also become
experts in software development.

In a world where humans have limited lifespans and also enjoy activities that don't involve staring for hours at a computer screen, python is an amazing tool that undeservedly gets maligned by professional software developers who don't see the utility of scripting languages, because they themselves don't write scripts.

## Too Long Didn't Read
I am a software developer who has written enterprise software in Python, C# and Rust.  I believe that the execution speed and developer experience of Rust makes it a good choice over Python for enterprise software, especially if that software has a complex domain. I believe that the marginal learning required to use Rust over C# is outweighed by those same benefits.  I believe that Rust's memory management strategies are easy enough for non-professional software developers to understand and write correctly.

## Distinguishing Scripting Languages from Non-Scripting Languages

Scripts in general are programs that are written more than they are run.  Enterprise software on the other hand is read/run more than it is written.

| Scripts | Enterprise Software |
|---------|---------------------|
| Complete a single task | Complete a workflow or set of tasks |
| Written by one or a few | Written by a team |
| Doesn't need to be maintained | Requires constant maintenance |
| Written by domain experts | Written by professional developers |

## Why doesn't everyone use python?

The features of python that make it such a strong scripting language make it a weak choice for enterprise software development.  Photovoltaic software (my domain) companies such as PVSyst (object pascal), SolarFarmer (C#), System Advisor Model (C++) and PVDesign (Java) have all chosen to write their software in a language other than Python, even though the open source pvlib-python exists, not because they don't know about the library.  On the contrary, many of those software companies employ people who are contributors or even maintainers of pvlib.  Those software companies chose a non-scripting language, because like physical engineering, software engineering requires practitioners to make deep trade-off decisions regarding the tools of their craft.  Each of these companies chose to trade the ease of use and head start they would gain from choosing python, for the long term benefits that another programming language would bring.

## Distinguishing Scripting, General Purpose, and Systems Languages

 * **Scripting Languages**:  Python, R, Matlab
 * **General Purpose Languages**:  Java, C#, Go
 * **Systems Languages**: C, C++, Rust

 As you get "closer to the metal" from scripting languages to general purpose languages, to systems languages, the more software development knowledge you must obtain to be productive.  Scripting languages abstract away concepts like "types" and you don't even really need to know much about things like "classes".  General purpose languages abstract away memory management techniques and concepts such as the "heap" and the "stack".  Systems languages give you even more control over the computer, and with that the opportunity to create truly fast software.  But with great power comes great responsibility.  Historically, software developers have wielded this power clumsily, creating security holes and very hard to fix bugs in programs written using systems programming languages.

 Systems programming languages have long been out of reach for scientific subject matter experts for this reason.   If professional software developers have historically not been able to use these languages correctly, how would domain programmers (who must split their attention between their particular domain and software development) do so?

# Rust

## Speed Comparison

Rust I believe, brings the dream of systems software development closer than ever to attainability for subject matter experts.  At a high level, Rust's compiler reads through your code and ensures that no memory management errors have been introduced.  I like to think of the language as a sort of jet engine with training wheels attached.  The training wheels help you to program with confidence while piloting one of the fastest programming languages available to modern developers.

For example, here is a time trial of the NREL 2008 solar position algorithm written in Rust vs calling pvlib python for 1 year of hourly data on my computer.

#### Rust calling into a custom nrel_2008 function
```
let (azimuth_series, zenith_series) = match model {
    SolarPositionModel::NREL2008 => inputs
        .map(|(time_step, t_amb)| {
            nrel_2008(time_step, latitude, longitude, elevation, &pressure, t_amb)
        })
        .collect(),
    SolarPositionModel::FutureModel => {
        panic!("No solar position model exists for this model choice yet, please use NREL2008")
    }
};
```

#### Python calling into pvlib
```
times = pd.date_range(
    '2022-01-01 00:00:00-08:00',
    '2022-12-31 23:00:00-08:00',
    freq='1h'
    )

start = time.time()
spa = pvlib.solarposition.get_solarposition(
    time=times,
    latitude=33.2,
    longitude=-117.35,
    altitude=200,
    temperature=12.0,
    method="nrel_numpy"
)
duration = round((time.time() - start) * 1000, 2)
print(f"{model}: {duration} ms")
```


* **Rust**:  ~12 milliseconds
* **pvlib (numpy)**: ~26 milliseconds


### Caveat
By no means is this a scientific benchmarking test of the two functions.  Function run time is dependent on a host of factors including operating system load, state of the CPU cache, etc.  This test is meant to be indicative only.  The calculation of the solar position is also not the bottleneck in run speed for a photovoltaic performance model which computes 3D shading, but there have been some discussions (https://github.com/pvlib/pvlib-python/issues/1906) about potentially speeding it up.

Why does Rust go faster than pvlib, even though pvlib is calling out to C via the numpy library?  One guess could be that pvlib has to coerce python types to C types.  Another could be that not all of the SPA algorithm code fits neatly into numpy's strict array-like programming model.

## Correctness

Training wheels are a core part of Rust's philosophy, and while the memory management training wheels are semi-mandatory (they can be opt-out in specific "unsafe" blocks), the language gives you many ways to use training wheels to your advantage in other parts of the code.

Writing enterprise software is hard.  There are many more ways to write bad software than there are to write good software, and bad software is much easier and quicker to write than good software.  All commercial software projects ironically have strong incentives for writing bad software.  Upcoming deadlines, endless feature additions, churn of software developers all push projects away from best practices.

Good software is easy to understand, and hard to make mistakes with.  Good software is as simple as possible without being any simpler.  Rust gives you the tools to eliminate some of many doorways to badly written software, and these tools are probably what makes Rust Stackoverflow's most loved language 8 years in a row.

One of the hardest parts of writing enterprise scientific software, is communicating between domain experts and professional software developers.  Software developers have no incentive to learn the domain that they are writing software for, since they can move to a different company in a totally different domain at any time.  Domain experts are incentivized to learn software development, but often cannot spend enough time doing it that they become software development experts as well.

Here is just one of many things that Rust allows you to do to make sure that domain experts and software developers communicate correctly in the code base.

Here is an enumeration in Rust.  Enums are algebraic data types, meaning you can mix different types inside of them.  This enum, named `Pressure` has one variant named `MilliBar` which is a unit type of `f32`, meaning it is a named wrapper around the real `f32` type.

```
/// Pressure in mbar
pub enum Pressure {
MilliBar(f32),
}
```

Simple right?  Why would you want to do this?  Here is another snippet of Rust code, directly copying pvlib.

```
impl Pressure {
    pub fn calculate_from_altitude(elevation: &Elevation) -> Self {

        let Elevation::Meters(elevation) = *elevation;

        let pressure =
            100.0 * ((44_331.514 - elevation as f64) / 11_880.516).powf(1.0 / 0.190_263_2);

        Self::MilliBar(pressure as f32)
    }
}
```

Notice that this method takes another enum called `Elevation`.  Because it is an enum, and Rust forces you to account for all possible variants of an enum when writing code, we were forced to deal with the `Meters` case of elevation.  The same thing will happen when a developer encounters `Pressure` somewhere further down in the code base.  Now domain experts and software developers don't ever have to wonder what units a given variable is in.  This saves us time when developing new functionality on top of the existing code and helps us to not make mistakes.  In enterprise software, where this code may get inherited by a new set of developers who are totally unfamiliar with the domain, this can be incredibly helpful.

# Conclusion

Overall the fundamental trade-offs between Rust and Python are simple.  Rust is slower to write, but runs fast and encourages correctness.  It feels like building a structure with stone.  Python is quick to write, but runs slowly and encourages experimentation.  It feels like building with plywood.

If experimentation or prototyping are your goals, use Python.  If reducing maintenance burden, or increasing speed are your goals, use Rust.  Some may argue that building with stone is better than building with plywood, but a good architect knows when and how to use both.
