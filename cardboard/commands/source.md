# cb source — the timeline as a file

`timeline.tsx` in every bundle is a literal TSX projection of the timeline, regenerated on every save. An apply parses
it, diffs it against the revision it was printed from, and changes only the items you touched.

```
cb source print <project>                     canonical timeline.tsx on stdout
cb source check <project> [file|-] [--eval]   parse + report the ops, write nothing
cb source apply <project> [file|-] [--eval]   apply onto the project
       --prefer file|editor                   who wins an item changed on both sides (default: file)
```

`<project>` is a name, id, or bundle path. With no file argument the bundle's own `timeline.tsx` is used, so
`cb source apply Demo` re-applies a file you edited while the app was closed. `-` reads stdin.

## Rules

- **Literals only.** Attribute values must be string, number, boolean, array or object literals; the file is parsed,
  never executed. Comments are not kept.
- **Loops go through `--eval`.** `cb source apply <project> gen.tsx --eval` transpiles and runs the file in the CLI
  process (your trust domain) with a JSX factory that produces the same tree the parser would. `key` props are ignored.
  Stdin works too: `... apply <project> - --eval`.
- **`src`** is a media file name from the header (`clip.mp4`, or `clip` when unambiguous), a media id, or a path. A path
  is absolute or starts with `./`, `../` or `~/` (a bare `dir/clip.mp4` reads as a name); paths are ingested on apply
  and rewritten to the media id.
- **Item-level merge.** The `rev` in the header pins the document the file was printed from. Your changes are diffed
  against that revision and applied onto the current document, so GUI edits to other items survive. If the same item
  changed on both sides, the file wins and the apply result lists the conflict; `--prefer editor` keeps the GUI's values
  instead.
- **Stacking.** The first `<track>` is the top layer, for `<code>` graphics too; `zIndex` overrides track order.
- Errors keep the timeline untouched. Warnings apply and are listed in the header; a clip that runs past its source
  warns and holds its last frame.
- **Placeholders.** `pending` stands in for an item's content: it renders as nothing, blocks export, and `cb verify`
  lists it. Filling one keeps its `id` and drops `pending`.

## Elements

| Element        | Track kind | Content                           | Notes                                                                                                          |
| -------------- | ---------- | --------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| `<timeline>`   | —          | —                                 | `aspect`: `16:9` `4:3` `3:4` `9:16` `1:1` `4:5`                                                                |
| `<track>`      | —          | —                                 | `kind`: `video` (default), `audio`, `caption`, `adjustment`, `layout`; `name`, `locked`, `visible`, `muted`    |
| `<clip>`       | video      | `src`, `start`, `end`             | video media; sound props apply                                                                                 |
| `<image>`      | video      | `src`, `start`, `end`             |                                                                                                                |
| `<audio>`      | audio      | `src`, `start`, `end`             | `volume` 0–200, `muted`, `audioFadeIn/Out`, `playbackSpeed`                                                    |
| `<text>`       | video      | `start`, `end`, `textContent`     | font, color, align, stroke, shadow, background, `textAnimation`                                                |
| `<caption>`    | caption    | `start`, `end`                    | `captionSegments`, preset, position, colors                                                                    |
| `<code>`       | video      | `start`, `end`, `codeComposition` | `{ source, width, height, assets? }`; give each a `name` for the timeline; linted on apply, see `cb docs code` |
| `<adjustment>` | adjustment | `start`, `end`                    | `lutPresetId`, `lutIntensity`, `colorGrading`                                                                  |

Apply checks that `start` and `end` are numbers and lints the `codeComposition` of every `<code>` that is not `pending`.
The rest of the Content column is not checked: a `<clip>` without `src` or a `<text>` without `textContent` applies with
nothing to show, so mark an unfilled item `pending`, which `cb verify` counts.

Common item attributes: `id`, `name` (the clip's timeline label), `start`, `end` (timeline seconds), `in`, `out` (source
seconds; default `in=0`, `out=in+(end-start)`), `pending` (placeholder), `transform`, `crop`, `opacity`, `hidden`,
`zIndex`, `blendMode`, `keyframeTracks`, `transitionIn`, `transitionOut`, `effects`, `borderRadius`, `gaussianBlur`,
`captionComposition` (`{ mode: 'front' | 'behind-subject', fallback: 'front' | 'hide' }`). Behind-subject composition
can be set on `<caption>` or `<code>` items; the editor prepares mattes for overlapping visible video clips. `transform`
is `{ x, y, scale, rotation, flipX? }` in pixels from the centre; `keyframeTracks` animate `x`, `y`, `scale`,
`rotation`, `opacity`, `gaussianBlur`, `borderRadius`, with `easing` on the track. Unknown keys inside these objects
warn and do nothing; unknown attributes round-trip untouched so nothing the GUI wrote is lost. `timeline.d.ts` beside
the file has the full typings.

## Output

Text mode prints one line per diagnostic, then a summary such as `Applied 1 addTrack, 3 addItem (rev 1788620626855)`,
conflicts, and ingested media. `check` lists paths it would ingest as `would ingest`, not errors, and a file it would
not apply ends with `Would not apply: N errors, M warnings`. `--json` prints
`{ applied, ops, conflicts, diagnostics, ingested, wouldIngest, rev, summary: { errors, warnings, ops } }`. `check` and
`apply` exit 1 when an error kept the timeline untouched (and when an `--eval` script fails to run); a file that changes
nothing applies with exit 0.
