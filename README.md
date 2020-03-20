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

### `printer.mbr`

`printer.mbr` is a simple example of the MBR code execution.

File contains:

- A fixed string at `0x0080` that will be print
- A binary code (x86 opcodes) with a loop extracting characters
  from data block and printing it using `INT 10h` BIOS function.
  
