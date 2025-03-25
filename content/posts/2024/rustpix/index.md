+++
title = "Rustpix"
description = "A lightweight tool to optimize images for the web"
date = 2024-11-18T12:24:38-03:00
tags = ['Blogging', 'Static Site', 'Coding', 'Metadata', 'Open Source']
draft = false
+++

I've just published a new open-source software on GitHub called [Rustpix](https://github.com/a-franca/rustpix). As the name suggests, it's a simple program written entirely in Rust.

Essentially, Rustpix is a lightweight tool that automatically optimizes images for web use.

How it works:

- Takes one or more images in common formats (JPEG, PNG, GIF, etc.) as input.
- Strips EXIF data from the images.
- Converts them to the WebP format with up to 75% quality for lossy compression.
- Renames each file to a random UUID (Universally Unique Identifier) with a .webp extension.
- Overwrites the original files with the optimized versions in the same directory.

The program is still under development, and I plan to add more features in the future.

This isn't meant to reinvent the wheel, as there are already many image optimization tools out there. However, it's a convenient way to streamline my workflow when preparing images for my blog or other online projects.