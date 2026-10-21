# Put captions or graphics behind a subject

Use the editor's native subject occlusion for captions and code graphics. It keeps the text or graphic editable and
places the visible person from overlapping video clips in front of it. The editor prepares the masks for those clips;
the agent does not need to create them with `cb media matte`.

## Synced captions

Choose one of the nine Behind Subject styles from the [caption preset gallery](previews/caption-presets.jpg), then pass
its printed ID to `--preset`:

```sh
cb audio captions <project> --apply --preset <chosen-preset-id>
```

The preset supplies the behind-subject composition. To change the wording or fine-tune the font, size, color, or
position, edit the caption item and keep its `captionComposition` set to `behind-subject`.

## Custom captions

Use a caption item for hand-written or custom-styled text. Set its composition mode explicitly; caption styling can
still come from any preset and its per-item overrides:

```tsx
<caption
  name="Opening thought"
  start={2}
  end={5}
  captionText="Make the unexpected feel easy"
  captionPresetId="minimal-modern"
  captionComposition={{ mode: 'behind-subject', fallback: 'front' }}
/>
```

## Code graphics

Use a `<code>` item for custom animated typography, shapes, or other graphics. Set the same `captionComposition` mode on
the code item and keep the graphic's root background transparent so the footage remains visible. Read `cb docs code` for
the code composition contract.

```tsx
<code
  name="Headline"
  start={2}
  end={5}
  captionComposition={{ mode: 'behind-subject', fallback: 'front' }}
  codeComposition={{
    width: 1920,
    height: 1080,
    source:
      "export default function Graphic({ width, height }) { return <div style={{ width, height, display: 'flex', alignItems: 'center', justifyContent: 'center', fontFamily: 'Inter, sans-serif', fontSize: 120, color: '#F3EEE6' }}>Behind the speaker</div>; }",
  }}
/>
```

When the matte is ready, the subject appears in front of the caption or graphic. Until then, `fallback: 'front'` keeps
it visible. Run `cb verify <project>` and inspect representative frames with `cb look <project> --at <times>` after
processing finishes.

## Other layers

Native subject occlusion applies to caption and code items. For a title or other layer that cannot request this mode,
`cb media matte --occlusion` creates a grayscale person mask but does not attach it to that timeline layer. Read
`cb docs media` for the FFmpeg compositing example; that route bakes the result into a new media file.
