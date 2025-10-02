# Terrible VGA #

Inspired by Ben Eater's discrete logic VGA and Leoneq's "bad vga" design,
this project implements a very basic, text only VGA display using just 1 
bit of a 128KB EEPROM data bus and 2KB of SRAM.

![Render of PCB](./badVga.png)

## Predictable issues ##

The logic chips used are clocked to near their limit (notably the 12-bit
counters) and struggle to meet the timing requirements of the design. 
Careful selection of components should help to resolve this.

The original designs by Ben and Leoneq were built on breadboard with no
practical ground plane and very little decoupling. This design is a 
dedicated 4-layer PCB with two solid ground planes that should help to 
resolve the problem. As much as possible, all signals are routed on a 
single layer with the "spare" layer used for power delivery.

Timing is not just an issue for the board's clock it is also a problem
for the host. The board clock is decoupled from the host which will,
inevitably, result in read and write conflicts. The host is expected to
be running at 3.147MHz while the video board is running at 20MHz. It
should be viable to gate host r/w operations such that they are blocked
(in the semaphore sense) by the video. If not the only option is to 
restrict write operations to the vertical blank period. The alternative
is to switch to a dual-port arrangement and bypass the problem completely.

## Integration ##
### Inputs ###

Integration with the host requires address bus, data bus, enable/select
and R/!W.

### Outputs ###

Outputs from the board, by default, are just the data bus but this needs
to be extended to include horizontal and vertical sync (TODO)

## Next ##

* Investigate dual-port ram option
* Investigate switch to 25.175MHz for better VGA compliance (can be
driven entirely from the host clock)
* Use latch and 3 bit load counter to reduce contention on databus
* Combine counter with 8 bit shift register (to reduce eeprom size
**and** allow eeprom programming on TL866 without adapters)
* Investigate combining with eeprom based state machine to manage
sync and blank timing

## References ##

Ben Eater VGA <https://eater.net/vga>

Leoneq <https://github.com/Leoneq/iNapGPU>

Dr Matt Regan VGA 
<https://github.com/Turing6502/ZX-Spectrum-No-ULA/blob/main/ZX%20Spectrum%20No%20ULA.pdf>

## License ##

This project is licensed under CC BY-SA 4.0
<https://creativecommons.org/licenses/by-sa/4.0/>

