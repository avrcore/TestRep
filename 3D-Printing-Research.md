# Research

Useful things other people have found out, and references to them.

## 8-Bit Speed Wall

Tech2C found out the fastest an 8-bit Arduino setup (RAMPS) can print over serial is pretty slow, and over sdcard on a CoreXY the fastest it can plot curves is about 75mm/sec:

* https://www.youtube.com/watch?v=ySqj3gPqfrs - Part 1 (Serial)
* https://www.youtube.com/watch?v=SBZ7MfvAsGc - Part 2 (Curves)

CoreXY requires more complex computations than cartesian so these limits don't apply to Mendel or cantilever designs. They're probably fairly applicable for Ultimaker-style gantries tho.

## Aluminium Build Plates

Use cast, not rolled. Trade name for precision cast plates is MIC6:

* https://www.reddit.com/r/3Dprinting/comments/4bo4q5/stop_buying_aluminum_build_plates_buy_raw_cast/

## corexy

* http://www.corexy.com/
* http://www.doublejumpelectric.com/projects/core_xy/2014-07-15-core_xy/
* https://www.youtube.com/watch?v=8WLZ8OesMF4

## E3D V6 Extrusion Speed Limit

Bryce Standley found out the fastest an E3D V6 can reliably extrude is about 150mm/sec, faster than that and there is poor layer adhesion due to the filament not spending enough time in the melt zone and not getting hot enough:

* https://www.youtube.com/watch?v=LdosTxvONI0

A Volcano-style hotend should be able to print faster.

## Fan Ducts

ruiraptor showed that [Lion4H's Fang Duct](http://www.thingiverse.com/thing:2175956) is really good:

* https://www.youtube.com/watch?v=c0llj3pBltA

## Infill and Supports

3D Printing Nerd suggests most infill is a waste of time and plastic:

* https://www.youtube.com/watch?v=fuGqsZjdPQM

Which is all well and good as long as your design doesn't overhang too much or try to print in mid-air.

Angus from Maker's Muse shows printing with multiple infill processes in the same model using S3D could be a good compromise, or just use S3D manual supports properly:

* https://www.youtube.com/watch?v=oR-cToT8v-c - processes
* https://www.youtube.com/watch?v=piwKAOOaPKc - supports

## Printed smoothrod Bearings

Russ from RWG Research showed that printed PLA bearings work best with silicone spray lubricant, and lasted pretty well even after a year of heavy printing:

* https://www.youtube.com/watch?v=MV1gH_RPmRk

List of models and discussion on r/Reprap:

* https://www.reddit.com/r/Reprap/comments/1g5a18/is_anybody_using_printed_lm8uu_style_bearings/

Some people make a carrier which holds PTFE (Teflon) tubes for the bearing surface:

* http://www.thingiverse.com/thing:1039173
* http://www.thingiverse.com/thing:1739340

## Z Probe Accuracy and Repeatability

Both Tom and Tech2C have done research and reviews on these:

* Tom shows 6-36V sensors work on 5V: https://www.youtube.com/watch?v=Bov87qlX0Gs
* Tom compares 12 sensors: https://www.youtube.com/watch?v=il9bNWn66BY
* Tech2C explores PNP and NPN sensors: https://www.youtube.com/watch?v=wih4fNkKUCc