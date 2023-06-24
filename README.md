Wake up babe, new Furby just dropped!


# Initial Teardown

This was definitely designed to snap together easily, but releasing the snaps is difficult.

For releasing the fur from the base, the best I found was to use a small metal rod to slip into the hemicircular holes to press the tabs.

![releasing the fur](images/releasing_the_fur.jpg)


## First Thoughts

Head and stomach touch sensors are capacitive touch.

RGB LEDs in ears and forehead are common + (anode).

There's a single motor to drive all movement (like the original 1998 Furby).

Main processor is black blob.

I see no wireless components, so the Furby-to-Furby communication is probably ultrasonic. That'll be fun to probe later.

![front pcb](images/board_front.jpg)

![back pcb](images/board_back.jpg)



# Flash Dump

On board is a sop-8 marked 25L3233F which appears to be a MX25L3233F 32Mbit Serial NOR Flash memory.

`flashrom` wasn't sure exactly which flash chip it, but all four models it suggested dumped the same. (`MX25L3205(A)`, `MX25L3205D/MX25L3208`, `MX25L3206E/MX25L3208E`, and `MX25L3273E`)

The dump is `furby2023_flashdump.bin`.

It's a 4MB file with a reasonable bit of empty space. There are a good bit of debug message strings in it. `binwalk` struggles to tell me much about it.

I've made some attempts to load the dump into Ghidra. Best I can figure is it's little-endian ARM and should be loaded at 0x0400_0000 with a RAM section at 0x2000_0000. Both I and Ghidra are definitely struggling since we don't know what chip this is.


# (Not) Debug Pins

The back PCB has `Debug`, `RESETn`, `MOSO`, `CLK`, and `IO_EN` pins that look really interesting. Poking `CLK` and the other pins with a SWD programmer doesn't yield anything, and a JTAGulator didn't identify any pair as SWD. I see no traffic on any of the pins. `RESETn` does reset the processor.

I have no clue what `MOSO` is other than a typo.


# Next Steps

I'd like to capture the ultrasonic communication between two Furbys.

I'd also like to decap the epoxy blob and hopefully see some processor markings, but that'll destroy the PCB and any chance of doing more dynamic analysis. At $74 each, I don't want to destroy too many if I can avoid it.
