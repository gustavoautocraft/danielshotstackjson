## Shotstack JSON Edit Specification Documentation

This document explains the structure and properties of the provided JSON object, which is used to define a video editing project for the Shotstack API. It breaks down each section and property to clarify its purpose and usage.

For more comprehensive details, advanced features, and the full range of available options, please refer to the official Shotstack documentation and API reference:

*   **Shotstack Guide:** https://shotstack.io/docs/guide/
*   **Shotstack API Reference:** https://shotstack.io/docs/api/

Consulting these official resources will provide the most up-to-date and exhaustive information available.

### Overview

The JSON object describes a video timeline, its tracks, the clips within those tracks, and the desired output format. Shotstack processes this JSON to render a final video file.

### Root Object

The main JSON object contains two top-level properties: `timeline` and `output`.

```json
{
  "timeline": { ... },
  "output": { ... }
}
```

1.  **`timeline` (Object):** Defines the structure and content of the video edit itself. This includes the background, tracks, and all the visual and audio elements.
2.  **`output` (Object):** Specifies the desired format, resolution, aspect ratio, and other technical details of the final rendered video file.

---

### `timeline` Object

The `timeline` object orchestrates the entire visual composition of the video.

```json
"timeline": {
  "background": "#000000",
  "tracks": [ ... ]
}
```

1.  **`background` (String - Hex Color):**
    *   **Purpose:** Sets the default background color for the entire video canvas.
    *   **Value:** A hexadecimal color code (e.g., `"#000000"` for black, `"#FFFFFF"` for white).
    *   **Usage:** This color is visible wherever no other opaque clips cover the canvas.

2.  **`tracks` (Array of Objects):**
    *   **Purpose:** Contains the individual layers of the video. Tracks are stacked visually, similar to layers in image editing software.
    *   **Stacking Order:** Tracks are rendered starting with the top layer. The first track in the array (`tracks[0]`) is the **top-most layer**, the second track (`tracks[1]`) is rendered directly below the first, and so on down to the last track in the array, which forms the bottom-most layer (closest to the `timeline.background`). Elements in tracks with lower indices (e.g., `tracks[0]`) will appear *on top of* elements in tracks with higher indices (e.g., `tracks[1]`) if they occupy the same space and time on the canvas.
    *   **Content:** Each object within the `tracks` array represents a single track and contains a `clips` array.

---

### `track` Object (within `timeline.tracks`)

Each track object holds a collection of clips that play sequentially or overlap within that specific layer.

```json
{
  "clips": [ ... ]
}
```

1.  **`clips` (Array of Objects):**
    *   **Purpose:** Contains the individual media elements (video, image, text, shapes) that appear on this specific track.
    *   **Content:** Each object within the `clips` array represents a single clip.

---

### `clip` Object (within `track.clips`)

A `clip` is the fundamental building block of the timeline, representing a piece of media or effect that appears for a specific duration.

```json
{
  "start": 0,
  "length": 6.12,
  "asset": { ... },
  "offset": { ... },
  "opacity": [ ... ],
  "scale": 1.1,
  "transition": { ... },
  "fit": "cover",
  "transform": { ... }
  // ... other potential properties
}
```

1.  **`start` (Number):**
    *   **Purpose:** Defines the exact time (in seconds) on the main timeline when this clip begins to appear.
    *   **Value:** A non-negative number representing seconds from the beginning of the video (e.g., `0`, `6.12`, `11.68`).

2.  **`length` (Number):**
    *   **Purpose:** Defines the duration (in seconds) for which this clip is visible or active on the timeline.
    *   **Value:** A positive number representing the clip's duration (e.g., `6.12`, `5.56`).

3.  **`asset` (Object):**
    *   **Purpose:** Describes the actual content of the clip. This is the core element being displayed or used.
    *   **Content:** The properties within the `asset` object vary significantly depending on its `type`. See the **`asset` Object Details** section below.

4.  **`offset` (Object):**
    *   **Purpose:** Adjusts the position of the clip's asset relative to the center of the canvas.
    *   **Properties:**
        *   `x` (Number or Array): Horizontal offset. `0` is the center, `-0.5` is the left edge, `0.5` is the right edge. Can be an array for animation (see **Animation Syntax** below).
        *   `y` (Number or Array): Vertical offset. `0` is the center, `-0.5` is the top edge, `0.5` is the bottom edge. Can be an array for animation.
    *   **Example:** `{ "y": -0.2 }` moves the asset slightly upwards.

5.  **`opacity` (Number or Array):**
    *   **Purpose:** Controls the transparency of the clip's asset.
    *   **Value:** A number between `0` (fully transparent) and `1` (fully opaque).
    *   **Animation:** Can be an array of objects to create fades or other opacity changes over time (see **Animation Syntax** below).
    *   **Example:** The example uses arrays for fade-in and fade-out effects.

6.  **`scale` (Number or Array):**
    *   **Purpose:** Controls the size of the clip's asset.
    *   **Value:** A number where `1` is the original size. `< 1` shrinks the asset, `> 1` enlarges it.
    *   **Animation:** Can be an array of objects to animate scaling (zoom in/out), like in the Ken Burns effect examples (see **Animation Syntax** below).

7.  **`transition` (Object):**
    *   **Purpose:** Defines simple transitions for how a clip appears or disappears.
    *   **Properties:**
        *   `in` (String): The transition effect when the clip starts (e.g., `"fade"`).
        *   `out` (String): The transition effect when the clip ends (e.g., `"fade"`).
    *   **Note:** Transitions often have a default duration (like 0.5s). Opacity animations provide more control.

8.  **`fit` (String):**
    *   **Purpose:** Determines how an image or video asset should be scaled to fit within the output dimensions if its aspect ratio differs.
    *   **Common Values:**
        *   `cover`: Scales the asset to fill the entire frame, potentially cropping edges.
        *   `contain`: Scales the asset to fit entirely within the frame, potentially leaving empty space (letterboxing/pillarboxing).
        *   `fill`: Stretches the asset to fill the frame, potentially distorting it.
        *   `none`: Uses the asset's original dimensions.

9.  **`transform` (Object):**
    *   **Purpose:** Applies transformations like rotation to the asset.
    *   **Properties:**
        *   `rotate` (Object): Controls rotation.
            *   `angle` (Number or Array): Rotation angle in degrees. Can be animated (see **Animation Syntax** below).

---

### `asset` Object Details

The content and properties of the `asset` object depend entirely on its `type`.

**Common Property:**

*   **`type` (String):** Specifies the kind of asset. Examples: `"text"`, `"image"`, `"video"`, `"shape"`, `"luma"`.

**Asset Type: `text`**

Used to display text on the screen.

```json
{
  "type": "text",
  "text": "Your text here...",
  "font": { ... },
  "stroke": { ... },
  "background": { ... },
  "width": 600,
  "height": 110.8,
  "alignment": { ... }
}
```

*   **`text` (String):** The actual text content to display.
*   **`font` (Object):** Text styling options.
    *   `family` (String): Font family name (must be available to Shotstack, e.g., `"Roboto-Regular"`).
    *   `size` (Number): Font size in pixels.
    *   `color` (String - Hex Color): Text color.
    *   `weight` (Number): Font weight (e.g., `400` for regular, `600` for semi-bold, `700` for bold). Depends on the font family.
*   **`stroke` (Object):** Defines an outline around the text.
    *   `color` (String - Hex Color): Outline color.
    *   `width` (Number): Outline thickness in pixels.
*   **`background` (Object):** Creates a colored background box behind the text.
    *   `color` (String - Hex Color): Background color.
    *   `opacity` (Number): Background transparency (0-1).
    *   `borderRadius` (Number): Rounds the corners of the background box.
    *   `padding` (Number): Space (in pixels) between the text and the background box edges.
*   **`width` (Number):** The width of the text bounding box in pixels. Text will wrap within this width.
*   **`height` (Number):** The calculated or specified height of the text bounding box. Often determined automatically but can be set.
*   **`alignment` (Object):** How the text is aligned within its bounding box.
    *   `horizontal` (String): `"left"`, `"center"`, `"right"`.
    *   `vertical` (String): `"top"`, `"center"`, `"bottom"`.

**Asset Type: `image`**

Used to display static images.

```json
{
  "type": "image",
  "src": "https://url.to/your/image.jpg"
}
```

*   **`src` (String - URL):** The URL of the image file (JPEG, PNG, GIF, etc.).

**Asset Type: `video`**

Used to display video files.

```json
{
  "type": "video",
  "src": "https://url.to/your/video.mp4",
  "trim": 0.2,
  "chromaKey": { ... }
}
```

*   **`src` (String - URL):** The URL of the video file (MP4, MOV, etc.).
*   **`trim` (Number, Optional):** Skips the first `n` seconds of the source video file.
*   **`chromaKey` (Object, Optional):** Used for green/blue screen removal.
    *   `color` (String - Hex Color): The color to make transparent (e.g., `"#24d590"` for green).
    *   `threshold` (Number): Sensitivity of the keying (0-255, higher is more sensitive).
    *   `halo` (Number): Helps reduce the "halo" effect around the keyed subject (0-255).

**Asset Type: `shape`**

Used to draw simple geometric shapes.

```json
{
  "type": "shape",
  "shape": "rectangle",
  "rectangle": {
    "width": 720,
    "height": 1280
  },
  "fill": {
    "color": "#FFFFFF"
  }
}
```

*   **`shape` (String):** The type of shape (e.g., `"rectangle"`, `"circle"`, `"triangle"`).
*   **`rectangle` (Object, if `shape` is `"rectangle"`):** Defines rectangle dimensions.
    *   `width` (Number): Width in pixels.
    *   `height` (Number): Height in pixels.
*   **`fill` (Object):** Defines the shape's fill color.
    *   `color` (String - Hex Color): The fill color.

**Asset Type: `luma`**

Used to create a Luma Matte mask. The track *immediately below* the track containing the luma asset will only be visible where the luma asset is light/white. Dark/black areas of the luma asset will mask out (make transparent) the content below.

```json
{
  "type": "luma",
  "src": "https://url.to/your/luma_matte.jpg"
}
```

*   **`src` (String - URL):** The URL of the image or video file to be used as the luma matte. White areas reveal, black areas conceal the track below.

---

### Animation Syntax

Several properties (`opacity`, `scale`, `offset`, `transform.rotate.angle`) can be animated over the duration of a *clip*. This is done by providing an array of "tween" objects instead of a single static value.

```json
"property": [
  {
    "from": value1, // Starting value
    "to": value2,   // Ending value
    "start": 0,     // Start time *relative to the clip start* (in seconds)
    "length": 0.5,  // Duration of this specific animation segment (in seconds)
    "interpolation": "bezier", // Optional: How values change (linear, bezier)
    "easing": "easeOutCubic"    // Optional: The acceleration curve (e.g., easeIn, easeOut, easeInOut, linear)
  },
  { // Optional: Next segment of the animation
    "from": value2,
    "to": value3,
    "start": 0.5, // Starts where the previous one ended
    "length": 1.0
    // ... potentially different easing/interpolation
  }
  // ... more segments
]
```

*   **`from`:** The value of the property at the beginning of this animation segment.
*   **`to`:** The value of the property at the end of this animation segment.
*   **`start`:** The time *within the clip's duration* (starting at 0) when this segment begins.
*   **`length`:** How long this specific segment of the animation lasts.
*   **`interpolation` (Optional):** How the value changes between `from` and `to`. `"linear"` is default, `"bezier"` allows for easing curves.
*   **`easing` (Optional):** When using `bezier` interpolation, this defines the speed curve of the animation (e.g., `easeInCubic`, `easeOutCubic`, `easeInOutQuad`, `linear`). Affects the acceleration/deceleration.

---

### `output` Object

This object defines the technical specifications for the final rendered video file.

```json
"output": {
  "format": "mp4",
  "resolution": "hd",
  "aspectRatio": "9:16",
  "fps": 30
}
```

1.  **`format` (String):**
    *   **Purpose:** The desired file container format.
    *   **Value:** `"mp4"`, `"gif"`, `"mp3"`, etc.

2.  **`resolution` (String):**
    *   **Purpose:** The vertical resolution of the output video.
    *   **Value:** Standard definitions like `"sd"` (480p), `"hd"` (720p), `"1080"` (1080p). Can also be custom pixel values.

3.  **`aspectRatio` (String):**
    *   **Purpose:** The width-to-height ratio of the video frame.
    *   **Value:** Common ratios like `"16:9"` (standard widescreen), `"9:16"` (vertical video), `"1:1"` (square).

4.  **`fps` (Number):**
    *   **Purpose:** Frames Per Second. Controls the smoothness of motion.
    *   **Value:** Common values are `24`, `25`, `30`, `60`.

---

This documentation covers all properties present in the example JSON. By understanding these components, you can modify this structure or create new JSON edits to generate diverse videos using the Shotstack API.
