# First Steps for your own module

It is now time to start using the SDK to create your own module, this article will teach you the first steps to creating and running your module.

## Table of Contents

1. [Setup](#setup)
2. [Creating the entry file](#creating-the-entry-file)
3. [Needed module exports](#needed-module-exports)
4. [Creating your own runtime](#creating-your-own-runtime)

## Setup

Firstly, you should create your C++ project. What compiler, IDE etc. you use is completely up to you, as long as it outputs a 64-bit DLL it's good to go.

Make sure to correctly set the include path so you can import the SDK header files from your project.
Also make sure your compiler is configured to support C++17 features, otherwise using the SDK will not work.

Next, add the `ALT_SERVER_API` or `ALT_CLIENT_API` define to your compiler or in the main header file of your code, this enables specific features
in the SDK.

Your main header file should now look something like this:
```c++
#define ALT_SERVER_API

#include "cpp-sdk/SDK.h"
```

This is the bare minimum needed setup for your module.

## Creating the entry file

Now you need to create your first `.cpp` file, where your entrypoint function will be.
The entrypoint function is called by the alt:V core when your module is loaded, from that entrypoint function you need to start your module.

On serverside the needed entry function is called `altMain` and it returns a bool whether the module could be started or it failed.
This function receives the alt:V core instance as the first argument, you need to set the received instance as the used instance for the SDK.
You also need to register your runtime as a script runtime in this function.
The `altMain` function should then look something like this:
```c++
// The 'EXPORT' macro is defined by the SDK
// It is needed so the alt:V core can call this function
EXPORT bool altMain(alt::ICore* core)
{   
    // Important! Sets the core for the SDK to the received instance, so you can interact with the server or client
    alt::ICore::SetInstance(core);

    // Pseudocode of creating a runtime,
    // how to create your own runtime will be explained in another article
    auto runtime = new MyRuntime();
    // Register the script runtime
    // The first argument is the 'type' for your module,
    // that needs to be specified in the resource.cfg
    // The second argument is a pointer to your runtime instance
    core->RegisterScriptRuntime("myModule", runtime);

    // Logs an info message to the console
    core->LogInfo("Loaded my module!");

    // Return true to indicate that the module loaded successfully
    return true;
}
```

On clientside it works a little different, you need to export an entrypoint function called `CreateScriptRuntime` that returns a pointer to an instance of your runtime.
Like on serverside, it receives the alt:V core as the first argument, you also need to set the core instance on the clientside.
The `CreateScriptRuntime` function should look something like this:
```c++
EXPORT alt::IScriptRuntime* CreateScriptRuntime(alt::ICore* core)
{
    // Important! Sets the core for the SDK to the received instance, so you can interact with the server or client
    alt::ICore::SetInstance(core);

    // Pseudocode of creating a runtime,
    // how to create your own runtime will be explained in another article
    auto runtime = new MyRuntime();

    // Return the pointer to our runtime
    // If it fails, return nullptr
    return runtime;
}
```

The `alt::ICore` is the main interface for interacting with the server or client, it provides the necessary functions like getting all entities, creating a vehicle etc.

> You should only ever create a single instance of your runtime in the entrypoint function, the function will only be called once when the server or client starts
> and is the only place where you should create create an instance of your runtime.

## Needed module exports

The module doesn't only need the entrypoint function as an export, on both sides there is also another export needed.
You need to export the `GetSDKVersion` function, which returns the version of the SDK the module was compiled with.
This is needed so the alt:V core can check if the used module SDK version is compatible with the alt:V core SDK version.
You just need to return `alt::ICore::SDK_VERSION` from this function.
The function should look like this:
```c++
EXPORT uint32_t GetSDKVersion()
{
    return alt::ICore::SDK_VERSION;
}
```

> Remember that your module always needs to be on the same SDK version as the SDK version used by the alt:V core. Otherwise your module will not load.

On clientside you also need another export called `GetType` that defines what the `type` of the module is.
That is the `client-type` that needs to be specified in the `resource.cfg` for your module to be used for the resource.
This function should return a `const char*` of the module type.
It should look like this:
```c++
EXPORT const char* GetType()
{
    return "myModule";
}
```

Now you finished the first steps to creating your own module. The needed exports are set up, and now you are ready to create your own script runtime.

## Creating your own runtime

In the next article you will learn how to [create your own script runtime](creating-runtime.md).
