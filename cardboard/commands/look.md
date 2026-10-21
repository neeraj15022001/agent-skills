# cb look — see a frame

```
cb look <project> [--at <s>[,<s>...]] [--out <dir>] [--format png|jpeg] [--sheet <file>] [--width <px>]
cb look <project> --range <from,to> [--every <s>] [--out <dir>] [--format png|jpeg] [--sheet <file>] [--width <px>]
```

Renders composited frames of the timeline: every track, effect, text, caption and code clip, exactly as the preview
shows them. Files land in `--out` (default `./look`) named by time (`frame-14_200.png`, `.jpg` with `--format jpeg`);
the command prints one row per frame with the time, size and path. `--at` defaults to the app's playhead and takes a
comma list (`--at 14.2,17.3,20.5`) when you want to check several beats in one call; `--range 0,10 --every 2` gives six
evenly spaced frames (`--every` defaults to 1). The app renders at most 240 frames per call. If the app could not
capture a code clip, a note on stderr says so and the frames show the video without it; do not treat that as the graphic
being absent from the timeline.

`--sheet review.jpg` tiles every rendered frame into one image, each cell captioned with its time, so a single file
answers "does the whole section look right". `--width 640` downsamples the frames (and the sheet's cells) so review
images stay small. Use both together when iterating on a graphic:
`cb look <project> --at 30.8,31.5,33 --width 640 --sheet review.jpg`.

When the app is running, frames come from its live renderer, in the project's own tab, without changing what the user
sees. When the app is closed, `cb` from a source checkout (`pnpm cli`) renders headlessly from the bundle (`--at` then
defaults to 0), slower and without playback-only effects; the `cb` the app installs asks you to open the app instead.

Frames come at the project's export size (1920x1080 for 16:9).
