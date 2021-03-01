# Introduction

Welcome to the guide for the alt:V C++ Module SDK! This guide will teach you the basics on how to write your own alt:V scripting module
and will explain important concepts and other good-to-know things for the SDK.

## Table of Contents

1. [Requirements](#requirements)
2. [What is the Module SDK](#what-is-the-module-sdk)
3. [How to use the Module SDK](#how-to-use-the-module-sdk)
4. [How to use the Module SDK from C or languages that support C code](#how-to-use-the-module-sdk-from-c-or-languages-that-support-c-code)
5. [The next steps](#the-next-steps)

## Requirements

There are several requirements for this guide, and if you do not fulfill all of them, the guide will be hard to understand and not possible to follow along.

Listed here are the requirements for working through this guide:

- Basic Knowledge of C++ or C
- Basic Knowledge of Git
- A text editor or IDE

If you fulfill all of these requirements you are ready to go to the next section. It is recommended to fulfill all of these requirements before proceding, though.

## What is the Module SDK

The Module SDK is the primary SDK for you to create your module with and interact with the alt:V server or client.
With it you can write a scripting module to add a new scripting language to alt:V.

The SDK is available as a header only library for you to include in your project, the actual code for the SDK lies in the alt:V core and as such is not open source.
So it is very easy to use the SDK, as you only need to include the needed headers in your project and the classes and methods inside the headers can then be used,
once your module is injected into the client or server.

It is possible to both write a server and a client module. Many things are exactly the same on both sides, but for specific features see the [client/](client/) and [server/](server/)
directories.
Because the API is mainly the same, much of your code can be reused so it is possible to write your module in a single project that works for both the server- and the clientside.

## How to use the Module SDK

As stated in the last section, including the Module SDK in your project is very easy as the SDK only consists of header files.
To add the SDK to your project simply clone the [repository](https://github.com/altmp/cpp-sdk) and add it as a submodule to your own project.

The next step is to include the header files in your project somewhere. You can include the `SDK.h` to include all the header files of the SDK.
```c++
#include <cpp-sdk/SDK.h>
```

And that's it! Now you can use the classes and methods provided by the SDK. Don't forget to include this header in a common header file of your project,
or include it in every file invidually.
Of course you can also just only include the header files that you currently need.

## How to use the Module SDK from C or languages that support C code

If you want to use the Module SDK but can not or don't want to use C++, but instead want to use C or any other language that supports C code (or C FFI) to write your module,
then you can use the [C Bindings Generator](https://github.com/7Hazard/altv-capi) by Hazard.
The wrapper autogenerates C bindings for the C++ SDK and provides the ability to use the SDK from any language that supports C FFI.

The guide will assume that the SDK is used from C++ directly, when the C Wrapper is used there will be some changes needed, but the general concepts stay the same.

## The next steps

Now that you are familiar with how to include the SDK in your project, you can now begin using it and creating your module.
The next article will tell you the [first steps of creating your own module](first-steps.md).
