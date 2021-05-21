# BeeYourself

Experiments in porting Powerlang/Bee-DMR bootstrap to Cuis

Nota Bene: _very_ pre-Alpha 

Background:
- http://esug.org/data/ESUG2014/IWST/Papers/iwst2014_Design%20and%20implementation%20of%20Bee%20Smalltalk%20Runtime.pdf
- http://stefan-marr.de/downloads/dls17-pimas-et-al-garbage-collection-and-efficiency-in-dynamic-metacircular-runtimes.pdf
- https://www.slideshare.net/esug/bee-smalltalk-runtime-anchors-aweigh
- https://2017.splashcon.org/details/meta-2017/3/Metaphysics-Towards-a-Robust-Framework-for-Remotely-Working-with-Potentially-Broken-
- https://powerlang.readthedocs.io/en/latest/index.html
- https://github.com/powerlang
- https://www.youtube.com/watch?v=ZWPMBSvYrs8 [Smalltalk VM Hackathon]

![Bee Yourself](BeeGraphic.png)

## Scattered Thoughts

We explain the world through stories.

- In pushing compute interface from a virtual machine down into actual hardware, we need to (re)present the HW compute context in the runtime.
- In particular the Debug Context needs to be improved.
- We need to present runtime services as telling _stories_, stories written in Smalltalk.
- Compiler optimization by program transformation is optimization by stories with measurement to see what pays for itself. [Efficient => apt metaphor]
- Smalltalk is a self-presenting mediator between hardware and software.

Where does this lead us?

Thread as VCPU (Virtual CPU), when running is assigned to real CPU (core). VCPU object follows Bee pattern: OOP points just past header to register/CPU-state save area, followed by Thread Local Storage (e.g. per-thread dynamic context).

When a Debug Thread is running, the debugged thread is not, so VCPU registers are directly inspectable as instance variables.

The natural model is to think of the runtime like a RTOS (Real Time OS) Kernel.  Starting an image is spawning a Primordial/Home/Mother thread which spawns worker threads (including UI, debugger(s), ?timer?, ?gc?).  HW interrupt reflected through Mother Thread to events.

Smalltalk is the mediator between raw HW and its SW presentation.

Smalltalk is a system that knows itself.**

The stories written in Smalltalk explain the computational world view.

## Machine <<==Smalltalk==>> Presentation

Stack <<=========>> Linked Contexts

Regs+Mem <<=====>> Objects+ivars/slots

Instructions <<=====>> Operations/[Macro]Opcodes


## Visualize & Explain Runtime Structures & Strategies

- Hash & Cache
  - PICs
  - Symbol interning
  - Dictionaries
- Constat Propagation
- Object Layouts
- Horizontal (bitstring) vs Vertical (index) Encoding
- Control flow
  - Interrupts; exceptions
- GC
  - Storage allocation
  - Pinned objects
  - When is it stable?

## Find Still/Save Points

- Don't change the code for a method while the method is being executed
[ e.g. during message-send ]

- Don't add/remove elements of a collection as it is being traversed
- Don't move (gc) objects while they are being referenced
- Note live-update rubrics; atomics; lock-free/wait-free

## Learn Smalltalk = Learn all of Computer Science

- Recursion
- Color
- Time(r)
- Emergent Properties
- Compile

--

** "What is a System?  Simply a set of things that are interconnected in ways that produce distinct patterns of behavior." _Doughnut Economics_, Kate Raworth.
