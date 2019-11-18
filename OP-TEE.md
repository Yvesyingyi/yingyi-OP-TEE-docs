# OP-TEE Note

This is the one and only OP-TEE note, serving both as a reminder for myself of
what I have learned and the official report of the progress I have made in the
case of OP-TEE. All OP-TEE related notes goes here, not in any other files. For
better clarity, as well as to avoid duplicate information, no content should be
copy-pasted directly from the Internet.

## Rationale

The goal of this is to understand the **code** in [Trusted side of the
TEE](https://github.com/OP-TEE/optee_os) in order to gain insight of how to
prove this system is safe. Proofs using Isabelle/HOL and/or autocorres is
preferred. The initial focus should be on memory, especially on shared
memories, and secure boot. After that, focus on threading. If there are
communications between threads in the secure world, or between one thread in
the secure world and another thread in the normal world, I should pay
attention. The general rule of focus is: if the subject is in need for security
proof, it's a focus; if a subject can possibly help the proof e.g. make it
easier, it's a focus. Also focus on trusted applications: my first goal is to
get a minimalist application up and running, while having insights on the
background processes.

## Resources

All of the resources for this note should be listed here.

- Official OP-TEE documentation
  - [HTML version](https://optee.readthedocs.io/general/index.html)
  - [PDF version](https://buildmedia.readthedocs.org/media/pdf/optee/latest/optee.pdf)

## Basics Concepts

### A Quick Scan of OP-TEE

TEE stands for Trusted Execution Environment. OP-TEE is an open-source solution
of TEE. It provides security and isolation to the system. By design, it relys
on the ARM Trustzone technology, which is a hardware isolation mechanism; but
it also supports different mechanisms such as VMs and seperate CPUs.

### About Shared Memory

TBD

## Advanced Concepts

TBD

## Build

