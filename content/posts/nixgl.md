---
title: "Nixgl"
date: 2024-09-02T19:14:53+02:00
tags: ["nix", "home-manager", "coding", "linux"]
draft: true
---

## Using OpenGL on non nix-OS system and the wrapper I wrote

Using graphical applications installed with nix on non nix-OS system requieres wslgl on wsl or nixGL on other systems.
It provides a lot of libraries and environment Variables and can be used as follows:

```
nixGL APPLICATION
```

Since I didnt want to include this prefix to all my programs installed through home-manager that might need it i wrote a wrapper.
The wrapper creates new binaries at install time that include the nixGL prefix which results
in a user wide openGL patch for these applications.
I even made it detect if the current system is nix-OS so it wouldnt patch if it is not needed.
The Code can be found in my home-manager repo under scripts @[Github](https://github.com/FrederikRichter/home-manager/).

### Breaking down how it works and how to implement it.

The script provides a lambda function that takes 2 arguments (pkgs and programs)
which can be included in your home.nix and turns packages into nixgl patched packages.
It takes in the pkgs argument first when we import it and takes packages as arguements later and returns patched
or normal packages depending on the os.

Relevant Code:
./scripts/nixgl.nix
```nix
pkgs:
program:
let
currentOS = builtins.currentSystem;
pname = program.meta.mainProgram;
wrapped = pkgs.writeShellScriptBin "${pname}" ''
exec nixGL ${program}/bin/${pname}
'';
in
# if not nixos patch with nixGL else return input
if builtins.match "nixos" currentOS == null then
	pkgs.symlinkJoin {
	  name = pname;
	  paths = [
	    wrapped
	    program
	  ];
	}
else
	program
```

./home.nix
```nix
{config, pkgs, inputs, ... }:
let
	mkgl = import ./scripts/mkgl.nix pkgs;
    ...
in
{
    home.packages = with pkgs; [ ... ]
    ++ map (mkgl) [kitty firefox ... ];
    ...
}
we use map to apply mkgl to a list of packages that we want to be patched.

If you have any issues or questions feel free to open them on my github.
