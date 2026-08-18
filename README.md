# geodish

**an interactive, parametric geodesic parabolic dish designer**

geodish generates the physical construction geometry for a self-supporting parabolic reflector built from triangulated ribs — in the spirit of Buckminster Fuller's geodesic domes, rather than a conventional radial-rib mesh dish. Give it a stock material length and an f/D ratio, and it derives the full cut list (rib lengths, hole spacings, aperture, focal length) along with a live, mouse-orbitable 3D view of the resulting shape.

It is wavelength-agnostic by design: useful for radio astronomy, amateur radio / satellite dish feeds, general RF work, or if you are a sadist and/or seek to make a solar oven... optical reflector projects.

## try it

Open [`geodish.html`](https://zenmetsu.github.io/geodish/geodish.html) directly in any modern browser.

### controls

- **drag** to orbit the camera, **scroll** to zoom (reversible via the on-screen toggles)
- Edit any parameter directly, or use the `<` / `>` steppers beside each value
- **hover** a rib in the 3D view to highlight that entire physical piece in the element planner at the bottom
- **optimize** auto-solves the inner-ring position that equalizes the rib-B leg and ring lengths, holding f/D and stock length fixed
- the results panel (upper right) shows derived aperture, focal length, wavelength/beam angle at your chosen frequency, and the feed illumination angle

## background

This design descends from Yoshiyuki Takeyasu's (JA6XKQ) "Geodesic Parabola Antenna" — a stressed-skin dish built from three families of ribs (radial, geodesic-diagonal, and circumferential) that self-forms into a paraboloid once bolted together, rather than needing a mold or rolling mill. geodish generalizes that specific fixed-size design into a fully parametric tool: any diameter, any f/D ratio, and differing spoke counts.  for spoke counts much removed from the original 6 spoke design, care must be taken with the medium length (B) elements since excessive curvature could preclude construction.  usually the "optimize" button will help with this, but experience shows that 4, 6, or 8 spoke designs work out the best

## repo layout

```
.
├── README.md       this file
├── geodish.html    the tool itself — open this in a browser
└── geodish.hs      Haskell source that generates geodish.html
```

`geodish.html` is generated output, not hand-edited directly — see below if you want to modify the tool itself.

## regenerating geodish.html

requires GHC (the Glasgow Haskell Compiler) and nothing else; the generator only uses Haskell's `base` library.

```sh
ghc -O1 geodish.hs -o geodish
./geodish
```

this writes `geodish.html`

## how it works, briefly

- each rib's length is computed from the true **arc length** along the parabola's meridian (z = r²/4f), not the straight-line chord, since physical stock bends into that shape without stretching.
- the rib-B and rib-C members are placed using a doubled-density node ring (N nodes at the inner ring, 2N nodes at the rim), matching the actual JA6XKQ topology rather than a naive radial layout.
- the aperture is *derived* from your input stock length and f/D; arc length scales exactly linearly with size at a fixed f/D (a pure geometric-similarity relationship), so there's a closed-form solution rather than an iterative one.

## license

[MIT](https://choosealicense.com/licenses/mit/)

## contributing

issues and PRs welcome.  this is an actively evolving design tool, and if you build a dish with it, seeing real-world results (and where the tool's assumptions break down) is genuinely useful feedback, but please commit resources to this project at your own risk as there is no guarantee that chosen parameters will result in a functional dish.

## TO DO

- generate cutting templates for wire/screen mesh
- generate cutting/drilling templates for rib members
- add material list estimator and export abilities
- add documentation for construction of the dish.  reviewing Yoshiyuki Takeyasu's (JA6XKQ) "Geodesic Parabola Antenna" might offer some insight as to the construction process, and i highly recommend anyone become acquainted with the project details there before starting anything.
