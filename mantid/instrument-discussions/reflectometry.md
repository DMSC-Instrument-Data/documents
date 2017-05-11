# Reflectometry

Meeting 2017-05-11
- Hanna Wacklin
- Simon Heybrock


- Coherent summing (along constant Q instead of theta), ILL Robert Cubitt
  - More compute intense
  
- Vary 1D binning during live reduction

- Alternate between samples, must sort events into separate workspaces -> live listener must support this

- Stroboscopic measurements -- very fast processing -> event filtering

- detectors are 2D (has depth, but TOF correction done internally in detector?)

- wavelength-frame multiplication -> better resolution
  - no overlap in TOF -> overlap in lambda (convert separately then merge)

Monitors:
- long experiment: time normalized from detector
  - 4cm x mm -> hits strip of pixels 1e8 n/s/cm^2
  - simple normalization
- per-pulse normalization (event mode, or time resolved histogram)
  - 

## Future

- penetration depth - > wavelength