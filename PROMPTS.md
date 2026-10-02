# PROMPTS.md

All prompts were typed to Claude (Claude Sonnet 5.5) in claude.ai, in this order. Where I pasted a whole file or the assessment text, I show it in square brackets instead of repeating it. The full chat is in the link I shared. All image work was also done by Claude, which wrote Python code to draw the images (no image generator).

## Prompt 1
- Tool: Claude (claude.ai)
- Type: other
- Prompt:
  > [pasted the full assessment brief, "Developer Assessment: Rescue the Mistvale tea store"]
  >
  > can you explain this assessment in simple term so that i can understand and confidently start
- Outcome: accepted
- Why: The plain-language summary and start plan were clear and matched the brief.

## Prompt 2
- Tool: Claude (claude.ai)
- Type: other
- Prompt:
  > [pasted the full BRAND.md, "Mistvale Tea Co. brand and design rules", with no extra instruction]
- Outcome: accepted
- Why: I sent the brand rules so the answers would follow them. The reply turned them into a checklist and a list of likely traps.

## Prompt 3
- Tool: Claude (claude.ai)
- Type: other
- Prompt:
  > [pasted the full original index.html, with no extra instruction]
- Outcome: accepted
- Why: Claude gave a bug checklist grouped by business rule (R1 to R8), security, fake claims, design and accessibility. I used it as my test list.

## Prompt 4
- Tool: Claude (claude.ai)
- Type: code
- Prompt:
  > now i think you must have understood what we have to do in the assessment so keep the rules in mind from brand.md and readme.md and do the changes accordingly and give me the full code of the index.html . Things need to follow accordingly;
  > 1 - keep the themes, fonts, colors as per the brand.md
  > 2-check the 6 tasks in the readme.md and do the bug fixing accordingly
  > 3-enhance the design but do it in the rules as mentioned
  > 4 - keep this rules in mind
  > 5-keep this thing in mind
  > Design rules at a glance
  > [pasted the design rules table: colours, fonts, text sizes, spacing, product grid, buttons, corners, icons, forms, motion]
  >
  > 6-think about this creation and do accordingly
  > Things to watch (likely traps)
  > [pasted the list of eight traps: ratings, unprovable claims, section order, one primary button, reviews, shipping threshold, hero overlay, uppercase text]
  >
  > A good way to use this in your AI prompts
  > [pasted the advice about starting design prompts with the rules]
  >
  > Now do the analyzing of all the things and give the code accordingly.
- Outcome: [set after you test: accepted | modified]
- Why: [FILL IN: what was good or wrong, and what you changed after testing. The first version came with a note that the code was not run in a browser.]

## Prompt 5
- Tool: Claude (claude.ai)
- Type: image
- Prompt:
  > now tell me how to create images for this page and how to use it
- Outcome: accepted
- Why: Gave a file list, one shared style block for consistency, a prompt per product, and resize and compress steps. I did not use image-generator prompts for the final files (see Prompt 6).

## Prompt 6
- Tool: Claude (claude.ai)
- Type: image
- Prompt:
  > just give me all the images while following the rules and the specification provided with the names as mentioned in the code of index.html
- Outcome: [set after you review the images: accepted | modified | rejected]
- Why: Claude cannot run an image generator, so it drew the images with Python code and exported WebP files. The first run had a shadow wrap bug, clipped leaves in the hero, and a logo in an italic font, which it fixed after viewing the previews. The result is illustrations, not photos. [FILL IN: your own view of the images.]

## Prompt 7
- Tool: Claude (claude.ai)
- Type: code
- Prompt:
  > fix this [pasted a VS Code CSS warning: code "vendorPrefix", "Also define the standard property 'line-clamp' for compatibility", line 97]
- Outcome: accepted
- Why: The warning was correct. Claude added `line-clamp:3` beside `-webkit-line-clamp:3` in the product description style.

## Prompt 8
- Tool: Claude (claude.ai)
- Type: other
- Prompt:
  > now give me both `NOTES.md` and `PROMPTS.md`
- Outcome: modified
- Why: Claude wrote both files with blank spots for the things only I can answer (testing, hours, outcomes). I will fill those in before submitting.
