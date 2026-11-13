# Instructions: Writing Cinematic Stylized 3D Image Prompts in the FLUX 2 Prompt Builder JSON Format

## 1. Purpose

These instructions explain how to write AI image-generation prompts in the **Cinematic Stylized 3D** visual style using the structured JSON format produced by the **FLUX 2 Prompt Builder v2** (`flux2-prompt-node-v2.html`).

The procedure turns each documentary shot into one JSON prompt. Every field of the JSON carries one part of the visual style:

- `prompt` describes the scene.
- `style` sets the rendering style.
- `camera` sets the virtual camera.
- `lighting` sets the lighting design.
- `colors` sets the palette and the mood.
- `composition` sets the frame structure.

The expected result is a set of prompts that produces imagery with these qualities:

- It sits between realistic CGI and sophisticated animation.
- It is cinematic, mature and accurate to its period.
- It is visually consistent across the whole documentary.
- It is never photorealistic, toy-like, cartoonish or cluttered.

**Guiding principle:** *Stylized, not cartoonish. Realistic, not photorealistic. Cinematic, not theatrical. Detailed, not cluttered. Artistic, yet grounded in reality.*

## 2. Prerequisites

- **Knowledge:**
  - 3D form, materials, cinematic lighting, color grading, cinematography and composition.
  - Basic JSON syntax: objects, arrays, strings and integers.
- **Tool:** the file `flux2-prompt-node-v2.html`, opened in a web browser. The file runs locally and needs no installation.
- **Browser features:**
  - The image-based color picker works without a secure connection.
  - The screen eyedropper button works only over HTTPS or on `localhost`.
- **Subject context:** for each shot you need:
  - the documentary subject;
  - the historical period;
  - the location;
  - the emotional tone;
  - the narrative purpose.
- **Reference material:** verified historical and technical references for clothing, machinery, architecture, tools and equipment. The style guide requires accuracy but names no sources.
- **Series constants document:** a text file that stores, word for word, the style string, the palette family and the descriptions of recurring characters (see Step 4).
- **Generation pipeline:** a FLUX 2 workflow that accepts the JSON prompt, for example a ComfyUI prompt node. Neither source document describes how the JSON is passed to the model.

## 3. Procedure

### Phase A: Plan the frame before writing

1. **Define the single narrative idea.** Write one sentence that states what the frame must communicate, for example: *"Early industrial labour was dwarfed by its machines."*
   - Each frame must carry exactly one idea and have one dominant subject.
   - Keep this sentence as a planning note. Translate it into visible content in later steps. Do not paste abstract statements into the JSON.

2. **Classify the scene type.** This is a **decision point**, and the choice controls the lighting, aperture, angle and mood in later steps:

   | Scene type | Lighting | Depth of field | Typical mood |
   |---|---|---|---|
   | **Dramatic** | Stronger contrast, more pronounced shadows | Selective (moderate) | Tense, mysterious, melancholic |
   | **Scientific / technological explanation** | Cleaner lighting, controlled reflections, clearly visible structure | Deeper, so structure stays readable | Exploratory, technological |

3. **List and verify the subjects.** For the dominant subject and every supporting element:
   - record the correct period details from your references;
   - assign the shape language for its type:

   | Subject | Shape language to describe |
   |---|---|
   | Human characters | Natural proportions, subtly refined facial and body shapes |
   | Industrial robots | Precise geometric structures, clearly readable mechanical components |
   | Military equipment | Functional, robust forms, sophisticated surface detailing |
   | Architecture | Recognizable structural elements, controlled geometric complexity |
   | Natural environments | Organic shapes, simplified but convincing surfaces |

4. **Load or create the series constants.** For the first shot of a documentary, create the constants document. For every later shot, copy from it without rewording. It holds:
   - the canonical `style` string (Step 10);
   - the palette family and grading notes (Steps 19–21);
   - a fixed description block for each recurring character: age, build, face, hair, clothing and materials.

### Phase B: Prepare the Prompt Builder

5. **Do not load a built-in preset as the final prompt.** Every preset in the **Load Preset** menu is photographic (portrait, landscape, film noir and so on). Loading a preset overwrites every field, including the color palette.
   - **Optional:** load a preset only to study the field structure, then replace every value.

6. **Clear the default palette.** When the file opens, the palette already contains `#2A5BDA` (a saturated cobalt blue) and `amber glow`. Remove both chips with the **×** buttons before you add style colors. If you leave them, they appear in the output.

7. **Set the Output Options:**
   - **Include empty fields:** **off**. When it is on, empty strings are written into the JSON and reach the model.
   - **Numeric lens-mm format:** **on** (the default). The lens is then written as the integer key `lens-mm`.

8. **Learn the output schema.** The tool writes keys in this fixed order:

   | JSON key | UI field | Type | When it is written |
   |---|---|---|---|
   | `prompt` | Main Prompt | string | When the field is not empty |
   | `style` | Style | string | When the field is not empty |
   | `camera.angle` | Angle | string | When the field is not empty |
   | `camera.distance` | Shot / Distance | string | When the field is not empty |
   | `camera.lens-mm` | Lens (mm) | integer (the first number found) | Numeric toggle on and a digit is present |
   | `camera.lens` | Lens (mm) | string | Numeric toggle off, or no digit in the value (for example `fisheye`) |
   | `camera.f-number` | Aperture | string, e.g. `"f/4"` | When the field is not empty |
   | `camera.ISO` | ISO | integer if numeric, otherwise string | When the field is not empty |
   | `camera.focus` | Focus Description | string | When the field is not empty |
   | `film_stock` | Camera / Film Stock | string | **Only** when the field is not empty. The empty-fields toggle does not affect it. |
   | `lighting` | Lighting | string | When the field is not empty |
   | `colors.palette` | Color chips | array of strings (hex codes or color names) | When at least one chip is not empty |
   | `colors.mood` | Mood | string | When the field is not empty |
   | `composition` | Composition | string | When the field is not empty |

   - The Style, Camera/Film Stock, Lighting, Mood and Composition fields each have a dropdown and a **pencil (edit) toggle**.
   - Choosing a dropdown entry fills the text field with that preset's text.
   - The pencil toggle shows the raw text so you can edit it.
   - The Angle, Shot/Distance, Lens, Aperture and ISO fields accept free text as well as the ▼ menu values.

### Phase C: Write the `prompt` field

9. **Write the scene as one descriptive paragraph**, in the same single-paragraph form as the tool's built-in presets. Use this order:
   10. **Dominant subject**, with its shape language (Step 3).
   11. **Action or state.** Keep expressions and poses controlled and credible. Do not describe melodramatic gestures.
   12. **Environment.** State location, period and function through recognizable architectural and contextual details.
   13. **Verified period and technical details:** clothing, machinery, tools, structures.
   14. **Material identity.** Name each key material and how it should read, for example: *matte painted cast iron with soft edge wear*, *oiled timber*, *natural woven cotton*, *brushed aluminium with controlled reflections*.
   15. **Atmosphere:** haze, dust or volumetric depth, only when the scene needs it.
   16. **Supporting elements**, described as subordinate to the dominant subject. Leave out elements that do not support the narrative idea.

   Rules for this field:
   - **Stylize form, not function.** Machines keep recognizable components, realistic joints and believable engineering. People stay anatomically convincing, with slightly simplified facial structure.
   - **Paste recurring character blocks** from the constants document word for word.
   - **Describe what you want in positive terms.** The schema has no negative-prompt field, so write *"matte satin paint with soft highlights"*, not *"not glossy"*.
   - **Use visual metaphors only** when they make an abstract concept clearer and stay easy to understand.
   - **Avoid these descriptors:** *photorealistic*, *photograph*, *hyper-detailed*, *8K ultra-sharp*, *cute*, *chibi*, *toy*, *neon*, and lists of random mechanical parts.

### Phase D: Write the `style` field

10. **Use the custom canonical style string, not a dropdown preset.** No preset in the Style menu matches this style:
    - The **3D › Architectural** preset contains *"photorealistic representation"*.
    - The **Photography › Cinematic** preset describes photography.
    - The **Illustration** presets (Cartoon, Anime, Children's Book) are cartoonish.

    Click the pencil toggle and enter the canonical string:

    ```text
    cinematic stylized 3D CGI, premium animated feature film look, realistic 3D geometry with selectively simplified forms, clean intentional silhouettes, subtly refined proportions, believable physical structure, smooth but detailed surfaces with subtle roughness variation, tangible identifiable materials, controlled micro-detail, restrained sophisticated color grading, mature documentary tone, stylized rather than photorealistic
    ```

11. **Keep the `style` string identical in every shot of the documentary.** Express variation by period, location or emotion in `prompt`, `lighting` and `colors`, never in `style`. Consistency comes from a shared rendering philosophy.

### Phase E: Configure the `camera` object

12. **Choose the angle and the distance from the narrative purpose.** These values are recommended starting points, chosen from the tool's menus:

    | Narrative purpose | `angle` | `distance` | `lens-mm` | `f-number` |
    |---|---|---|---|---|
    | Establish location and period | `eye level` or `slightly high` | `wide shot` | 24–35 | `f/8`–`f/11` |
    | Scale and presence of a machine or building | `low angle` or `slightly low` | `wide shot` or `medium full shot` | 24–35 | `f/5.6`–`f/8` |
    | Human-centred storytelling | `eye level` | `medium shot` or `medium close-up` | 35–50 | `f/2.8`–`f/4` |
    | Intimate emotional moment | `eye level` | `close-up` | 85 | `f/2.8`–`f/4` |
    | Technical or structural detail | `eye level` or `slightly high` | `macro detail` or `tight detail` | 100 | `f/5.6`–`f/8` |
    | Controlled overview of a layout | `bird's-eye` | `wide shot` | 35 | `f/8` |

    - Use a **Dutch angle**, **worm's-eye** or **dynamic angle** only when the narrative clearly calls for it. Unmotivated angles are excluded.
    - Avoid **14mm** and **fisheye**, which cause excessive perspective distortion.
    - Avoid **tilt-shift**, which tends to produce a miniature, toy-like look.

13. **Set the depth of field selectively.** The dominant subject must stay readable, and background separation must build depth without hiding context.
    - For scientific or technological scenes, use the deeper end of each range.
    - Avoid `f/1.2`–`f/1.4`, which can blur away the context the documentary needs.

14. **Write the `focus` field as a visual hierarchy statement**, for example: *"Sharp focus on the foreman and the loom frame, background looms softly separated but readable."*

15. **Leave ISO empty** (the key is then omitted). ISO is a photographic sensor setting with no direct meaning in a CG render, and the tool labels high values *grainy*. If your pipeline requires the key, use `100`.

16. **Respect how the lens is parsed.** With the numeric toggle on, only the first number is kept:
    - `100mm macro` becomes `100`, so state *macro* in `distance` instead (`macro detail`).
    - `24-70mm` becomes `24`.

17. **Do not use motion shots** (`tracking shot`, `action shot`, `full body action`) for still frames. Camera movement belongs to the video stage, where it must be narrative-motivated: slow push-ins, subtle tracking, controlled pans.

### Phase F: Handle the `film_stock` field

18. **Leave Camera / Film Stock empty.** Every entry in that menu names a real camera body or film stock, for example *"shot on Sony A7 IV…"* or *"Cinestill 800T…halation effect"*. These push the output toward live-action photographic imitation, which the style excludes. An empty field omits `film_stock` from the JSON entirely.

### Phase G: Write the `lighting` field

19. **Describe a designed lighting setup** that has all of these:
    - a strong directional key light, with its source and direction;
    - a soft but defined fill;
    - a subtle rim light that separates the subject from the background;
    - atmospheric or volumetric depth when appropriate;
    - controlled highlights and shadow transitions.

    Aim the key light at the dominant subject.

    Use these templates according to the decision in Step 2:
    - **Dramatic:** *"strong directional key light from [source] camera [left/right], soft but defined fill, subtle rim light separating the subject from the background, pronounced but detailed shadows, controlled highlights, atmospheric volumetric depth"*
    - **Scientific / technological:** *"clean directional key light, soft even fill, subtle rim light for edge definition, controlled reflections on metal and glass, clearly visible structural detail, gentle shadow transitions"*

    The Lighting dropdown can serve as a starting point, but edit its text with the pencil toggle (see Section 5.1).

### Phase H: Define the `colors` object

20. **Build the palette from the base palette:**
    - muted earth tones;
    - warm amber and bronze highlights;
    - cool blue and teal shadows;
    - neutral grays;
    - desaturated industrial colors;
    - one carefully controlled accent.

    Enter each color as a **descriptive name** in the chip text box (for example `muted umber`, `aged bronze`, `cool slate-blue shadow`), or as a hex code. The style guide gives no hex values, so any hex value is your own art-direction choice.

21. **Adapt the palette to the subject and the period.** An industrial-revolution workshop and a contemporary military laboratory need different colors under the same grading philosophy. Do not default to generic orange-and-teal when it does not serve the subject. Keep saturation rich but controlled, and avoid neon.

22. **Optional: sample colors from an approved frame.** Drop an already-approved key frame from the documentary into the image drop zone and click the frame to add hex colors to the palette. This keeps palettes consistent across scenes. Review the sampled values and remove any that are oversaturated.

23. **Write the `mood` field** with a tone from the style guide, for example:
    - mysterious and investigative;
    - tense and dramatic;
    - reflective and melancholic;
    - technological and futuristic;
    - industrial and mechanical;
    - historical and atmospheric;
    - scientific and exploratory.

    Add *"mature, emotionally credible documentary tone"*.

### Phase I: Write the `composition` field

24. **Describe the frame structure explicitly.** Use a combination of these principles:
    - rule of thirds;
    - leading lines;
    - foreground, middle-ground and background separation;
    - negative space;
    - balanced visual weight;
    - controlled symmetry or asymmetry;
    - clear subject-background separation.

    Always end with *"single dominant focal point, uncluttered background"*.

    Suitable dropdown starting points: Rule of Thirds, Leading Lines, Foreground Interest, Deep Depth, Negative Space, Frame Within Frame, Centered / Symmetrical, Environmental Portrait.

### Phase J: Export and maintain consistency

25. **Copy the JSON.** Click **Copy to Clipboard**. The button shows **"Copied!"** for two seconds when the copy succeeds. If the label does not change, select the text in the JSON Output panel and copy it by hand.

26. **Archive the prompt** with the shot number, the narrative idea and the generated image, so later shots can reuse its values.

27. **Hold the constants fixed across shots:** the `style` string, the character blocks, the palette family, the grading philosophy and the lighting characteristics. Vary only what the period, location or emotion requires.

### 3.1 Reference examples

**Example A: dramatic, historical** (mid-19th-century textile mill):

```json
{
  "prompt": "A weathered mill foreman with natural, subtly refined proportions stands beside a cast-iron power loom in a mid-19th-century English textile mill, one hand resting on the loom frame as he inspects a length of woven cotton with a calm, focused expression. He wears a linen shirt with rolled sleeves, a dark wool waistcoat and a flat cloth cap. Behind him, rows of looms recede into soft haze, driven by leather belts from an overhead line shaft. Cast iron reads as matte painted metal with soft edge wear, the floor as worn timber boards, the cloth as natural woven cotton. Fine cotton dust hangs in shafts of daylight from tall multi-paned windows.",
  "style": "cinematic stylized 3D CGI, premium animated feature film look, realistic 3D geometry with selectively simplified forms, clean intentional silhouettes, subtly refined proportions, believable physical structure, smooth but detailed surfaces with subtle roughness variation, tangible identifiable materials, controlled micro-detail, restrained sophisticated color grading, mature documentary tone, stylized rather than photorealistic",
  "camera": {
    "angle": "slightly low",
    "distance": "medium shot",
    "lens-mm": 35,
    "f-number": "f/4",
    "focus": "Sharp focus on the foreman and the loom frame, background looms softly separated but readable"
  },
  "lighting": "strong directional key light from tall mill windows camera left, soft but defined fill from bounced daylight, subtle rim light separating the foreman from the background, volumetric light shafts through cotton dust, pronounced but detailed shadows, controlled highlights",
  "colors": {
    "palette": ["muted umber", "warm amber highlight", "aged bronze", "cool slate-blue shadow", "neutral warm gray"],
    "mood": "historical and atmospheric, reflective and melancholic, mature emotionally credible documentary tone"
  },
  "composition": "rule of thirds composition, foreman on the left third, loom rows as leading lines toward the windows, clear foreground, middle-ground and background separation, single dominant focal point, uncluttered background"
}
```

**Example B: scientific / technological** (contemporary robotics laboratory):

```json
{
  "prompt": "A six-axis industrial robot arm with precise geometric housings and clearly readable joints, cable routing and end effector holds a machined aluminium component above a calibration fixture in a contemporary robotics laboratory. The arm's painted housings read as satin industrial enamel with controlled highlights, the joints as brushed aluminium, the fixture as anodised metal. A glass safety partition and a single engineer's workstation sit softly in the background, subordinate to the arm.",
  "style": "cinematic stylized 3D CGI, premium animated feature film look, realistic 3D geometry with selectively simplified forms, clean intentional silhouettes, subtly refined proportions, believable physical structure, smooth but detailed surfaces with subtle roughness variation, tangible identifiable materials, controlled micro-detail, restrained sophisticated color grading, mature documentary tone, stylized rather than photorealistic",
  "camera": {
    "angle": "eye level",
    "distance": "medium full shot",
    "lens-mm": 50,
    "f-number": "f/8",
    "focus": "Sharp focus across the full robot arm and fixture, background softly separated but legible"
  },
  "lighting": "clean directional key light from overhead camera right, soft even fill, subtle rim light for edge definition, controlled reflections on metal and glass, clearly visible structural detail, gentle shadow transitions",
  "colors": {
    "palette": ["neutral cool gray", "desaturated industrial orange", "brushed steel", "deep teal shadow", "soft warm white"],
    "mood": "scientific and exploratory, technological, calm and precise documentary tone"
  },
  "composition": "centered composition with controlled symmetry, robot arm as the single dominant focal point, negative space around the end effector, clear foreground, middle-ground and background separation, uncluttered background"
}
```

The period and technical details in these examples illustrate the format. Verify them against references before you use them in production.

## 4. Verification

**4.1 Structural check (JSON)**

- [ ] The JSON parses without errors.
- [ ] The keys appear in the tool's order: `prompt`, `style`, `camera`, (`film_stock`), `lighting`, `colors`, `composition`.
- [ ] `lens-mm` and `ISO` are integers when present. `f-number` is a string such as `"f/4"`.
- [ ] `colors.palette` is an array and contains neither the default `#2A5BDA` nor `amber glow`, unless you chose them on purpose.
- [ ] There are no empty strings ("Include empty fields" is off).
- [ ] `film_stock` is absent.
- [ ] `style` matches the constants document exactly.

**4.2 Style check (generated image)**

- [ ] It sits between realistic CGI and sophisticated animation, neither photorealistic nor cartoonish.
- [ ] Silhouettes are clean and readable, and forms feel intentionally designed.
- [ ] Materials are identifiable and tangible, without excessive grunge or noise.
- [ ] Lighting is directional, guides attention and suits the scene type.
- [ ] Color follows the grading philosophy and suits the subject and period.
- [ ] The camera choice is motivated by the narrative.
- [ ] The frame communicates one idea and has a single dominant focal point.
- [ ] Recurring characters match their established design.
- [ ] Historical and technical details are accurate.
- [ ] Emotional expression is controlled and credible.
- [ ] None of the characteristics listed in Section 5.3 is present.
- [ ] The image looks consistent with the rest of the documentary.

The result is **unsuccessful** if the image looks like an unrelated AI-generated image rather than a frame from the same cinematic universe.

## 5. Troubleshooting and Important Notes

### 5.1 Compatibility of built-in dropdown presets

| Field | Usable as a starting point | Edit before use | Avoid |
|---|---|---|---|
| Style | None (use the canonical string) | — | 3D › Architectural ("photorealistic"), Photography presets, Cartoon, Anime, Children's Book, Fantasy |
| Lighting | Three-Point, Rembrandt, Chiaroscuro, Low Key, Volumetric, Motivated, Practical | Backlit / Rim Light (remove "lens flare"), Warm and Cool Mix (orange-and-blue risk), High Key / Softbox (flat-light risk) | Neon Lights, RGB Lighting, Fairy Lights / Bokeh |
| Mood | Moody & Atmospheric, Mysterious, Melancholic, Serene, Cinematic | Industrial (remove "urban decay", "gritty"), Sci-Fi Futuristic (remove "cyberpunk"), Intense & Dramatic | Whimsical & Playful, Fantasy & Magical, Retro 80s, Joyful & Vibrant |
| Composition | Rule of Thirds, Leading Lines, Foreground Interest, Deep Depth, Negative Space, Frame Within Frame, Centered | Layered (keep one focal point) | Flat / 2D, Action / Motion |
| Camera / Film Stock | — | — | All entries (leave the field empty) |

### 5.2 Common corrections

| Condition in the output | Field to change | Corrective action |
|---|---|---|
| Looks like a photograph | `style`, `prompt`, `film_stock` | Confirm the canonical style string. Remove photographic terms and any camera or film text. Add simplified-form and refined-silhouette wording. |
| Toy-like or cartoonish | `prompt`, `camera` | Restore believable proportions and material identity. Remove tilt-shift and cute descriptors. |
| Plastic or overly glossy surfaces | `prompt` | Describe roughness variation, satin or matte finishes and controlled highlights. |
| Cluttered frame | `prompt`, `composition` | Remove supporting elements. Restate the single dominant focal point. |
| Flat lighting | `lighting` | Add a directional key with a stated source, a defined fill and a subtle rim light. |
| Oversaturated or neon colors | `colors` | Return to the base palette. Remove neon or vivid chips and the default chips. |
| Recurring character changed | `prompt` | Re-paste the exact character block from the constants document. |
| Wrong period detail | `prompt` | Correct it to the historical record. Stylization does not excuse inaccuracy. |
| Excessive perspective distortion | `camera` | Raise `lens-mm` to 24–50. Replace extreme angles with eye level or slightly low. |
| Subject unreadable because of blur | `camera` | Raise `f-number` to `f/4`–`f/8`. Restate the focus hierarchy. |

### 5.3 Characteristics to avoid

Photorealistic live-action imitation; generic stock-photo aesthetics; childish cartoon proportions; toy-like characters; plastic-looking materials; excessively glossy surfaces; flat, lifeless lighting; overly saturated colors; excessive sharpening; random mechanical complexity; visual clutter; generic fantasy aesthetics; inconsistent character designs; unmotivated camera angles; excessive visual effects such as bloom, artificial lens flares and overexposure; artificial cinematic clichés.

### 5.4 Tool behaviour warnings

- Loading a preset overwrites every field, including the palette.
- The screen eyedropper is disabled outside HTTPS or `localhost`. The image-based picker still works.
- Numeric lens parsing discards text after the first number.
- `film_stock` appears in the output whenever its field contains text, whatever the state of the empty-fields toggle.

## 6. Accuracy Review

- **Model behaviour unverified:** neither source states how FLUX 2 weighs each JSON key. The effect of photographic keys (`lens-mm`, `f-number`, `ISO`) on stylized 3D output, and the effect of positive-only phrasing, are reasonable interpretations that need testing.
- **Canonical style string:** this is an interpretation built from the style guide's stated characteristics. No preset in the tool supplies it.
- **Recommended camera values:** the ranges in Step 12 are interpretations of the guide's shot-type guidance. The guide specifies no focal lengths or f-numbers.
- **Missing quantitative values:** the guide gives no hex codes, saturation limits or contrast ratios. The palette names in the examples are illustrative.
- **Palette tension:** the base palette (amber highlights, teal shadows) resembles the orange-and-teal grading the guide warns against. Judge this boundary for each subject.
- **Still frames versus video:** the Prompt Builder produces still-image prompts. The guide's camera-movement rules apply only to a later video stage, which neither source describes.
- **Example details:** the historical and technical details in Section 3.1 are illustrative and must be checked against references.
- **Pipeline integration:** how the copied JSON is passed to FLUX 2 (for example, the ComfyUI node configuration) is not specified in either source.