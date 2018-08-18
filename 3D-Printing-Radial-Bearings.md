This page refers to "spherical roller" or "radial ball" bearings like the popular 608 skateboard bearing.

![Miniature series of ball bearings](http://i.imgur.com/8FsEalg.jpg)

This page is not about fidget spinners :trollface: 

## Axial Load

It's a good idea to take the weight of the bed off spring couplers and rest it on a bearing instead. People like to say these bearings are not designed for axial loads, and while that may be right, they still do have an axial load rating. As long as we don't exceed that load rating, we're fine to apply a load.

So how much can a 608 bearing hold along its axis?

A 608 has a thickness of 7mm, outer diameter of 22mm, inner diameter of 8mm. Every 608 I can find has a static load rating about 1350N and dynamic load rating about 3250N. One kilogram under earth gravity applies 9.8N of force.

SMB Bearings suggest a thin radial bearing (like a 608) can hold 10% to 30% of its static load rating as an axial load, so we get a range of:

    1350N * 0.10 = 135N
    135N / 9.8N = 13.8 kg
    
    1350N * 0.30 = 405N
    135N / 9.8N = 41.3 kg

SKF Bearings suggest a spherical roller bearing can carry an axial load of 0.3% of the bearing inner diameter times depth, so:

    7 * 8 * 0.003 = 0.168 kN = 168N
    168N / 9.8N = 17.1 kg

RBC Bearings suggest a small ball bearing should not axially carry more than a quarter of its static load rating, so:

    1350N * 0.25 = 337.5N
    337.5N / 9.8N = 34.4 kg

A square foot of quarter-inch MIC6 weighs 3.636 lb or 1.65kg.

Long story short, you could support your entire printer on a single 608 bearing and be within its axial load rating. Don't worry about it.

* http://www.smbbearings.com/technical/bearing-load-rating.html
* http://www.skf.com/group/products/bearings-units-housings/roller-bearings/spherical-roller-bearings/loads/index.html
* http://www.rbcbearings.com/ballbearings/axial.htm
* http://www.skf.com/group/products/bearings-units-housings/ball-bearings/deep-groove-ball-bearings/deep-groove-ball-bearings/index.html?designation=608-2Z
* http://www.nmbtc.com/bearings/part-numbers/search/608ZZ/4521
* http://www.sengpielaudio.com/calculator-forceunits.htm
* https://www.onlinemetals.com/merchant.cfm?pid=7808&step=4&showunits=inches&id=309&top_cat=0

## References

* https://www.facebook.com/groups/1711323699127948/permalink/1926397280953921/ - Discussion with Seth on D-Bot Group
* `TODO` - find the Japanese guy on the Tarantula group who taught me this and link to that post