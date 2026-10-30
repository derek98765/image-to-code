---
name: image-to-code
description: Build the frontend code from a provided reference UI image, using only the project's existing design tokens and components, then compare the result with the image and fix the differences. The reference can be a generated concept UI, a Mobbin screen, or a screenshot of an existing site. Use when the user says "code this design", "build this screen from the image", "turn this mockup into a page", "make it match the image", or hands over a UI image to implement. Never invents tokens, components or content. Illustrations and photos become placeholder boxes.
---

# image-to-code

Turn a reference UI image into code that looks like it, built from the project's
own tokens and components. The image is the only source of what to build. The
project is the only source of how to build it.

**Never add things or make up information that is not in the image.** No extra
sections, labels, badges, cards, copy, icons or states. If it is not in the
image, it is not in the page.

## Step 1 — Get the reference

- The user provides the image(s): a concept UI, a Mobbin reference, a screenshot
  of an existing site, or anything similar.
- No image → stop and ask. Do not draw one.
- Several images (a flow or a long page) → treat each as one screen or section
  and build them in the given order.
- Note the target: web or mobile, and the width the image was captured at. Build
  in the project's stack and routing. Ask only if the target is truly unclear.

## Step 2 — Study the image

Look at the image closely and write notes before touching code. Cover every tiny
detail. For each screen or section, record:

- **Layout:** grid and columns, alignment, section order, content width, image
  aspect ratios, what sits side by side and what stacks.
- **Text:** every string, word for word. Where text is unreadable or garbled,
  mark it and ask in Step 3.
- **Type:** font family style (serif, sans, mono), size steps, weights, line
  height, letter spacing, case, color per text level.
- **Color:** page background, surfaces, text levels, accent, borders, dividers,
  overlays, gradients. Estimate as hex.
- **Spacing:** gutters, section top and bottom space, gaps between elements,
  card padding, space between a heading and what follows.
- **Shape:** corner radius per element, border width and style, button and
  input heights.
- **Effects:** drop shadows (offset, blur, spread, opacity), inner shadows,
  blurs, overlays, opacity, hover or active looks if shown.
- **Components:** every repeated or recognizable part (nav, buttons, inputs,
  cards, tabs, badges, list rows, modals) and their variants.
- **Icons:** which icons appear, their size and stroke weight.
- **Images:** every illustration, photo, graphic or background image: position,
  size, aspect ratio, whether it is cropped or full-bleed.

Estimates are fine, since the next step maps them onto the project's scale.
Rough numbers that fit a scale beat exact guesses.

## Step 3 — Read the project and map the image onto it

First read what the project already has: design tokens (CSS variables, theme
file, Tailwind config), the type scale, spacing scale, radius, shadows, colors,
and the shared components and how they are used. Then for each thing in your
notes, find the **closest existing match**:

| In the image | Use |
|---|---|
| A color | The nearest existing color token |
| A font size, weight, line height | The nearest existing type token or text style |
| A gap, padding, margin | The nearest existing spacing token |
| A radius, border, shadow | The nearest existing token |
| A button, input, card, nav, etc. | The existing component and its closest variant |

Rules:

- **Do not invent new tokens or components.** Stay consistent with the project,
  even if the match is a little off the image.
- If a value has no close match, or a component does not exist, still build the
  screen using the nearest available option. Do not stop, do not hardcode a
  one-off value, and do not create a new token or component.
- Keep a running **flag list** of every case where you used a stand-in or where
  you think a new token or component is warranted. Each entry says what the
  image shows, what you used instead, and what you suggest adding.
- Prefer reusing an existing component over rebuilding it, even if the image
  shows slight differences. Compose with the project's own layout conventions.

Ask the user one batched question only for things you cannot resolve yourself:
unreadable text, a detail you cannot make out, or a target that is unclear. A
small, minor unknown does not need asking. Use your best read and note it in the
final report.

## Step 4 — Build

- Build in the image's order, section by section, using the project's tokens and
  components from the Step 3 mapping.
- Use the text from the image exactly. Keep confirmed corrections for garbled
  text.
- **Images and graphics:** do not recreate illustrations, photos, generated art
  or background images. Put a placeholder box where each one goes, at the same
  position, size and aspect ratio as in the image. Label it so it is obvious it
  needs replacing (for example, "Image: hero illustration"). Use the project's
  neutral surface color and an existing token, not decorative styling.
- Icons: use the project's icon set. If a needed icon is not in it, use the
  nearest one and add it to the flag list.
- Only build states and sizes that are shown in the image. If the image is
  desktop only, do not invent a mobile layout. Do not add hover states,
  empty states or extra copy that are not shown.
- No wrapper elements, boxes inside boxes or decorative additions that the image
  does not have.

## Step 5 — Compare with the image and adjust

Once the whole page is built:

1. Screenshot the result in the user's real Chrome with the `browser-screenshot`
   skill, at the same width as the reference image.
2. Put the screenshot and the reference image side by side. Go through the same
   checklist as Step 2: layout, text, type, color, spacing, shape, effects,
   components, icons, image placeholders.
3. List every visible difference, then fix them using the project's tokens and
   components. A difference that comes from a stand-in token is already on the
   flag list. Do not hack around it with a one-off value.
4. Screenshot again and recheck. Stop after 3 rounds. Report what still differs
   and why.

## Step 6 — Report and ask

End with a short report:

- **Built:** what was built and where (files, routes).
- **Remaining differences:** what still differs from the image and why.
- **Placeholders:** each image placeholder box that needs a real asset.
- **Flagged for approval:** the flag list of suggested new tokens or components
  or stand-ins. For each: what the image shows, what was used instead, and what
  you suggest adding. Ask the user which ones they approve.

Add nothing from the flag list until the user approves it. After approval, add
only the approved items, switch the affected code over to them, and re-run
Step 5 on the affected parts.
