# DOPE Ballistic TI-85

This program is a DOPE (Data on Previous Engagements) chart recorder, shot calculator, and range estimator for the Texas Instruments TI-85 graphing calculator. Recording sample shots at known ranges with particular rifle and ammunition combinations allows DOPE to estimate the drop and drift of new shots. Accuracy increases with each shot recorded.

## Running DOPE

Transfer all programs in this project to your TI-85 and run the following initialization instructions to prepare your memory:

```
0→ID
DClear
```
These instructions set up all variables and matrices necessary for the program to function.

Once memory is initialized, on all subsequent uses of the calculator simply run `DOPE`. All other programs are subroutines called by DOPE. You'll need to chose the AMMO menu and set up an ammunition type before recording or solving shots.

## Using DOPE

DOPE's main menu lets you choose from a shot solver, a shot recorder, and a range estimator. The shot solver and recorder are based on your currently selected ammunition type.

### SOLVE

The shot solver asks you to estimate the range and wind conditions currently, then runs a cubic spline interpolation on your recorded data for the currently selected ammunition type. It will report how many mils of elevation and windage adjustment will be necessary to hold over or adjust your turrets.

### RECRD

The shot recorder asks you to record your current range and conditions, then fire with your zeroed rifle and zeroed turrets at the center of a target. You then record the drop and drift, which are integrated into the dataset for your selected ammunition type. The more shots you record, the more accurately the TI-85 can model your rifle and ammunition's behavior.

### RANGE

The range estimator asks you to provide a known target size, then check the target's size in your scope and report it in mils. An estimated range in yards is returned.

### AMMO

Manage your ammunition types. 12 slots are provided. Each should be a unique firing setup, so if you're using a single rifle with the TI-85 then slots can represent one ammunition type/lot, or if you're using multiple rifles then each can represent one rifle/ammunition combination. Slots can be loaded, cleared, renamed, and moved up and down in a paginated list.

## Memory limitations

The TI-85 only has 28kb of memory, so the data recording and shot calculations take some liberties with reality. Each ammunition type is recorded in a matrix with 5 values per row: Range, Elevation offset, Windage offset, Elevation weight, and Windage weight.

To preserve memory, shots are not recorded individually, but instead folded into existing data for the range you're shooting at with each ammunition type. For example, if you're shooting ammo #3 at 100yds, and you've already shot it at 100yds before, new shots' elevation and windage offsets will be merged into ammo #3's row of data for the 100 yard range according to its weights. The more shots, the less weight new shots have against existing data. Since there is not always crosswind, the weights of elevation and windage data are managed independently.

Additionally, only 12 slots are available for different ammunition types. Significantly increasing that number would decrease the available memory to record more shots. If you shoot more than 12 rifle and ammunition combinations, just buy another TI-85. They're practically free.

## FAQ

### How does this program calculate shots at new ranges and wind conditions?

The shots you record become points in two cubic splines specific to each ammo type you use: one for drop and one for drift. New shots' conditions are interpolated on these curves based on the range and wind conditions you report.

Crosswind is assumed to have a linear effect on bullet drift in the calculations, and all of your recorded shots with a crosswind component are normalized to 10mph. If you report that you're shooting in a 2mph crosswind, a fifth of the estimated drift is reported.

If you record more shots at the same range, that anchor point of the curve becomes more true to reality. If you record more shots at different ranges, the overall curve becomes more accurate to how your bullet actually arcs.

### Is there an option to switch to MOA?

No, but "mils" is merely what's in the text strings. The solver doesn't care what unit of measure you're actually using--it has no idea what a mil is. You can replace all references to "mil" with "MOA", or just enter values in MOA whenever the programs ask for mils. So long as you're not mixing units, the units you put in are the units you get out.

The one exception to this is the range estimator. The subroutine DRange has a mil-specific formula baked in that will need to be swapped for the corresponding MOA range estimation formula if you're looking through a scope marked for MOA.

### What factors does this program not take into account?

Head/tailwind, shot angle, altitude, temperature, barometric pressure, and humidity.

The only environmental conditions recorded are the wind heading and speed, and it's going to decompose those into a head/tailwind and crosswind component and only keep the crosswind. In reality, the headwind or tailwind will affect the elevation adjustment and the windage adjustment to a degree.

Along with the other atmospheric conditions that are ignored, these effects become noise in the recorded data that increases in severity at greater ranges.

Shot angle will also affect elevation adjustment; however, it is not considered here. All shots are assumed to be level with the target.

If you're a professional sniper, the data recording and shot calculations are likely not advanced enough for your use. You're probably also not using a TI-85 in the first place.

### What factors does it surprisingly take into account?

A lot of data required by ballistic calculator apps is simply unnecessary here because it gets modeled with real-world feedback into your dataset more accurately than any information from the ammo manufacturer could enable. This program does not need to know things like your ammunition's weight, ballistic coefficient, velocity, or anything about how your rifle and sights are configured.

### Is an anemometer recommended?

Yes, for the most accurate results you should use an anemometer at least on your recorded shots. But let's be honest, it would be ridiculous to pair a $500 Kestrel with a $2 TI-85. Just get a $20 anemometer off AliExpress or something. This program should not be fundamentally accurate enough to warrant a precision meter.

### Will this work on a TI-83? A TI-89? A 92?

No. It may work on an 86, but I can't say for sure. The TI-85 is the weird one out of the 8x lineup, and a lot of its TI-BASIC is either limited or unique to it. It wouldn't be hard to rewrite this for an 89 or 92, though, mostly with some find and replace. The 83/84 should also be capable, though some structural changes and significant variable renaming would be required. I imagine it will never be ported to a different calculator, but feel free to prove me wrong with a merge request for equal measures of judgment and respect.

### How do I get this on my TI-85?

In 2026? You probably don't, frankly. If you have an old GraphLink cable and software and a computer from the turn of the century then sure, there's a way. I don't remember what it is, but it's theoretically possible. Failing that, you type it by hand into your TI-85 like you mean it.