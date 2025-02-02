# Crosstool-NG Installation and Usage guide
## Installation Steps

### 1. Clone the Repository
Clone the Crosstool-NG repository from GitHub:
```bash
git clone https://github.com/crosstool-ng/crosstool-ng
```
**What this does**: Downloads the Crosstool-NG source code to your local machine.

### 2. Checkout to Tag `1.26.0` "Latest Version"
Switch to the specified version tag:
```bash
git checkout crosstool-ng-1.26.0
```
**What this does**: Ensures you are working with the stable version `1.26.0` of Crosstool-NG.

### 3. Bootstrap the Build System
Prepare the build system:
```bash
./bootstrap
```
**What this does**: Initializes the build environment for Crosstool-NG.

### 4. Configure the Build
Run the configuration script with local installation enabled:
```bash
./configure --enable-local
```
**What this does**: Configures Crosstool-NG for a local build. Ensure any required dependencies are installed 
it will give you some errors to install the dependencies required
```bash
sudo apt install <dependency>
```
_Example_: `sudo apt install zlib`

### 5. Build Crosstool-NG
Compile the Crosstool-NG source code:
```bash
make
```
**What this does**: Compiles the Crosstool-NG executable file. After this step, you will have the `ct-ng` executable ready to use.

For additional information, refer to the [Crosstool-NG Documentation](https://crosstool-ng.github.io/docs/).

---

## Using Crosstool-NG

### 1. List Available Samples
Display the list of pre-configured samples:
```bash
./ct-ng list-samples > samples.txt
```
**What this does**: Filters the available samples for the architecture of your choice.

### 2. Select a Sample
Choose a sample for your desired architecture:
```bash
./ct-ng arm-cortexa9_neon-linux-gnueabihf
```
**What this does**: Selects the toolchain configuration for the specified architecture.

### 3. Open the Menu Configuration
Customize your toolchain configuration:
```bash
./ct-ng menuconfig
```
**What this does**: Opens an interactive menu to edit the selected toolchain configuration.

### 4. Edit Your Toolchain
Make any necessary changes to the configuration.
**What this does**: Allows you to tailor the toolchain to your specific needs.

### 5. Build the Toolchain
Generate the toolchain:
```bash
./ct-ng build
```
**What this does**: Builds the cross-compilation toolchain based on your configuration.

---

## Example: Cross-Compiling a Simple C Program

I used `arm` as an architecture and wrote a simple C program that adds two numbers and prints hello to the user. This program was compiled with `arm-gcc` as part of the cross-compilation process.

### After the Build
After building the toolchain, you will have a directory called `x-tools`. Update your `PATH` to include the toolchain binaries:
```bash
export PATH=/home/omar/x-tools/arm-omar_mohamed-linux-musleabihf/bin:$PATH
```
**What this does**: Allows you to use `arm-gcc` from anywhere.

### Running the Program
If you try to run the program directly on your x86 machine:
```bash
./a.out
```
**Problem**: The executable `a.out` is built for an ARM architecture and cannot run on an x86 machine.

### Running the Program with QEMU
To test the program without an ARM physical machine, use QEMU. First, install QEMU:
```bash
sudo apt install qemu-user
```
Attempting to run the program directly with QEMU:
```bash
qemu-arm ./a.out
```
**Error**: `Could not open '/lib/ld-musl-armhf.so.1': No such file or directory`

### Solution: Specify the Sysroot
The linker searches in the root file system of the Linux, but the `sysroot` file system is the one which contain the required libraries for arm. 
Run the program using:
```bash
qemu-arm -L /home/omar/x-tools/arm-omar_mohamed-linux-musleabihf/arm-omar_mohamed-linux-musleabihf/sysroot ./a.out
```
**What this does**: The `-L` option tells QEMU where to look for shared libraries during runtime.
