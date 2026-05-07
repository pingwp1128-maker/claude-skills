---
name: Render Eval Skill
description: Evaluate rendered HTML/page visuals using screenshots.
---

Overview
This skill is for visual evaluation of rendered outputs, not just source inspection. It is most useful for HTML, rendered Markdown, DOCX, PDF, landing pages, newsletter pages, WeChat-style pages, dashboards, longform article pages, and other artifacts where the user cares about what the output actually looks like after rendering.

The core rule is simple:

do not claim to have done render-layer evaluation unless the page was actually rendered
do not score visual quality from source code alone if screenshots could have been obtained
This skill should often be used together with a writing or content evaluation skill, but it has a different job:

writing/content skills judge the text and task fulfillment
render-eval judges the visible result after rendering
Use the bundled screenshot script when possible:

screenshot capture workflow: screenshot-workflow.md
automation script: capture_page_screens.py
When To Use
Use this skill when the user asks to:

evaluate an HTML page visually
evaluate rendered Markdown output visually
evaluate DOCX layout, pagination, or document polish visually
evaluate PDF visual readability or page presentation
assess layout, typography, spacing, hierarchy, color, or overall polish
judge mobile readability or desktop readability
verify whether a page is publish-ready
review screenshots of a rendered page
distinguish between source-level quality and render-level quality
Do not use this skill as the primary evaluator when the task is only about text quality and no rendered view matters.

Inputs
Collect these inputs before evaluating:

the file path or URL to render
the original user request or intended page purpose
the expected audience and device priority if known
any constraints such as mobile-first, brand style, print mode, or no-image requirements
If the target cannot be rendered or screenshotted, state that limitation clearly and downgrade the review to source-level or structure-level only.

Supported Targets
local HTML files
local Markdown files after they are rendered in a viewer or converted to HTML
local DOCX files after they are opened in a document viewer or exported to PDF
page-like outputs viewable in a browser
screenshots supplied by the user
PDFs when they are being evaluated as rendered documents rather than only as text containers
Evaluation Modes
Always identify which of these modes the review is using:

Mode A. Fully Rendered Review
Use when you actually rendered the page and captured screenshots.

This is the preferred mode. It supports true render-layer evaluation.

Mode B. Screenshot-Only Review
Use when the user provided screenshots but you did not render the page yourself.

This supports visual evaluation, but not validation of responsiveness beyond the screenshots provided.

Mode C. Source-Only Fallback
Use only when rendering was not possible.

In this mode:

do not call the result a true render-layer evaluation
do not give full confidence on layout quality
explicitly say that visual conclusions are limited
Required Workflow
Follow these steps in order.

Step 0. Identify output type, purpose, and device priority
Before evaluating, determine:

output type
HTML / webpage
rendered Markdown
DOCX document
PDF document
screenshot set
page or document type
article page
marketing page
report/dashboard
documentation page
poster-like landing page
printable document
other
primary audience
consumers
partners / clients
internal readers
general public
technical audience
primary viewing device
mobile-first
desktop-first
both equally important
primary task
reading
scanning
persuasion
conversion
announcement
reporting
All later evaluation must be based on this framing.

Step 0.5. Choose the render path
Choose the correct render path before evaluating:

A. HTML / webpage
render directly in a browser
capture desktop and mobile screenshots when responsiveness matters
B. Markdown
if the user only cares about structural readability, a structure-level review may be enough
if the user cares about actual visual presentation, render the Markdown in its intended viewer or convert it to HTML first
do not judge final visual quality from raw .md source alone
C. DOCX
if the user cares about layout, pagination, or finish quality, open the document in a document viewer or export to PDF first
evaluate screenshots from the actual rendered document pages
do not treat extracted text as proof of good layout
D. PDF
treat the PDF itself as the rendered artifact
capture screenshots of representative pages or sections
when the PDF is long, review cover/first page, a representative body page, and an ending page at minimum
E. Screenshot-only input
evaluate what is visible in the supplied screenshots
clearly note what cannot be inferred because the underlying artifact was not rendered by you
Step 1. Render the target
If the environment supports rendering, actually open the target in its intended viewer.

When local automation is available, prefer using the bundled script to capture evidence first, then evaluate from the screenshots:

python3 scripts/capture_page_screens.py /path/to/page.html --output-dir /tmp/render-eval
For browser-rendered targets, at minimum, evaluate:

one desktop viewport
one mobile viewport
Recommended viewport examples:

desktop: around 1440x900
mobile: around 390x844
If the target is clearly desktop-only, mobile-only, or print-oriented, say so and adapt the check accordingly.

For non-browser targets:

Markdown: render in the target preview environment or convert to HTML first
DOCX: open in a document viewer or export to PDF before screenshotting
PDF: capture screenshots from representative pages
Step 2. Capture screenshots
Capture screenshots as evidence. Prefer at least:

beginning / first screen / first page
middle content area / representative body page
lower section / ending / last relevant page
If the target is very long, take enough screenshots to show the full reading rhythm or page progression.

If full-page capture is available, use it in addition to sectional screenshots, not instead of them.

Required rule:

if no screenshot evidence exists, do not claim strong confidence in visual conclusions
Step 3. Record the evaluation basis
State:

whether you rendered the page yourself
what viewports were checked
what screenshots were captured
whether any parts could not be validated
Step 4. Evaluate the rendered result
Judge the rendered output based on what is visible in the screenshots, not only what the source suggests.

Always cover these fixed visual dimensions:

1. 总体排版合理性
Check:

whether the overall layout feels orderly and easy to follow
whether spacing, alignment, paragraph length, and section division feel reasonable
whether the page or document looks balanced rather than crowded, broken, or awkward
whether the reading path is natural without obvious layout friction
2. 整体观感与成品度
Check:

whether the rendered result looks polished enough to inspire trust
whether the design feels visually clean, coherent, intentional, and aesthetically pleasing enough for the task
whether the page or document feels like a finished deliverable rather than a draft or rough export
whether the overall appearance is pleasant, attractive, and appropriate enough for its actual use case
3. 展示完整性与异常检查
Check:

whether images, charts, tables, cards, and text blocks display normally
whether any content is clipped, overlapping, misaligned, stretched, or hidden
whether line wrapping, scroll areas, and page boundaries behave normally
whether there are broken modules, placeholder areas, or obviously failed rendering states
4. 跨设备或跨页面稳定性
Check:

whether the output remains stable across the tested mobile and desktop views when both matter
whether a print-oriented document remains stable across representative pages
whether there are device-specific failures such as overflow, ugly wrapping, or broken spacing
whether the beginning, middle, and ending maintain a consistent presentation quality
Step 4.5. Evaluate dynamic visual dimensions when triggered
Do not force these into every review. Activate them only when the query or target clearly makes them relevant.

文体与呈现匹配度
Trigger when:

the user explicitly requested a genre or document form
for example: WeChat article, news release page, formal report, poster-like page, brand promo page, speech handout
Check:

whether the rendered result visually feels like the requested genre
whether the presentation supports the expected reading behavior for that genre
whether the output looks mismatched, such as content that reads like a WeChat post but renders like a generic webpage
whether the visual tone is too casual, too promotional, or too plain for the stated task
whether the aesthetic style feels suitable for the requested genre rather than merely functional
图表与媒体呈现有效性
Trigger when:

the rendered output contains charts, tables, images, or media blocks
or the user explicitly cares about chart or figure presentation
Check:

whether charts, images, and tables render fully and clearly
whether legends, labels, or captions are cut off
whether media blocks are stretched, clipped, overlapping, or unreadable
whether chart/table/image presence improves the page rather than causing display problems
Step 5. Run visual red-flag checks
Always check for these:

clipped or hidden content
overlapping text, images, tables, or charts
broken chart, image, or table display
ugly wrapping that hurts reading
inconsistent alignment
cramped content or excessive blank areas
overflow in mobile or desktop view
placeholder or failed-render areas
abrupt ending, broken footer, or unfinished-looking sections
Step 6. Separate confirmed findings from source-based inference
If a conclusion comes from a screenshot, treat it as confirmed.

If a conclusion comes from source structure because rendering was not available, label it as limited-confidence inference.

Do not mix these two types of evidence.

Source-Level Companion Checks
When source is available, include a short supporting check, but keep it secondary to screenshot evidence.

For HTML, inspect:

heading structure
semantic grouping
responsiveness rules
image containers
overflow risks
extreme fixed dimensions
suspicious spacing hacks
For Markdown, inspect:

heading hierarchy
list and table structure
quote/code block boundaries
whether the source structure can plausibly render cleanly
For DOCX, inspect when possible:

heading style consistency
paragraph and list style usage
table/image anchoring risks
whether the file appears to rely on manual spacing hacks
For PDF, source-level inspection is usually not the focus. Treat the PDF itself as the rendered object unless the user specifically wants internal structure diagnosis.

Source inspection can explain why a visual issue exists, but it should not replace screenshot review.

Resource Notes
scripts/capture_page_screens.py
Use this script to capture desktop and mobile screenshots for local HTML files or URLs.

Expected behavior:

captures desktop and mobile screenshots
captures top, middle, and bottom sections when possible
writes files into an output directory for later visual review
If Safari automation is unavailable, the script should fail with a clear message instead of pretending screenshots were captured.

references/screenshot-workflow.md
Read this file when:

Safari automation needs to be enabled
you need the expected screenshot set
you need a standard review basis for desktop/mobile visual inspection
Output Format
Return the evaluation in Markdown using this structure.

# Render Eval Report

## Review Mode

- Mode: Fully Rendered Review / Screenshot-Only Review / Source-Only Fallback
- Target:
- Page type:
- Audience:
- Device priority:
- Purpose:

## Render Basis

- Rendered by evaluator: Yes / No
- Desktop viewport checked:
- Mobile viewport checked:
- Screenshots reviewed:
- Validation limits:

## Overall Verdict

- Overall rating:
- Publish readiness:
- Biggest visual issue:
- Strongest visual strength:

## Visual Scorecard

| 维度 | 得分 | 评级 | 说明 |
|------|------|------|------|
| 总体排版合理性 | 8/10 | 良好 | ... |
| 整体观感与成品度 | 7/10 | 良好 | ... |
| 展示完整性与异常检查 | 8/10 | 良好 | ... |
| 跨设备或跨页面稳定性 | 7/10 | 良好 | ... |

**综合结论：良好**

## Dynamic Visual Dimensions

仅展示被触发的专项维度。

| 动态维度 | 是否触发 | 得分/结论 | 说明 |
|----------|----------|-----------|------|
| 文体与呈现匹配度 | 是 | 8/10 | 呈现形式基本符合公众号文章页 |
| 图表与媒体呈现有效性 | 否 | N/A | 无图表或图片 |

## Screenshot Findings

### 开头区域
- ...

### 中段区域
- ...

### 结尾区域
- ...

## Visual Red Flags

- ...

## Source-Level Notes

仅在有源码且值得补充时展示。

- ...

## Dynamic Visual Notes

仅在触发动态专项维度时展示。

### 文体与呈现匹配度
- ...

### 图表与媒体呈现有效性
- ...

## Priority Fixes

1. ...
2. ...
3. ...

## Confidence

- Visual confidence: High / Medium / Low
- Why:
Scoring Rules
优秀: 9-10
良好: 7-8
合格: 5-6
需改进: 1-4
Do not compute a fake-precise weighted average unless the task specifically needs it. For most render reviews, the qualitative verdict plus dimension scores is enough.

Evaluation Style
Be concrete and visual.
Refer to what is actually seen in screenshots.
Prefer comments like "the mobile title wraps awkwardly on the second line" over abstract praise or criticism.
Do not overfocus on aesthetics while ignoring readability.
Prioritize user-facing impact over minor polish.
If the page was not rendered, say so early and clearly.
Hard Rules
No screenshot evidence, no high-confidence render-layer claim.
A CSS file is not proof of visual quality.
Responsive rules are not proof of good mobile experience.
If desktop and mobile impressions differ, report both.
If the page looks good in structure but weak in screenshots, trust the screenshots.