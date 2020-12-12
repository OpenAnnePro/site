---
title: "How to Install"
permalink: /install/
layout: page
nav_order: 2
---

# Install Instructions
** WARNING: these instructions are by no means complete, feel free to ask in the
[Discord Server](https://discord.gg/ygssH9x) if you encounter any problems.
As we are still in early test stages.**

# Alternative Docker Build Script
Checkout the docker build script provided by `@zinosat`.

[Dockerfile]({% link install_docker.md %}){: .btn }

# Get AnnePro2 Tools

0. Install the latest stable `rust` toolchain using [rustup](https://rustup.rs/)
0. Also install [Visual Studio Community edition](https://visualstudio.microsoft.com/downloads/) 
including the C/C++ module to prevent errors while compiling
0. Download or Clone the [AnnePro2-Tools](https://github.com/OpenAnnePro/AnnePro2-Tools) project.
0. Compile the tool using
```bash
cargo build --release
```
0. The compiled tool should be in `./target/release/annepro2_tools` (In later I will refer to this as `annepro2_tools`)


# Get QMK firmware
*Hint: If you are on windows, I recommend completing this step using WSL. YMMV*
0. Clone our fork of the QMK firmware by using the command below. (Install Git if needed)
```bash
git clone https://github.com/OpenAnnePro/qmk_firmware.git annepro-qmk --recursive --depth 1
```
0. Obtain the `gcc-arm-none-eabi` toolchain so you can build the project.
0. To compile the firmware type
```bash
# For C15 Revision
make annepro2/c15
# For C18 Revision
make annepro2/c18
```
This should complete without any error. And you should be able to see a file named
`annepro2_c15(18)_default.bin` in your directory.

# Flashing the firmware
0. Put the keyboard into DFU/IAP mode.
0. Run annepro2_tools with the firmware you just built.
**Please substitute with correct path**
```bash
annepro2_tools annepro2_c15_default.bin
```
If you have the C18 revision, you must specify the interface number.
```bash
annepro2_tools annepro2_c18_default.bin -i=[[interface_number]]
```
Replace [[interface number]] with the keyboard's interface number. 

If the tool reports can't find device please double check you have the keyboard in IAP mode.

Interface number can be found by running the tool without the -i flag:
```bash
annepro2_tools annepro2_c18_default.bin
```
The tool lists the usb devices with their information. Search for the device with the `0x04d9:8009` vid pid pair:
`HID Dev: 04d9:8009 if: [[interface_number]] Some("USB-HID IAP")`.
This is the keyboard, interface number can be found here.

If the tool doesn't list the keyboard please double check you have the keyboard in IAP mode.

# Anne Pro 2 Shine

Building the shine firmware is very simailar to the QMK firmware.

0. Checkout the repository using
```bash
git clone https://github.com/OpenAnnePro/annepro2-shine.git --recursive
```

0. Build using
```bash
# for C15
make C15
# for C18
make C18
# for both
make
```

0. If built without error you can find the binary in `build/` directory.
You will flash the .bin file using annepro2 tools
```bash
# for C15
annepro2_tools -t led build/annepro2-shine-C15.bin

# for C18
annepro2_tools -t led -i=[[interface_number]] build/annepro2-shine-C18.bin
```
Enjoy!
