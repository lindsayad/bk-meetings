#How should we couple our physics?

###Questions

- Existing ACELIB data in Serpent repository is spaced at 300K intervals
- There is also a Doppler broadening routine in Serpent
    - Are these two things enough to generate a reliable interpolation table of
      group constants with respect to temperature?
    - Or do we need to use NJOY to reduce the 300K step size?
- How do we want to homogenize cross sections over the reactor?
    - Homogenize over the entire domain? (1 set of contants; probably shouldn't
      even be included in this list)
    - Homogenize over material? (2 sets of constants; what I'm currently doing)
    - Homogenize per pin? (23x23 sets of constants)
    - Homogenize per pin per material? (2x23x23 sets of constants)
- My initial instinct would be to homogenize per material. Then in Serpent we
  can easily:
    - Sweep somewhat independently through temperature in moderator and fuel and
      construct a 2D lookup table for group constants as a function of fuel
      temperature and moderator temperature
    - As I'm writing this, this seems really quite silly since moderator or fuel
      at the edge of the reactor will have probably quite a bit different
      temperature than moderator or fuel in the center of the reactor
- Very complicated coupling option: iterate back and forth between Moltres and
  Serpent, mapping temperature profile from Moltres to materials in
  Serpent. This would require a lot of material definitions. Just in the
  xy-plane something like 23x23/8 (using symmetry) and then some kind of
  discretization in the z direction.
