I found it hard to get good data on 3D printed part strength, so here's some scratch notes and references.

## Broad Recommendations

### Cosmetic Parts

Things meant to look good but not stand up to mechanical stress beyond everyday human handling.

* Perimeters: 2 to 3
* Bottom Layers: 2 to 4
* Top Layers: 4 to 6
* Infill:
    * If the part can be printed without infill, don't use infill.
    * If the part has overhanging top layers which need infill, use as little as possible. Consider using Slic3r's Shape Modifiers or Cura's concentric infill or Simplify3D's multiple processes to reduce the amount of infill plastic used. You only need enough infill to support the top layers so they look nice.
* Layer Height: Low (0.1mm) looks better but takes a long time, high (0.3mm) prints faster but looks worse

### Mechanical Parts

Or more accurately, 3D printer and CNC machine parts. Pieces meant to hold frames and linear motion systems in place under the stress of torque applied through belts and motors.

* Perimeters: 4, though reduce to 3 or 2 if the part *really* doesn't print well with 4
* Bottom Layers: 8
* Top Layers: 8 to 10
* Infill: 50%. Maybe a printed CNC should have more like 75%?
* Layer Height: 0.25mm as most parts have dimensions in multiples of 0.5mm

## General Notes

* Rectangular/rectilinear infill varies the least under bending stress, I'd use this for stable mechanical parts.
* Hexagon/honeycomb infill is still fairly okay, it resists tension less and is faster to print.
* Rectilinear aligned to the force direction is strongest, 3D honeycomb is next best, rectilinear at 45 degrees to force direction is next weakest. Seems 3D honeycomb is the best all-rounder.
* The difference from low infill like 10% or 20% to high infill like 50% is drastic.
* There is diminishing returns from 50% infill to 75% infill.
* I don't think it's worth printing over 80% infill.
* Adding another shell is about as good as another ~15% infill, at least at low infill rates.
* Different blends of the same material vary greatly, so you can't make generalizations like "PETG is this strong" and "PLA is that strong". Some PLAs are stronger than some PETGs and vice versa.
* Different colors can matter too. White pigment changes flow properties. People say black filament is often blends of other failed batches with more pigment added in so is weaker because there's less plastic. "Natural" color with no pigment is the most pure plastic and also seems easiest to print with.
* Temperature matters, higher temperature probably results in layers adhering to each other better.
* Printing slower matters, again it probably results in temperature soak and layers adhering better.
* Orientation matters, the weak point is always the layer lines.
* Over extruding 10% or 20% will produce stronger parts, but affect visual quality and dimensional accuracy. I recommend [tuning flowrate](http://www.desiquintans.com/flowrate) then printing a box or other solid shape are your desired settings until you find the 1% iteration where the top is not overstuffed. That is the perfect flow ratio.
* Some people say larger layers are stronger, but there is more supporting data saying smaller layers are stronger. However, going from 0.25mm to 0.1mm usually increases print time by more than double, so this was a hard sell for me.
* To really be conclusive, you would need to build a test rig and put parts through specific mechanical tests of interest to you, like Tom Sanladerer and CNC Kitchen have done.

## References

* http://my3dmatter.com/influence-infill-layer-height-pattern/
* http://3dprintingforbeginners.com/infill-strength/
* https://www.3dhubs.com/knowledge-base/selecting-optimal-shell-and-infill-parameters-fdm-3d-printing
* [3D Printing Nerd: Stop Wasting Plastic on Infill Percentage](https://www.youtube.com/watch?v=fuGqsZjdPQM)
* [Maker's Muse: Multiple Infills in the same 3D Print using Simplify3D](https://www.youtube.com/watch?v=UTs7Y5VGNm8)
* [3D Printing Nerd: How To Use Multiple Infill Percentages When 3D Printing With Simplify3D](https://www.youtube.com/watch?v=9hw-6KTvZdA)
* https://www.typeamachines.com/3d-printing/infill-patterns-in-the-new-cura-part-6-concentric-infill-a-capping-surfaces-best-friend
* https://www.filaween.com/
* [Tom Sanladerer: Filaween Playlist](https://www.youtube.com/watch?v=sJER9QYnAcw&list=PLDJMid0lOOYl8TZJV9xHznKFq5yA5ZTi2)
* [CNC Kitchen: Strength Tests](https://www.youtube.com/watch?v=mIv507btE08&list=PLEOQTmIWJ_rncRcWmjQIvMKFAeM071CXM)
* http://makerstoolworks.com/all-filament-is-not-created-equal/
* http://3dprintboard.com/showthread.php?3549-Does-Layer-Height-effect-print-strength&p=95464&viewfull=1#post95464
* [CNC Kitchen: Infill and Shells](https://www.youtube.com/watch?v=AmEaNAwFSfI)
* https://engineerdog.com/2015/09/02/mechanical-testing-3d-printed-parts-results-and-recommendations/