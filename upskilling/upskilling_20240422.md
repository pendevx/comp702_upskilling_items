# Block Formatting Context (BFC)

A block formatting context is a part of the CSS Visual Formatting Model. It defines how block boxes are laid out in the normal flow, and also dictates some of the rules in float layouts.

BFCs are generated in many situations, but the most common of them are:
- Generated at the root element of the document `<html>`
- Floats
- Absolutely positioned elements (`position: absolute` or `position: fixed`)
- Inline blocks
- `display: flow-root`
- Flex items
- Grid items
- `overflow` set to anything other than `visible`

Within a BFC, each box's left outer edge touches the left edge of the containing block, regardless of the presence of floats, unless the box itself establishes a new BFC.

A BFC has the three following rules:
1. Contains internal floats
2. Excludes external floats
3. Suppresses margin collapsing

## Contains internal floats

BFCs generates containing blocks for its own internal floats. The rules of the internal floats follow the same rules as listed in the previous document.

## Excludes external floats

BFCs will ignore or run away from external floats. The size of a float placed horizontally to a new BFC will cause the BFC's size to shrink when the float is expanded.

## Suppresses margin collapsing

Adjacent vertical margins which span across two different BFCs will not cause the margins to collapse.

# Inline Formatting Context (IFC)

IFCs define the rule for the layout and alignment of inline boxes.

Only the horizontal margins, borders, and padding of inline boxes are respected. Vertical margins, borders, and padding are ignored.

IFCs generate many lines of content, where each line of content generates a *line box*. A line box is the layouting model of these individual lines, to which the content may be aligned in different ways, such as the bottoms or tops, or the baselines of the text. These values may be adjusted for all line boxes within an IFC using the `vertical-align` property. Line boxes are always tall enough to contain all of its boxes. 

Floats may impact the layouting of line boxes. Line boxes have the same "exclude external floats" behaviour of BFCs, which allow the content of line boxes to "wrap" around floating boxes, also meaning that floating boxes may reduce the space inside a line box.

When the total width of line boxes in an IFC is less than the width of the box containing them, the horizontal distribution of the content is determined by the `text-align` property. 

When an inline box exceeds the width of a line box, it will split into multiple line boxes. If the inline box cannot be split, then the inline box overflows the line box.
