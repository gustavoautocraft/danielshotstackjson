---


---

<h2 id="shotstack-json-edit-specification-documentation">Shotstack JSON Edit Specification Documentation</h2>
<p>This document explains the structure and properties of the provided JSON object, which is used to define a video editing project for the Shotstack API. It breaks down each section and property to clarify its purpose and usage.</p>
<p>For more comprehensive details, advanced features, and the full range of available options, please refer to the official Shotstack documentation and API reference:</p>
<ul>
<li><strong>Shotstack Guide:</strong> <a href="https://shotstack.io/docs/guide/">https://shotstack.io/docs/guide/</a></li>
<li><strong>Shotstack API Reference:</strong> <a href="https://shotstack.io/docs/api/">https://shotstack.io/docs/api/</a></li>
</ul>
<p>Consulting these official resources will provide the most up-to-date and exhaustive information available.</p>
<h3 id="overview">Overview</h3>
<p>The JSON object describes a video timeline, its tracks, the clips within those tracks, and the desired output format. Shotstack processes this JSON to render a final video file.</p>
<h3 id="root-object">Root Object</h3>
<p>The main JSON object contains two top-level properties: <code>timeline</code> and <code>output</code>.</p>
<pre class=" language-json"><code class="prism  language-json"><span class="token punctuation">{</span>
  <span class="token string">"timeline"</span><span class="token punctuation">:</span> <span class="token punctuation">{</span> <span class="token operator">...</span> <span class="token punctuation">}</span><span class="token punctuation">,</span>
  <span class="token string">"output"</span><span class="token punctuation">:</span> <span class="token punctuation">{</span> <span class="token operator">...</span> <span class="token punctuation">}</span>
<span class="token punctuation">}</span>
</code></pre>
<ol>
<li><strong><code>timeline</code> (Object):</strong> Defines the structure and content of the video edit itself. This includes the background, tracks, and all the visual and audio elements.</li>
<li><strong><code>output</code> (Object):</strong> Specifies the desired format, resolution, aspect ratio, and other technical details of the final rendered video file.</li>
</ol>
<hr>
<h3 id="timeline-object"><code>timeline</code> Object</h3>
<p>The <code>timeline</code> object orchestrates the entire visual composition of the video.</p>
<pre class=" language-json"><code class="prism  language-json"><span class="token string">"timeline"</span><span class="token punctuation">:</span> <span class="token punctuation">{</span>
  <span class="token string">"background"</span><span class="token punctuation">:</span> <span class="token string">"#000000"</span><span class="token punctuation">,</span>
  <span class="token string">"tracks"</span><span class="token punctuation">:</span> <span class="token punctuation">[</span> <span class="token operator">...</span> <span class="token punctuation">]</span>
<span class="token punctuation">}</span>
</code></pre>
<ol>
<li>
<p><strong><code>background</code> (String - Hex Color):</strong></p>
<ul>
<li><strong>Purpose:</strong> Sets the default background color for the entire video canvas.</li>
<li><strong>Value:</strong> A hexadecimal color code (e.g., <code>"#000000"</code> for black, <code>"#FFFFFF"</code> for white).</li>
<li><strong>Usage:</strong> This color is visible wherever no other opaque clips cover the canvas.</li>
</ul>
</li>
<li>
<p><strong><code>tracks</code> (Array of Objects):</strong></p>
<ul>
<li><strong>Purpose:</strong> Contains the individual layers of the video. Tracks are stacked visually, similar to layers in image editing software.</li>
<li><strong>Stacking Order:</strong> Tracks are rendered from the bottom up. The first track in the array (<code>tracks[0]</code>) is the bottom-most layer, and the last track is the top-most layer. Elements in higher tracks will appear <em>on top of</em> elements in lower tracks if they occupy the same space and time.</li>
<li><strong>Content:</strong> Each object within the <code>tracks</code> array represents a single track and contains a <code>clips</code> array.</li>
</ul>
</li>
</ol>
<hr>
<h3 id="track-object-within-timeline.tracks"><code>track</code> Object (within <code>timeline.tracks</code>)</h3>
<p>Each track object holds a collection of clips that play sequentially or overlap within that specific layer.</p>
<pre class=" language-json"><code class="prism  language-json"><span class="token punctuation">{</span>
  <span class="token string">"clips"</span><span class="token punctuation">:</span> <span class="token punctuation">[</span> <span class="token operator">...</span> <span class="token punctuation">]</span>
<span class="token punctuation">}</span>
</code></pre>
<ol>
<li><strong><code>clips</code> (Array of Objects):</strong>
<ul>
<li><strong>Purpose:</strong> Contains the individual media elements (video, image, text, shapes) that appear on this specific track.</li>
<li><strong>Content:</strong> Each object within the <code>clips</code> array represents a single clip.</li>
</ul>
</li>
</ol>
<hr>
<h3 id="clip-object-within-track.clips"><code>clip</code> Object (within <code>track.clips</code>)</h3>
<p>A <code>clip</code> is the fundamental building block of the timeline, representing a piece of media or effect that appears for a specific duration.</p>
<pre class=" language-json"><code class="prism  language-json"><span class="token punctuation">{</span>
  <span class="token string">"start"</span><span class="token punctuation">:</span> <span class="token number">0</span><span class="token punctuation">,</span>
  <span class="token string">"length"</span><span class="token punctuation">:</span> <span class="token number">6.12</span><span class="token punctuation">,</span>
  <span class="token string">"asset"</span><span class="token punctuation">:</span> <span class="token punctuation">{</span> <span class="token operator">...</span> <span class="token punctuation">}</span><span class="token punctuation">,</span>
  <span class="token string">"offset"</span><span class="token punctuation">:</span> <span class="token punctuation">{</span> <span class="token operator">...</span> <span class="token punctuation">}</span><span class="token punctuation">,</span>
  <span class="token string">"opacity"</span><span class="token punctuation">:</span> <span class="token punctuation">[</span> <span class="token operator">...</span> <span class="token punctuation">]</span><span class="token punctuation">,</span>
  <span class="token string">"scale"</span><span class="token punctuation">:</span> <span class="token number">1.1</span><span class="token punctuation">,</span>
  <span class="token string">"transition"</span><span class="token punctuation">:</span> <span class="token punctuation">{</span> <span class="token operator">...</span> <span class="token punctuation">}</span><span class="token punctuation">,</span>
  <span class="token string">"fit"</span><span class="token punctuation">:</span> <span class="token string">"cover"</span><span class="token punctuation">,</span>
  <span class="token string">"transform"</span><span class="token punctuation">:</span> <span class="token punctuation">{</span> <span class="token operator">...</span> <span class="token punctuation">}</span>
  <span class="token comment">// ... other potential properties</span>
<span class="token punctuation">}</span>
</code></pre>
<ol>
<li>
<p><strong><code>start</code> (Number):</strong></p>
<ul>
<li><strong>Purpose:</strong> Defines the exact time (in seconds) on the main timeline when this clip begins to appear.</li>
<li><strong>Value:</strong> A non-negative number representing seconds from the beginning of the video (e.g., <code>0</code>, <code>6.12</code>, <code>11.68</code>).</li>
</ul>
</li>
<li>
<p><strong><code>length</code> (Number):</strong></p>
<ul>
<li><strong>Purpose:</strong> Defines the duration (in seconds) for which this clip is visible or active on the timeline.</li>
<li><strong>Value:</strong> A positive number representing the clip’s duration (e.g., <code>6.12</code>, <code>5.56</code>).</li>
</ul>
</li>
<li>
<p><strong><code>asset</code> (Object):</strong></p>
<ul>
<li><strong>Purpose:</strong> Describes the actual content of the clip. This is the core element being displayed or used.</li>
<li><strong>Content:</strong> The properties within the <code>asset</code> object vary significantly depending on its <code>type</code>. See the <strong><code>asset</code> Object Details</strong> section below.</li>
</ul>
</li>
<li>
<p><strong><code>offset</code> (Object):</strong></p>
<ul>
<li><strong>Purpose:</strong> Adjusts the position of the clip’s asset relative to the center of the canvas.</li>
<li><strong>Properties:</strong>
<ul>
<li><code>x</code> (Number or Array): Horizontal offset. <code>0</code> is the center, <code>-0.5</code> is the left edge, <code>0.5</code> is the right edge. Can be an array for animation (see <strong>Animation Syntax</strong> below).</li>
<li><code>y</code> (Number or Array): Vertical offset. <code>0</code> is the center, <code>-0.5</code> is the top edge, <code>0.5</code> is the bottom edge. Can be an array for animation.</li>
</ul>
</li>
<li><strong>Example:</strong> <code>{ "y": -0.2 }</code> moves the asset slightly upwards.</li>
</ul>
</li>
<li>
<p><strong><code>opacity</code> (Number or Array):</strong></p>
<ul>
<li><strong>Purpose:</strong> Controls the transparency of the clip’s asset.</li>
<li><strong>Value:</strong> A number between <code>0</code> (fully transparent) and <code>1</code> (fully opaque).</li>
<li><strong>Animation:</strong> Can be an array of objects to create fades or other opacity changes over time (see <strong>Animation Syntax</strong> below).</li>
<li><strong>Example:</strong> The example uses arrays for fade-in and fade-out effects.</li>
</ul>
</li>
<li>
<p><strong><code>scale</code> (Number or Array):</strong></p>
<ul>
<li><strong>Purpose:</strong> Controls the size of the clip’s asset.</li>
<li><strong>Value:</strong> A number where <code>1</code> is the original size. <code>&lt; 1</code> shrinks the asset, <code>&gt; 1</code> enlarges it.</li>
<li><strong>Animation:</strong> Can be an array of objects to animate scaling (zoom in/out), like in the Ken Burns effect examples (see <strong>Animation Syntax</strong> below).</li>
</ul>
</li>
<li>
<p><strong><code>transition</code> (Object):</strong></p>
<ul>
<li><strong>Purpose:</strong> Defines simple transitions for how a clip appears or disappears.</li>
<li><strong>Properties:</strong>
<ul>
<li><code>in</code> (String): The transition effect when the clip starts (e.g., <code>"fade"</code>).</li>
<li><code>out</code> (String): The transition effect when the clip ends (e.g., <code>"fade"</code>).</li>
</ul>
</li>
<li><strong>Note:</strong> Transitions often have a default duration (like 0.5s). Opacity animations provide more control.</li>
</ul>
</li>
<li>
<p><strong><code>fit</code> (String):</strong></p>
<ul>
<li><strong>Purpose:</strong> Determines how an image or video asset should be scaled to fit within the output dimensions if its aspect ratio differs.</li>
<li><strong>Common Values:</strong>
<ul>
<li><code>cover</code>: Scales the asset to fill the entire frame, potentially cropping edges.</li>
<li><code>contain</code>: Scales the asset to fit entirely within the frame, potentially leaving empty space (letterboxing/pillarboxing).</li>
<li><code>fill</code>: Stretches the asset to fill the frame, potentially distorting it.</li>
<li><code>none</code>: Uses the asset’s original dimensions.</li>
</ul>
</li>
</ul>
</li>
<li>
<p><strong><code>transform</code> (Object):</strong></p>
<ul>
<li><strong>Purpose:</strong> Applies transformations like rotation to the asset.</li>
<li><strong>Properties:</strong>
<ul>
<li><code>rotate</code> (Object): Controls rotation.
<ul>
<li><code>angle</code> (Number or Array): Rotation angle in degrees. Can be animated (see <strong>Animation Syntax</strong> below).</li>
</ul>
</li>
</ul>
</li>
</ul>
</li>
</ol>
<hr>
<h3 id="asset-object-details"><code>asset</code> Object Details</h3>
<p>The content and properties of the <code>asset</code> object depend entirely on its <code>type</code>.</p>
<p><strong>Common Property:</strong></p>
<ul>
<li><strong><code>type</code> (String):</strong> Specifies the kind of asset. Examples: <code>"text"</code>, <code>"image"</code>, <code>"video"</code>, <code>"shape"</code>, <code>"luma"</code>.</li>
</ul>
<p><strong>Asset Type: <code>text</code></strong></p>
<p>Used to display text on the screen.</p>
<pre class=" language-json"><code class="prism  language-json"><span class="token punctuation">{</span>
  <span class="token string">"type"</span><span class="token punctuation">:</span> <span class="token string">"text"</span><span class="token punctuation">,</span>
  <span class="token string">"text"</span><span class="token punctuation">:</span> <span class="token string">"Your text here..."</span><span class="token punctuation">,</span>
  <span class="token string">"font"</span><span class="token punctuation">:</span> <span class="token punctuation">{</span> <span class="token operator">...</span> <span class="token punctuation">}</span><span class="token punctuation">,</span>
  <span class="token string">"stroke"</span><span class="token punctuation">:</span> <span class="token punctuation">{</span> <span class="token operator">...</span> <span class="token punctuation">}</span><span class="token punctuation">,</span>
  <span class="token string">"background"</span><span class="token punctuation">:</span> <span class="token punctuation">{</span> <span class="token operator">...</span> <span class="token punctuation">}</span><span class="token punctuation">,</span>
  <span class="token string">"width"</span><span class="token punctuation">:</span> <span class="token number">600</span><span class="token punctuation">,</span>
  <span class="token string">"height"</span><span class="token punctuation">:</span> <span class="token number">110.8</span><span class="token punctuation">,</span>
  <span class="token string">"alignment"</span><span class="token punctuation">:</span> <span class="token punctuation">{</span> <span class="token operator">...</span> <span class="token punctuation">}</span>
<span class="token punctuation">}</span>
</code></pre>
<ul>
<li><strong><code>text</code> (String):</strong> The actual text content to display.</li>
<li><strong><code>font</code> (Object):</strong> Text styling options.
<ul>
<li><code>family</code> (String): Font family name (must be available to Shotstack, e.g., <code>"Roboto-Regular"</code>).</li>
<li><code>size</code> (Number): Font size in pixels.</li>
<li><code>color</code> (String - Hex Color): Text color.</li>
<li><code>weight</code> (Number): Font weight (e.g., <code>400</code> for regular, <code>600</code> for semi-bold, <code>700</code> for bold). Depends on the font family.</li>
</ul>
</li>
<li><strong><code>stroke</code> (Object):</strong> Defines an outline around the text.
<ul>
<li><code>color</code> (String - Hex Color): Outline color.</li>
<li><code>width</code> (Number): Outline thickness in pixels.</li>
</ul>
</li>
<li><strong><code>background</code> (Object):</strong> Creates a colored background box behind the text.
<ul>
<li><code>color</code> (String - Hex Color): Background color.</li>
<li><code>opacity</code> (Number): Background transparency (0-1).</li>
<li><code>borderRadius</code> (Number): Rounds the corners of the background box.</li>
<li><code>padding</code> (Number): Space (in pixels) between the text and the background box edges.</li>
</ul>
</li>
<li><strong><code>width</code> (Number):</strong> The width of the text bounding box in pixels. Text will wrap within this width.</li>
<li><strong><code>height</code> (Number):</strong> The calculated or specified height of the text bounding box. Often determined automatically but can be set.</li>
<li><strong><code>alignment</code> (Object):</strong> How the text is aligned within its bounding box.
<ul>
<li><code>horizontal</code> (String): <code>"left"</code>, <code>"center"</code>, <code>"right"</code>.</li>
<li><code>vertical</code> (String): <code>"top"</code>, <code>"center"</code>, <code>"bottom"</code>.</li>
</ul>
</li>
</ul>
<p><strong>Asset Type: <code>image</code></strong></p>
<p>Used to display static images.</p>
<pre class=" language-json"><code class="prism  language-json"><span class="token punctuation">{</span>
  <span class="token string">"type"</span><span class="token punctuation">:</span> <span class="token string">"image"</span><span class="token punctuation">,</span>
  <span class="token string">"src"</span><span class="token punctuation">:</span> <span class="token string">"https://url.to/your/image.jpg"</span>
<span class="token punctuation">}</span>
</code></pre>
<ul>
<li><strong><code>src</code> (String - URL):</strong> The URL of the image file (JPEG, PNG, GIF, etc.).</li>
</ul>
<p><strong>Asset Type: <code>video</code></strong></p>
<p>Used to display video files.</p>
<pre class=" language-json"><code class="prism  language-json"><span class="token punctuation">{</span>
  <span class="token string">"type"</span><span class="token punctuation">:</span> <span class="token string">"video"</span><span class="token punctuation">,</span>
  <span class="token string">"src"</span><span class="token punctuation">:</span> <span class="token string">"https://url.to/your/video.mp4"</span><span class="token punctuation">,</span>
  <span class="token string">"trim"</span><span class="token punctuation">:</span> <span class="token number">0.2</span><span class="token punctuation">,</span>
  <span class="token string">"chromaKey"</span><span class="token punctuation">:</span> <span class="token punctuation">{</span> <span class="token operator">...</span> <span class="token punctuation">}</span>
<span class="token punctuation">}</span>
</code></pre>
<ul>
<li><strong><code>src</code> (String - URL):</strong> The URL of the video file (MP4, MOV, etc.).</li>
<li><strong><code>trim</code> (Number, Optional):</strong> Skips the first <code>n</code> seconds of the source video file.</li>
<li><strong><code>chromaKey</code> (Object, Optional):</strong> Used for green/blue screen removal.
<ul>
<li><code>color</code> (String - Hex Color): The color to make transparent (e.g., <code>"#24d590"</code> for green).</li>
<li><code>threshold</code> (Number): Sensitivity of the keying (0-255, higher is more sensitive).</li>
<li><code>halo</code> (Number): Helps reduce the “halo” effect around the keyed subject (0-255).</li>
</ul>
</li>
</ul>
<p><strong>Asset Type: <code>shape</code></strong></p>
<p>Used to draw simple geometric shapes.</p>
<pre class=" language-json"><code class="prism  language-json"><span class="token punctuation">{</span>
  <span class="token string">"type"</span><span class="token punctuation">:</span> <span class="token string">"shape"</span><span class="token punctuation">,</span>
  <span class="token string">"shape"</span><span class="token punctuation">:</span> <span class="token string">"rectangle"</span><span class="token punctuation">,</span>
  <span class="token string">"rectangle"</span><span class="token punctuation">:</span> <span class="token punctuation">{</span>
    <span class="token string">"width"</span><span class="token punctuation">:</span> <span class="token number">720</span><span class="token punctuation">,</span>
    <span class="token string">"height"</span><span class="token punctuation">:</span> <span class="token number">1280</span>
  <span class="token punctuation">}</span><span class="token punctuation">,</span>
  <span class="token string">"fill"</span><span class="token punctuation">:</span> <span class="token punctuation">{</span>
    <span class="token string">"color"</span><span class="token punctuation">:</span> <span class="token string">"#FFFFFF"</span>
  <span class="token punctuation">}</span>
<span class="token punctuation">}</span>
</code></pre>
<ul>
<li><strong><code>shape</code> (String):</strong> The type of shape (e.g., <code>"rectangle"</code>, <code>"circle"</code>, <code>"triangle"</code>).</li>
<li><strong><code>rectangle</code> (Object, if <code>shape</code> is <code>"rectangle"</code>):</strong> Defines rectangle dimensions.
<ul>
<li><code>width</code> (Number): Width in pixels.</li>
<li><code>height</code> (Number): Height in pixels.</li>
</ul>
</li>
<li><strong><code>fill</code> (Object):</strong> Defines the shape’s fill color.
<ul>
<li><code>color</code> (String - Hex Color): The fill color.</li>
</ul>
</li>
</ul>
<p><strong>Asset Type: <code>luma</code></strong></p>
<p>Used to create a Luma Matte mask. The track <em>immediately below</em> the track containing the luma asset will only be visible where the luma asset is light/white. Dark/black areas of the luma asset will mask out (make transparent) the content below.</p>
<pre class=" language-json"><code class="prism  language-json"><span class="token punctuation">{</span>
  <span class="token string">"type"</span><span class="token punctuation">:</span> <span class="token string">"luma"</span><span class="token punctuation">,</span>
  <span class="token string">"src"</span><span class="token punctuation">:</span> <span class="token string">"https://url.to/your/luma_matte.jpg"</span>
<span class="token punctuation">}</span>
</code></pre>
<ul>
<li><strong><code>src</code> (String - URL):</strong> The URL of the image or video file to be used as the luma matte. White areas reveal, black areas conceal the track below.</li>
</ul>
<hr>
<h3 id="animation-syntax">Animation Syntax</h3>
<p>Several properties (<code>opacity</code>, <code>scale</code>, <code>offset</code>, <code>transform.rotate.angle</code>) can be animated over the duration of a <em>clip</em>. This is done by providing an array of “tween” objects instead of a single static value.</p>
<pre class=" language-json"><code class="prism  language-json"><span class="token string">"property"</span><span class="token punctuation">:</span> <span class="token punctuation">[</span>
  <span class="token punctuation">{</span>
    <span class="token string">"from"</span><span class="token punctuation">:</span> value1<span class="token punctuation">,</span> <span class="token comment">// Starting value</span>
    <span class="token string">"to"</span><span class="token punctuation">:</span> value2<span class="token punctuation">,</span>   <span class="token comment">// Ending value</span>
    <span class="token string">"start"</span><span class="token punctuation">:</span> <span class="token number">0</span><span class="token punctuation">,</span>     <span class="token comment">// Start time *relative to the clip start* (in seconds)</span>
    <span class="token string">"length"</span><span class="token punctuation">:</span> <span class="token number">0.5</span><span class="token punctuation">,</span>  <span class="token comment">// Duration of this specific animation segment (in seconds)</span>
    <span class="token string">"interpolation"</span><span class="token punctuation">:</span> <span class="token string">"bezier"</span><span class="token punctuation">,</span> <span class="token comment">// Optional: How values change (linear, bezier)</span>
    <span class="token string">"easing"</span><span class="token punctuation">:</span> <span class="token string">"easeOutCubic"</span>    <span class="token comment">// Optional: The acceleration curve (e.g., easeIn, easeOut, easeInOut, linear)</span>
  <span class="token punctuation">}</span><span class="token punctuation">,</span>
  <span class="token punctuation">{</span> <span class="token comment">// Optional: Next segment of the animation</span>
    <span class="token string">"from"</span><span class="token punctuation">:</span> value2<span class="token punctuation">,</span>
    <span class="token string">"to"</span><span class="token punctuation">:</span> value3<span class="token punctuation">,</span>
    <span class="token string">"start"</span><span class="token punctuation">:</span> <span class="token number">0.5</span><span class="token punctuation">,</span> <span class="token comment">// Starts where the previous one ended</span>
    <span class="token string">"length"</span><span class="token punctuation">:</span> <span class="token number">1.0</span>
    <span class="token comment">// ... potentially different easing/interpolation</span>
  <span class="token punctuation">}</span>
  <span class="token comment">// ... more segments</span>
<span class="token punctuation">]</span>
</code></pre>
<ul>
<li><strong><code>from</code>:</strong> The value of the property at the beginning of this animation segment.</li>
<li><strong><code>to</code>:</strong> The value of the property at the end of this animation segment.</li>
<li><strong><code>start</code>:</strong> The time <em>within the clip’s duration</em> (starting at 0) when this segment begins.</li>
<li><strong><code>length</code>:</strong> How long this specific segment of the animation lasts.</li>
<li><strong><code>interpolation</code> (Optional):</strong> How the value changes between <code>from</code> and <code>to</code>. <code>"linear"</code> is default, <code>"bezier"</code> allows for easing curves.</li>
<li><strong><code>easing</code> (Optional):</strong> When using <code>bezier</code> interpolation, this defines the speed curve of the animation (e.g., <code>easeInCubic</code>, <code>easeOutCubic</code>, <code>easeInOutQuad</code>, <code>linear</code>). Affects the acceleration/deceleration.</li>
</ul>
<hr>
<h3 id="output-object"><code>output</code> Object</h3>
<p>This object defines the technical specifications for the final rendered video file.</p>
<pre class=" language-json"><code class="prism  language-json"><span class="token string">"output"</span><span class="token punctuation">:</span> <span class="token punctuation">{</span>
  <span class="token string">"format"</span><span class="token punctuation">:</span> <span class="token string">"mp4"</span><span class="token punctuation">,</span>
  <span class="token string">"resolution"</span><span class="token punctuation">:</span> <span class="token string">"hd"</span><span class="token punctuation">,</span>
  <span class="token string">"aspectRatio"</span><span class="token punctuation">:</span> <span class="token string">"9:16"</span><span class="token punctuation">,</span>
  <span class="token string">"fps"</span><span class="token punctuation">:</span> <span class="token number">30</span>
<span class="token punctuation">}</span>
</code></pre>
<ol>
<li>
<p><strong><code>format</code> (String):</strong></p>
<ul>
<li><strong>Purpose:</strong> The desired file container format.</li>
<li><strong>Value:</strong> <code>"mp4"</code>, <code>"gif"</code>, <code>"mp3"</code>, etc.</li>
</ul>
</li>
<li>
<p><strong><code>resolution</code> (String):</strong></p>
<ul>
<li><strong>Purpose:</strong> The vertical resolution of the output video.</li>
<li><strong>Value:</strong> Standard definitions like <code>"sd"</code> (480p), <code>"hd"</code> (720p), <code>"1080"</code> (1080p). Can also be custom pixel values.</li>
</ul>
</li>
<li>
<p><strong><code>aspectRatio</code> (String):</strong></p>
<ul>
<li><strong>Purpose:</strong> The width-to-height ratio of the video frame.</li>
<li><strong>Value:</strong> Common ratios like <code>"16:9"</code> (standard widescreen), <code>"9:16"</code> (vertical video), <code>"1:1"</code> (square).</li>
</ul>
</li>
<li>
<p><strong><code>fps</code> (Number):</strong></p>
<ul>
<li><strong>Purpose:</strong> Frames Per Second. Controls the smoothness of motion.</li>
<li><strong>Value:</strong> Common values are <code>24</code>, <code>25</code>, <code>30</code>, <code>60</code>.</li>
</ul>
</li>
</ol>
<hr>
<p>This documentation covers all properties present in the example JSON. By understanding these components, you can modify this structure or create new JSON edits to generate diverse videos using the Shotstack API.</p>

