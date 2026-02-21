# cat-photo-clarity-score

A scalar function that answers one objective question about a cat photograph: **can you clearly see the cat?**

## Overview

`cat-photo-clarity-score` evaluates the technical visual clarity of a single cat photograph and returns a numerical score between 0 and 1. It measures whether the photograph succeeded as a clear visual record of its subject — not whether the photo is beautiful, well-composed, or emotionally compelling. A score of 1 represents a photograph where the cat is plainly and cleanly visible; a score of 0 represents a photograph where technical failures have rendered the cat difficult or impossible to see.

## Input

The function accepts a single required input:

| Field | Type | Required | Description |
|---|---|---|---|
| `cat_photo` | `image` | Yes | A photograph of a cat to be evaluated for technical visual clarity. |

The image can be provided as a URL or base64-encoded data. The function expects the image to contain a cat — it evaluates how clearly the cat is rendered, not whether a cat is present.

## Output

A scalar value in the range **[0, 1]**:

- **0.8 – 1.0**: Excellent clarity. The cat is well-lit, sharply focused, and the image is clean. Form, features, and fine details like fur texture are all easy to see.
- **0.5 – 0.8**: Moderate clarity. The cat is visible but one or more technical qualities are lacking — perhaps slightly underexposed, a touch soft, or showing some grain.
- **0.2 – 0.5**: Poor clarity. The cat is hard to see clearly due to significant issues with lighting, focus, or image degradation.
- **0.0 – 0.2**: Very poor clarity. The cat is largely obscured — lost in shadow, severely blurred, or buried under heavy noise and artifacts.

## What It Evaluates

The function assesses three fundamental qualities, each representing a distinct way a photograph can fail to clearly render its subject:

### 1. Lighting

Is the cat adequately illuminated? The function checks whether the cat's form, features, and textures are visible under the available light. Both underexposure (cat lost in shadow) and overexposure (cat washed out in glare) are penalized. The standard is adequacy, not artistry — a harsh fluorescent kitchen light is fine as long as the cat is plainly visible.

### 2. Sharpness and Focus

Is the cat itself crisp and in focus? The function evaluates whether edges are well-defined, whether fine details like individual hairs and eye definition are resolved, and whether the camera's focal plane landed on the cat rather than the background or foreground. Motion blur — common in cat photography due to the subject's unpredictable movement — is also detected and penalized.

### 3. Freedom from Noise and Artifacts

Is the image clean? Even a well-lit, sharply focused photograph can fail if it is coated in digital grain from high ISO capture or degraded by compression artifacts such as blockiness, color banding, or smeared details. The function checks that these technical imperfections do not obscure the cat.

## What It Does NOT Evaluate

This function deliberately excludes:

- **Composition** — rule of thirds, framing, negative space
- **Aesthetics** — color grading, warmth, mood, artistic style
- **Subject appeal** — cuteness, pose, expression
- **Emotional impact** — whether the photo is charming, funny, or moving
- **Cat detection** — whether a cat is present at all

This narrow scope is intentional. Technical clarity is the foundation upon which all other visual judgments rest. By isolating it, the function remains composable — it can be paired with other evaluations (aesthetics, composition, subject quality) without conflating concerns.

## Use Cases

- **Adoption platforms**: Automatically filter out low-quality uploads so every listed cat is presented in a photo where it can actually be seen.
- **Social media apps**: Help users select the clearest shot from a burst of cat photos.
- **Veterinary telemedicine**: Screen submitted images before they reach a veterinarian, flagging photos too unclear for reliable visual assessment.
- **ML dataset curation**: Preprocess training data for cat-related models, filtering out images with insufficient clarity to be useful for training.
- **Photo management**: Batch-sort large collections of cat photos, surfacing the clear ones and flagging the dark, blurry, or grainy ones for deletion.

## Example Interpretations

| Scenario | Expected Score |
|---|---|
| Cat sitting in a sunlit window, sharp focus, clean image | High (~0.9) |
| Cat in a dim room, slightly soft focus, minor grain | Moderate (~0.5) |
| Cat mid-leap, heavy motion blur, underexposed | Low (~0.2) |
| Near-black frame, cat barely visible as a dark shape | Very low (~0.05) |