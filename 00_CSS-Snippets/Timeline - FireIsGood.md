#type/CSS-Snippet #topic/timeline-fireisgood-modified #for/Obsidian

```css
/*
author: FireIsGood (adapted for single-callout by Alfredo + Claude)
source: https://discord.com/channels/686053708261228577/702656734631821413/1156868388249935883

USAGE — one callout, all entries inside:

> [!timeline-fig]
>
> # Siglo X
> ### Title here
> Content text, lists, images — anything.
> - bullet item
>
> # 2021
> ### Another entry
> More content here.

  H1        → date label, appears LEFT of the vertical line
  H3        → card title (styled in accent colour)
  H2/p/ul/ol → card body content
*/


/* ================================================================
   CSS VARIABLES — adjust to taste
   ================================================================ */

body {
  --timeline-title-color:            var(--text-normal);
  --timeline-title-size:             var(--h2-size);
  --timeline-title-width:            150px;

  --timeline-line-color:             var(--color-base-35);
  --timeline-line-margin:            16px;
  --timeline-line-width:             4px;
  --timeline-line-style:             solid;

  --timeline-dot-color:              white;
  --timeline-dot-size:               28px;
  --timeline-dot-radius:             100%;
  --timeline-dot-border-color:       var(--background-primary);
  --timeline-dot-border-size:        5px;

  --timeline-card-margin:            24px;
  --timeline-card-background-color:  var(--color-base-30);
  --timeline-card-padding-x:         16px;
  --timeline-card-padding-y:         12px;
  --timeline-card-radius:            12px;

  /* Total left space = date column + gap + line + gap */
  --timeline-offset: calc(
    var(--timeline-title-width) +
    var(--timeline-line-margin) +
    var(--timeline-line-width)  +
    var(--timeline-line-margin)
  );
}


/* ================================================================
   CALLOUT CONTAINER
   ================================================================ */

[data-callout="timeline-fig"] {
  padding:           0 0 1rem 0;
  margin:            0;
  mix-blend-mode:    normal;
  background-color:  unset;
  border:            none;

  /* No SVG icon */
  .svg-icon { display: none; }

  /* Callout title bar is hidden — H1s inside content replace it */
  .callout-title { display: none; }

  .callout-content {
    position:       relative;
    padding-left:   var(--timeline-offset);
    padding-top:    0;
    padding-bottom: 0;
    background:     none;
  }
}


/* ================================================================
   VERTICAL LINE — continuous, spans full content height
   ================================================================ */

[data-callout="timeline-fig"] .callout-content::before {
  content:           "";
  position:          absolute;
  left:              calc(var(--timeline-title-width) + var(--timeline-line-margin));
  top:               0;
  bottom:            0;
  width:             var(--timeline-line-width);
  background-color:  var(--timeline-line-color);
}


/* ================================================================
   H1 = DATE LABEL (pulled into the left column)
   ================================================================ */

[data-callout="timeline-fig"] .callout-content > h1 {
  position:      relative;
  /* Pull left to sit in the date column */
  left:          calc(-1 * var(--timeline-offset));
  width:         var(--timeline-title-width);
  text-align:    right;
  padding:       0 var(--timeline-line-margin) 0 0;
  margin-top:    var(--timeline-card-margin);
  margin-bottom: 0;
  border:        none;

  color:         var(--timeline-title-color) !important;
  font-size:     var(--timeline-title-size)  !important;
  font-weight:   bold                        !important;
}

/* Less top margin on the very first entry */
[data-callout="timeline-fig"] .callout-content > h1:first-child {
  margin-top: var(--timeline-card-padding-y);
}


/* ================================================================
   DOT — one per H1, centred on the vertical line
   ================================================================ */

[data-callout="timeline-fig"] .callout-content > h1::after {
  content:          "";
  position:         absolute;
  /* Centre on the line: title-width + margin + half-line-width − half-dot-size */
  left: calc(
    var(--timeline-title-width)   +
    var(--timeline-line-margin)   +
    var(--timeline-line-width) / 2 -
    var(--timeline-dot-size)   / 2
  );
  top:              50%;
  transform:        translateY(-50%);
  width:            var(--timeline-dot-size);
  height:           var(--timeline-dot-size);
  border-radius:    var(--timeline-dot-radius);
  background-color: var(--timeline-dot-color);
  border:           var(--timeline-dot-border-size) solid var(--timeline-dot-border-color);
  box-sizing:       border-box;
  z-index:          2;
}


/* ================================================================
   CARD CONTENT — H2, H3, p, ul, ol
   All elements share the card background; radius is applied
   to the first and last elements of each group.
   ================================================================ */

[data-callout="timeline-fig"] .callout-content > :is(h2, h3, p, ul, ol) {
  background-color: var(--timeline-card-background-color);
  margin-top:       0;
  margin-bottom:    0;
  padding-top:      0;
  padding-bottom:   0;
  padding-left:     var(--timeline-card-padding-x);
  padding-right:    var(--timeline-card-padding-x);
}

/* Top of card — first element after an H1 */
[data-callout="timeline-fig"] .callout-content > h1 + :is(h2, h3, p, ul, ol) {
  border-top-left-radius:  var(--timeline-card-radius);
  border-top-right-radius: var(--timeline-card-radius);
  padding-top:             var(--timeline-card-padding-y);
  margin-top:               -1.25em;
}

/* Bottom of card — last element before next H1, or last child */
[data-callout="timeline-fig"] .callout-content > :is(h2, h3, p, ul, ol):has(+ h1),
[data-callout="timeline-fig"] .callout-content > :is(h2, h3, p, ul, ol):last-child {
  border-bottom-left-radius:  var(--timeline-card-radius);
  border-bottom-right-radius: var(--timeline-card-radius);
  padding-bottom:             var(--timeline-card-padding-y);
  margin-bottom:              var(--timeline-card-margin);
}

/* Single-element card (only one element between two H1s) */
[data-callout="timeline-fig"] .callout-content > h1 + :is(h2, h3, p, ul, ol):has(+ h1),
[data-callout="timeline-fig"] .callout-content > h1 + :is(h2, h3, p, ul, ol):last-child {
  border-radius:  var(--timeline-card-radius);
  padding-top:    var(--timeline-card-padding-y);
  padding-bottom: var(--timeline-card-padding-y);
  margin-bottom:  var(--timeline-card-margin);
}

/* H3 — card title, accent colour */
[data-callout="timeline-fig"] .callout-content > h3 {
  color:       var(--interactive-accent) !important;
  font-size:   var(--font-text-size)     !important;
  font-weight: 700                       !important;
}

/* Lists — restore bullet indentation inside the card */
[data-callout="timeline-fig"] .callout-content > :is(ul, ol) {
  padding-left: calc(var(--timeline-card-padding-x) + 1.5rem);
}


/* ================================================================
   OPTIONAL DOT COLOURS
   Apply to the whole timeline: > [!timeline-fig|blue]
   ================================================================ */

[data-callout="timeline-fig"][data-callout-metadata="red"]    { --timeline-dot-color: var(--color-red);    }
[data-callout="timeline-fig"][data-callout-metadata="orange"] { --timeline-dot-color: var(--color-orange); }
[data-callout="timeline-fig"][data-callout-metadata="yellow"] { --timeline-dot-color: var(--color-yellow); }
[data-callout="timeline-fig"][data-callout-metadata="green"]  { --timeline-dot-color: var(--color-green);  }
[data-callout="timeline-fig"][data-callout-metadata="cyan"]   { --timeline-dot-color: var(--color-cyan);   }
[data-callout="timeline-fig"][data-callout-metadata="blue"]   { --timeline-dot-color: var(--color-blue);   }
[data-callout="timeline-fig"][data-callout-metadata="purple"] { --timeline-dot-color: var(--color-purple); }
[data-callout="timeline-fig"][data-callout-metadata="pink"]   { --timeline-dot-color: var(--color-pink);   }
```
