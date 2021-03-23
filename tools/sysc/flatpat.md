flatpat
consider if flatpat can be used to replace conan.

it is easier to track the s/w lifecycle and release compiled the conan package system.
flatpat need a runtime base
see. flatpat.org pack a bundle of program together into a single flatpat app should be possible, with software inter-dependence being resolved.

flatpak run --command=sh --devel <application-id> This creates a sandbox for the application with the given ID and, instead of running the application, runs a shell inside the sandbox. From the shell prompt, it is then possible to run the application. This can also be done using any debugging tools that you want to use. For example, to run the application with gdb:

$ gdb /app/bin/ This works because the --devel option tells Flatpak to use the SDK as the runtime, which includes debugging tools like gdb. The --devel option also adjusts the sandbox setup to enable debugging.

It is also possible to get a shell inside an application sandbox without having to install it. This is done using flatpak-builder’s --run option:

$ flatpak-builder --run sh This sets up a sandbox that is populated with the build results found in the build directory, and runs a shell inside it.
