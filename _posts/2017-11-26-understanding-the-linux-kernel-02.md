---
layout: post
title: "Understanding the Linux Kernel 02"
date: 2017-11-26 15-47-35 +0800
categories: ['Linux']
tags: ['Linux', 'Kernel']
disqus_identifier: 257026711520257894806533149560676763425
---
<small>*对枯燥的事情有所坚持—耗子*</small>

- TOC
{:toc}

- - -

## 2.1 Memory Addresses

Programmers casually refer to a *memory address* as the way to access the contents of a memory cell. But when dealing with Intel 80x86 microprocessors, we have to distinguish among three kinds of addresses:

- **Logical address**

    Included in the machine language instructions to specify the address of an operand or of an instruction. This type of address embodies the well-known Intel segmented architecture that forces MS-DOS and Windows programmers to divide their programs into segments. Each logical address consists a *segment* and an *offset* (or *displacement*) that denotes the distance from the start of the segment to the actual address.

- **Linear address**

    A single 32-bit unsigned integer that can be used to address up 4 GB, that is, up to 4,294,967,296 memory cells. Linear addresses are usually represented in hexadecimal notation; their values range from 0x00000000 to 0xffffffff.

- **Physical address**

    Used to address memory cells included in memory chips. They correspond to the electrical signals sent along the address pins of the microprocessor to the memory bus. Physical addresses are represented as 32-bit unsigned integers.

The CPU control unit transforms a logical address into a linear address by means of a hardware circuit called a *segmentation unit*; successively, a second hardware circuit called a *paging unit* transforms the linear address into a physical address.

![An example of a directory tree]({{ site.baseurl }}/assets/images/understanding-the-linux-kernel/Logical address translation.png)

- - -

### References

1. Daniel P. Bovet、 Marco Cesati (2005-11), "Chapter 2: Understanding the Linux Kernel"
1. Real mode - Wikipedia, [https://en.wikipedia.org/wiki/Real_mode](https://en.wikipedia.org/wiki/Real_mode)
1. Protected mode - Wikipedia, [https://en.wikipedia.org/wiki/Protected_mode](https://en.wikipedia.org/wiki/Protected_mode)
1. Difference between real mode and protected mode - Geek.com, [https://www.geek.com/chips/difference-between-real-mode-and-protected-mode-574665/](https://www.geek.com/chips/difference-between-real-mode-and-protected-mode-574665/)