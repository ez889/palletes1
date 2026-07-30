# Pallet line demo (CODESYS / ST)
version 1.0
ST code is contain all the logic for this application.
LD scheme is contain some example of networks of this application.

Small learning project: an operator loads a pallet manually, it moves through rollers, passes 3 turntables, then stops at a print station.  
At the print station the pallet is held in position, I send a signal to the print cabinet to start printing, wait for the printed feedback bit, and then release the pallet.

This is intentionally simplified demo logic.

## How the cycle works:
1) m1 accept a new pallet and move it forward  
2) turntable #1 (m2_2, m2_1, e1) - pull pallet onto the table, rotate, release  
3) m3 transfer to the next step 
4) turntable #2 (m4_2, m4_1, e2) - pull, rotate, release  
5) m5 transfer to the next step
6) turntable #3 (m6_2, m6_1, e3) - pull, rotate, release  
7) m7 move to print position, stop, print, release
   
![photo_2026-01-25_18-35-00](https://github.com/user-attachments/assets/43dfc27a-516f-4d82-8361-f294742d0161)

   

Control is a simple step sequencer (steps 0,10,20,30,40,50,60,70,701).

### Outputs:
- m1 - motor for point 1 conveyor
- m2_2 - turntable #1 rollers motor
- m2_1 - turntable #1 rotate motor
- m3 - motor for point 3 conveyor
- m4_2 - turntable #2 rollers motor
- m4_1 - turntable #2 rotate motor
- m5 - motor for point 5 conveyor
- m6_2 - turntable #3 rollers motor
- m6_1 - turntable #3 rotate motor
- m7 - motor through the print station
- print - pulse command to the print cabinet (start print using some printing machine)

### Inputs:
- s1-s14 - pallet position sensors (see the scheme image)
- e1, e2, e3 - encoder positions for the turntables
- start, stop - start / stop
- printed - feedback from the print cabinet: printing finished

### Print handshake:
- Main PLC sends a short pulse on print to start printing.
- Print cabinet runs the print cycle internally.
- When printing is finished, the print cabinet sets printed=1.
- Main PLC detects printed and releases the pallet (uses timer7 for a short delay).

### Timers:
- timer2 delay after turntable #1 rotation (5s)
- timer4, timer4_1 delays around turntable #2 rotation (5s/5s)
- timer6_2, timer6_1 delays around turntable #3 rotation (5s/5s)
- timer7 delay to eject the pallet after printing (15s)

### What’s simplified(currently)
- no safety/interlocks/emergency chain
- no alarms/jam detection/timeouts
- no sensor debouncing

The project is contain:
ST code, picture of sheme, LD logic screenshots from CODESYS.
