# [neomura/game-connector](./readme.md)/notes

The connector links the edge of a [1.6mm thick](https://oshpark.com/shared_projects/KrFAoVRl) PCB containing a game (ROM, CPU, RAM, etc.) to a hosting console or arcade cabinet's power/video/audio/

There are [56 contacts (28 per side)](https://wiki.neogeodev.org/index.php?title=JAMMA_connector_pinout#JAMMA_pinout), mapping as follows, from left to right, looking from the component side:

|   | Reverse side |    | Component side |
| - | ------------ | -- | -------------- |
| A | GND          | 1  | GND            |
| B | GND          | 2  | GND            |
| C | +5V          | 3  | +5V            |
| D | +5V          | 4  | +5V            |
| E | -5V          | 5  | -5V            |
| F | +12V         | 6  | +12V           |
|   | (key)        | 7  | (key)          |
| J | COUNTER_2    | 8  | COUNTER_1      |
| K | LOCKOUT_2    | 9  | LOCKOUT_1      |
| L | SPEAKER-     | 10 | SPEAKER+       |
| M | RESERVED     | 11 | RESERVED       |
| N | VIDEO_GREEN  | 12 | VIDEO_RED      |
| P | VIDEO_SYNC   | 13 | VIDEO_BLUE     |
| R | SERVICE      | 14 | VIDEO_GND      |
| S | TILT         | 15 | TEST           |
| T | P2_CREDIT    | 16 | P1_CREDIT      |
| U | P2_START     | 17 | P1_START       |
| V | P2_UP        | 18 | P1_UP          |
| W | P2_DOWN      | 19 | P1_DOWN        |
| X | P2_LEFT      | 20 | P1_LEFT        |
| Y | P2_RIGHT     | 21 | P1_RIGHT       |
| Z | P2_A         | 22 | P1_A           |
| a | P2_B         | 23 | P1_B           |
| b | P2_C         | 24 | P1_C           |
| c | P2_D         | 25 | P1_D           |
| d | P2_E         | 26 | P1_E           |
| e | GND          | 27 | GND            |
| f | GND          | 28 | GND            |

## Power

All power contacts (+5V/-5V/+12V) are DC.  [Games may or may not use the various input voltages, and hosts may or may not supply them (or may only have limited current available on certain rails)](https://forums.arcade-museum.com/threads/the-mystery-of-5-volts.419936/).

## Video

The video contacts (VIDEO_RED/VIDEO_GREEN/VIDEO_BLUE/VIDEO_SYNC) generally output a simple RGB video signal.  Unfortunately, there is no strict standard regarding voltages or protocol; voltages may be as [high as +5V](http://forum.arcadecontrols.com/index.php?topic=31120.0).

The following measurements were taken from a simple ["60-in-1" game PCB](https://www.ebay.co.uk/itm/234199084333?epid=0&hash=item36875a6d2d:g:Y1EAAOSwZzlhSuSM):

- All voltage ranges were 0 to +2.5V.
- Each frame lasted 16641500000ps.
- Each line lasted 63500000ps.
- A horizontal synchronization pulse was low and lasted 5125000ps.
- A vertical synchronization pulse was low and lasted 259208333ps.
- The first 10 lines following the vertical synchronization pulse are blank.
- The last 10 lines before the vertical synchronization pulse are blank.

Therefore:

- Each frame had 262 lines.
- The vertical synchronization pulse covered 4 lines.
- 238 lines were NOT blank.
- The refresh rate was 60Hz.

## Audio

A 8Ω speaker is to be connected across the SPEAKER- and SPEAKER+ contacts.  The game is responsible for power amplification.  Most game PCBs can drive [up to 8W](https://forums.arcade-museum.com/threads/maximum-wattage-on-a-jamma-cab.311089/).

## Inputs

The following contacts are [connected to ground through normally-open switches](https://www.thegeekpub.com/279928/jamma-pinout-pdf#JAMMA_FUNCTIONAL_DIAGRAM).

### Joysticks

P1_UP, P1_DOWN, P1_LEFT, P1_RIGHT, P2_UP, P2_DOWN, P2_LEFT and P2_RIGHT are connected to 8-way joysticks for up to two players.

### Buttons

P1_A, P1_B, P1_C, P1_D, P1_E, P2_A, P2_B, P2_C, P2_D and P2_E are connected to action buttons for fire, jump, etc.  The precise number provided by the host varies [(usually 2-3)](http://forum.arcadecontrols.com/index.php?topic=139537.0).

P1_START and P2_START should start the game for that player if one is not running and sufficient credits have been inserted.

### Credits

P1_CREDIT and P2_CREDIT are "pressed" when a coin is inserted.  They should activate the corresponding COUNTER_1 or COUNTER_2 contact and add to that player's credits.

SERVICE, on the other hand, generally gives player(s) credits [without activating COUNTER_1 or COUNTER_2](https://wiki.arcadeotaku.com/w/JAMMA).

### Tilt

The tilt input is activated when [the machine is physically stuck](https://allpinouts.org/pinouts/connectors/buses/jamma-japanese-arcade-machine-manufacturers-association/).  Most game PCBs do not use it.

### Test

The test input [may be a momentary or latching switch](https://forums.arcade-museum.com/threads/jamma-test-service-switch-question.73066/).  When activated, the game should show some form of service menu.

## Miscellaneous

## Coin lockouts

LOCKOUT_1 and LOCKOUT_2, on some games/hosts, [physically prevent entered coins from being accepted](https://wiki.neogeodev.org/index.php?title=Coin_lockout) when the game PCB does NOT open a path to ground.

### Coin counters

COUNTER_1 and COUNTER_2 increment a physical counter when the game PCB [https://forums.arcade-museum.com/threads/how-do-i-hook-up-the-coin-counter-on-a-jamma-cab.65327/](opens a path to ground).
