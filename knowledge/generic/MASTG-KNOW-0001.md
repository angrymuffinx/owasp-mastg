---
title: Basic Tampering Techniques
platform: generic
---

## Binary Patching

_Patching_ is the process of changing the compiled app, e.g., changing code in binary executables, modifying Java bytecode, or tampering with resources. This process is known as _modding_ in the mobile game hacking scene. Patches can be applied in many ways, including editing binary files in a hex editor and decompiling, editing, and re-assembling an app. We'll give detailed examples of useful patches in later chapters.

Keep in mind that modern mobile operating systems strictly enforce code signing, so running modified apps is not as straightforward as it used to be in desktop environments. Security experts had a much easier life in the 90s! Fortunately, patching is not very difficult if you work on your own device. You simply have to re-sign the app or disable the default code signature verification facilities to run modified code.

## Code Injection

Code injection is a very powerful technique that allows you to explore and modify processes at runtime. Injection can be implemented in various ways, but you'll get by without knowing all the details thanks to freely available, well-documented tools that automate the process. These tools give you direct access to process memory and important structures such as live objects instantiated by the app. They come with many utility functions that are useful for resolving loaded libraries, hooking methods and native functions, and more. Process memory tampering is more difficult to detect than file patching, so it is the preferred method in most cases.

Substrate, [Frida](0x08a-Testing-Tools.md#frida), and [Xposed](0x08a-Testing-Tools.md#xposed) are the most widely used hooking and code injection frameworks in the mobile industry. The three frameworks differ in design philosophy and implementation details: Substrate and Xposed focus on code injection and/or hooking, while Frida aims to be a full-blown "dynamic instrumentation framework", incorporating code injection, language bindings, and an injectable JavaScript VM and console.

However, you can also instrument apps with Substrate by using it to inject [Cycript](0x08a-Testing-Tools.md#cycript), the programming environment (aka "Cycript-to-JavaScript" compiler) authored by Saurik of Cydia fame. To complicate things even more, Frida's authors also created a fork of Cycript called ["frida-cycript"](https://github.com/nowsecure/frida-cycript "Cycript fork powered by Frida"). It replaces Cycript's runtime with a Frida-based runtime called Mjølner. This enables Cycript to run on all the platforms and architectures maintained by frida-core (if you are confused at this point, don't worry). The release of frida-cycript was accompanied by a blog post by Frida's developer Ole titled "Cycript on Steroids", a title that [Saurik wasn't very fond of](https://www.reddit.com/r/ReverseEngineering/comments/50uweq/cycript_on_steroids_pumping_up_portability_and/ "Cycript on steroids: Pumping up portability and performance with Frida").

We'll include examples of all three frameworks. We recommend starting with Frida because it is the most versatile of the three (for this reason, we'll also include more Frida details and examples). Notably, Frida can inject a JavaScript VM into a process on both Android and iOS, while Cycript injection with Substrate only works on iOS. Ultimately, however, you can of course achieve many of the same goals with either framework.
