## Patching binary in Linux

### Using xxd

* Binary to Hex
xxd prog1.bin > b1.hex
xxd prog1.bin > b2.hex

* Hex to Binary
xxd -r -p b1.hex new_prog1.bin


diff b1.hex b2.hex
  or
vimdiff b1.hex b2.hex


### Using bsdiff
apt-get install bsdiff

bsdiff old_prog.bin new_prog.bin patch.bin
bspatch old_prog.bin output.bin patch.bin