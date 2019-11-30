# The ChessBox

Sections:

1. Project outline
2. Parts list
3. Psuedocode
4. Useful links

## Project Outline

This repository (and by extension this document) exists to help organize and gather my thoughts on my newest hobby electronics/coding project, the ChessBox.

The ChessBox is a small, lockable box meant for storing small amounts of documents or personal valuables, primarily in the form of security through obfuscation.  The basic concept is to use an arduino, a series of magnetically activated leaf switches, a chess board and pieces, and a linear solenoid to provide a means of access control to a small box that would otherwise be assumed to be an unremarkable piece of furniture. This was partially inspired by adafruit's lorwan chess project, but with being built with different activation and code mechanics.

## Parts list

-4 Magnetic triggering reed switches (sold in 5's commonly, so one spare)
https://www.ebay.com/itm/5x-Aleph-Small-Magnetic-Reed-Switch-SPST-NO-1-Amp-110VAC-200VDC-57mm-Long/122946259424?ssPageName=STRK%3AMEBIDX%3AIT&_trksid=p2057872.m2749.l2649

-Electronic Solenoid Latch (more expensive if US shipped)
https://www.ebay.com/itm/12V-Electronic-Latch-Lock-Catch-Door-Gate-Electric-Release-Assembly-Solenoid-a3/303109420112?epid=6031018772&hash=item4692bab450:g:5MoAAOSwgs5cmxda

-Neodymiun Magnets

-Chess pieces (weighted)

-Materials to build storage box (wood for sides, maybe brass for corners, hinges, etc.)

## Psuedocode

**Overview:**
This project will require moving 4 specific chess pieces to particular spaces, in a specific order. It is my belief this offers a sufficient complexity of moves to add security. The math is approximately 20x20x48x48 for roughly 920,000 combinations translating to more than an order of magnitude above a standard combination lock. If the correct sequence is made, the arduino will send a signal to the solenoid, thereby opening the box. The following is the psuedocode to make that happen.

**Variables:**
`Move1` `Move2` `Move3` `Move4` `Unlock`
Timer (to give user time to open the door once moves have been completed)

**Code:**

If Input1 on High (meaning circut complete) set `Move1` to True
	If Input1 goes Low, flip `Move1` back to False
If Input2 on High and `Move1` equals True, set `Move2` to True
	If Input1 goes Low, flip `Move2` back to False
If Input3 on High and `Move1/Move2` equal True, set `Move3` to True
	If Input1 goes Low, flip `Move3` back to False
If Input4 on High and `Move1/Move2/Move3` equal True, set `Move4` to True

If `Move1/Move2/Move3/Move4` equal to True, set `Unlock` to True
	Signal the solenoid to unlock

Start check on inputs for low signals (meaning pieces in original place), once every five seconds
	If safe has been open 75 plus seconds, remind user with beep or LED flash
  
If all 4 Inputs register low, set `Unlock` to False
	Make lock warning beep/LED flash
	Send signal to solenoid to lock
  
Wait 5 seconds, begin input checks again
