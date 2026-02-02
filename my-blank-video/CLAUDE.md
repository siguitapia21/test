# Remotion Video Project

This is a Remotion project for creating videos programmatically with React and TypeScript.

## Tech Stack

- **Remotion** - React framework for creating videos
- **TypeScript** - Type-safe JavaScript
- **Tailwind CSS v4** - Utility-first CSS framework
- **React 19** - UI library

## Project Structure

```
src/
├── Composition.tsx    # Main video composition
├── Root.tsx           # Registers all compositions
├── index.ts           # Entry point
└── index.css          # Tailwind CSS imports
```

## Key Concepts

### Compositions
A composition is a video template defined by:
- `id` - Unique identifier
- `component` - React component to render
- `durationInFrames` - Total frames (duration = frames / fps)
- `fps` - Frames per second
- `width` / `height` - Video dimensions

### Using Time in Components
```tsx
import { useCurrentFrame, useVideoConfig } from "remotion";

const MyComponent = () => {
  const frame = useCurrentFrame();
  const { fps, durationInFrames } = useVideoConfig();
  // frame starts at 0, increments each frame
};
```

### Animations
Use `interpolate()` for smooth animations:
```tsx
import { interpolate, useCurrentFrame } from "remotion";

const opacity = interpolate(
  frame,
  [0, 30],      // input range (frames)
  [0, 1],       // output range
  { extrapolateRight: "clamp" }
);
```

### Sequences
Use `<Sequence>` to time elements:
```tsx
import { Sequence } from "remotion";

<Sequence from={0} durationInFrames={60}>
  <FirstScene />
</Sequence>
<Sequence from={60} durationInFrames={90}>
  <SecondScene />
</Sequence>
```

### Spring Animations
```tsx
import { spring, useCurrentFrame, useVideoConfig } from "remotion";

const { fps } = useVideoConfig();
const scale = spring({
  frame,
  fps,
  config: { damping: 10, stiffness: 100 }
});
```

## Commands

```bash
npm run dev       # Start Remotion Studio at localhost:3000
npm run build     # Bundle the project
npm run lint      # Run linter
npm run upgrade   # Upgrade Remotion packages
```

## Rendering Videos

```bash
# Render to MP4
npx remotion render src/index.ts MyComp out/video.mp4

# Render specific frames
npx remotion render src/index.ts MyComp out/video.mp4 --frames=0-100

# Render as GIF
npx remotion render src/index.ts MyComp out/video.gif --codec=gif
```

## Best Practices

1. **Keep compositions pure** - Components should render the same output for the same frame
2. **Use `staticFile()`** for assets in `public/` folder
3. **Avoid `useEffect` for animations** - Use `useCurrentFrame()` instead
4. **Test with Remotion Studio** before rendering
5. **Use Tailwind classes** for styling - configured and ready to use

## Video Presets

| Use Case | Resolution | FPS | Aspect Ratio |
|----------|------------|-----|--------------|
| YouTube | 1920x1080 | 30 | 16:9 |
| Instagram Reel | 1080x1920 | 30 | 9:16 |
| TikTok | 1080x1920 | 30 | 9:16 |
| Square | 1080x1080 | 30 | 1:1 |

## Adding New Compositions

1. Create a new component in `src/`
2. Register it in `src/Root.tsx`:
```tsx
<Composition
  id="NewVideo"
  component={NewVideoComponent}
  durationInFrames={150}
  fps={30}
  width={1920}
  height={1080}
/>
```

## Resources

- [Remotion Documentation](https://www.remotion.dev/docs)
- [Remotion GitHub](https://github.com/remotion-dev/remotion)
