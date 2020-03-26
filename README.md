# Ambroxol

Funny MBR experiments


## Builder

Use `./builder` to transform an opcode file with comments like this:

```sh
# Simple example

45 46 49 20 50 41 52 54  # First line
00 00 01 00              # Second line

# Comment
00 00 01 00              # Third line
27 6D 9F C9              # Yet another line
```

into plain binary file:

```sh
$ ./build example.mbr sector.bin
$ xxd sector.bin                
00000000: 4546 4920 5041 5254 0000 0100 0000 0100  EFI PART........
00000010: 276d 9fc9                                'm..
```


## Loaders

### A simple string printer in the `printer.mbr`

`printer.mbr` is a simple example of the MBR code execution.

File contains:

- A fixed string at `0x0080` that will be print
- A binary code (x86 opcodes) with a loop extracting characters
  from data block and printing it using `INT 10h` BIOS function.


### A not very simple printer in the `color-printer.mbr`

Improves:

- Changes a video mode to use colored output
- String printing in the infinite loop
- (New!) String changing a color!


### A useless typewriter in the `typewriter.mbr`

`typewriter.mbr` contains an example of the keyboard input processing.

It can:

- Print pressed characters in a loop.
- Process a `Return`/`Enter` key to create a new line.
- Process a `Backspace` key to return to previous character 
  (except new line :)) and replace it with another one.

  

## Usage
### How to build

```sh
$ ./build printer.mbr printer.img && stat -f %z printer.img
512
```

`512` -- is a valid MBR size. Otherwise you've got a corrupted file :(


### How to test

You need to use QEMU or any other emulator or VM able to use 
`.img` or `.bin` disk images.

```sh
$ qemu-system-i386 -nic none printer.img
```

> `-nic none` option disables network interfaces


### How to use on hardware

Just write a USB-drive using `dd`:

```sh
$ dd if=printer.mbr of=/dev/sdb
```

> Of course you need to find a valid path of your USB-drive.

> Of course you have not to use partition path (e. g. `/dev/sdb1`) instead of devices path.

Then plug a drive into your x86-compatible device and boot it using USB.

