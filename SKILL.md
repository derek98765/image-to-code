---
name: image-to-code
description: Build a website or app screen in code so it matches design images the user has approved, then screenshot the result and fix the differences. Reads design/<name>/approved/ (from generate-concept-ui) or any images the user hands over — Figma exports, screenshots, other tools. Use when the user says "code this design", "build these approved images", "turn this mockup into a page", or "make it match the image". It does not invent designs or draw new ones.
---

# image-to-code

You turn approved design images into code that looks the same. The images are the
spec. Your job is to copy them faithfully, not to improve them.

## The 5 mistakes to avoid

These are the most common ways code drifts from the image. Check for them at every
step.

1. **Drifting into a generic template.** Distinctive sections become plain rows,
   strong type becomes default headings. Keep what makes the design look the way it
   does.
2. **Tighter spacing than the image.** Code tends to shrink gaps and padding. Match
   the image's space, even when it feels generous.
3. **Adding things that are not in the image.** No extra pills, labels, badges,
   cards, wrappers or sections. No boxes inside boxes.
4. **Guessing instead of asking.** When the image is unclear, ask. Do not fill the
   gap with a default.
5. **Not checking the result.** Code is not done until a screenshot has been
   compared with the image.

## Step 1 — Find the inputs

- Look for `design/<name>/approved/` and `approved/design-spec.md`.
- If the user gave images some other way, use those.
- No approved images → stop and ask. Do not draw your own; drawing is
  `generate-concept-ui`'s job and the user must approve first.
- No `design-spec.md` → write one from the images (palette with hex, fonts, type
  sizes, spacing, radius, buttons, section order) and show it to the user before
  coding.

Also read the project: its stack, existing tokens, shared components and styling
approach. Code goes in the project's stack. For mobile images, use the project's
mobile stack; if there is none, ask whether to build responsive web or something
else.

## Step 2 — Study each image

Treat each image as a spec, not a mood. For every image write short notes:

- **Text:** headline, subheadline, buttons, section titles, nav, labels — word for
  word where readable.
- **Type:** size steps, weights, line count, line height, tracking, serif or sans,
  display vs body.
- **Spacing:** headline to subheadline, text to buttons, card gaps, card padding,
  side gutters, section top and bottom space.
- **Buttons and parts:** size, shape, radius, fill or outline, icons, primary vs
  secondary, borders, dividers, shadows, inputs.
- **Color:** background, surfaces, text levels, accent, borders, image tint,
  gradients — as hex.
- **Layout:** grid, columns, alignment, image ratios, section order.
- **Unclear:** anything you cannot read or measure.

The goal is the right spacing and type logic, not exact pixel guesses. Map what you
see onto a consistent scale.

## Step 3 — Clear up what is unclear

Collect every unclear point and ask the user in one message. Common ones:

- **Garbled text.** Image models mangle text. Propose the copy you think was meant
  and let the user confirm.
- **A detail too small to read.** Ask whether to request a closer image from
  `generate-concept-ui`. Never crop and zoom the approved image.
- **Images inside the design** (photos, illustrations). Ask whether to make them
  with `image-gen`, use the user's own assets, or use placeholders for now.
- **Missing states and sizes:** hover, menus, empty states, tablet and phone widths.
  Propose a choice that fits the design and say what you decided.

If something is minor and the user has said to go ahead, resolve it in this order:
keep the visible design language → keep the layout and spacing logic → keep the
component family → pick the simplest faithful version. Never fall back to a generic
default.

## Step 4 — Build

- Build section by section, in the image's order.
- Use the project's existing tokens and components when they match the image. When
  they do not, match the image and tell the user what differs from the project.
- Put the spec values (colors, type scale, spacing, radius) in one place, such as
  CSS variables or the theme file, not scattered literals.
- Use the confirmed copy, not the garbled image text.
- For screen sizes without an approved image, keep the same design language and use
  the `responsive-design` skill's approach.

## Step 5 — Compare and fix

1. Screenshot each section at the image's width with the `browser-screenshot` skill
   (desktop 1920 for web images, mobile 393 for phone screens).
2. Put the screenshot and the image side by side and list the differences: layout,
   spacing, type size and weight, color, radius, missing or extra elements.
3. Fix them and screenshot again.
4. Stop after 3 rounds. Report what still differs and why (for example, a
   browser font that cannot match exactly).

## Done

Tell the user:
- what was built and where,
- the remaining differences from the images,
- the decisions you made without an image (sizes, states, copy).
