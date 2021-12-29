---
layout: post
title: "Metal for Compute"
---

**Note**: This blog post is a Work in Progress.

This blog post describes how to use Apple's Metal API for computation on a GPU in a C++ project.
The accompanying repository is [say4n/metal.compute](https://github.com/say4n/metal.compute).
Be sure to check it out!

## before we get started

Head over to Apple's [getting started with `metal-cpp` guide](https://developer.apple.com/metal/cpp/) and follow the
instruction in the section titled "**Step 1. Prepare your Mac**".
Additionally, make sure to also follow the part of the instruction that describe how to use metal-cpp as a single
header include (section titled "**Metal-cpp single header alternative**") in a project.

We will be using Xcode for development, I used version 13.2.1 (13C100).

## getting started

At this stage, this post assumes that you have the `metal-cpp` sources extracted to a convenient location and that you
have executed the command to use `metal-cpp` as a single header include by following the steps described previously.

The next step is to set up an Xcode C++ project.