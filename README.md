# obsidian Timelines
Generate a chronological timeline of all notes with the specified set of tags. 

## Example - Vertical Timeline
![example](https://raw.githubusercontent.com/Darakah/obsidian-timelines/main/images/exp_2.png)

## Example - Horizantal Timeline
![example](https://raw.githubusercontent.com/Darakah/obsidian-timelines/main/images/TimelineVis.png)

## Inserting a Timeline Event

VIDEO BRIEF EXAMPLE: https://www.youtube.com/watch?v=_gtpZDXWcrM

1. Add the `timeline` tag to the note, either in the YAML frontmatter or somewhere else (a note without a timeline tag is ignored when building a timeline).
2. or a timeline HTML comment for static rendering.

### Using a Timeline code block for dynamic rendering

Create the following code block where a timeline is to be inserted using: `timeline` or `timeline-vis`

![example](https://raw.githubusercontent.com/Darakah/obsidian-timelines/main/images/example_1.png)

the render block takes a single input which is the list of tags by which to filter timeline tagged notes (e.g. in the above example block, ONLY notes with all three tags `timeline`, `test` and `now`). this can be used to filter battles only with a note tagged `timeline` and `war` for example or birthdays using `timeline` and `birthday` for the single input.

### Using a Timeline inline code block for static rendering

Using an HTML code block, insert the following HTML comment where a statically rendered timeline should be inserted:
```
<!--TIMELINE BEGIN tags='test;now'--><!--TIMELINE END-->
```
(this is rendering notes that have both tags: test, now appiled to it)

Use the Timelines: Render Timeline command to generate a timeline statically. Running the command again will replace everything between the two comments (BEGIN/END) with a freshly rendered timeline. Changes will not be detected using this method, but as it is creating static HTML, the generated content will work and readable without Obsidian.

## Timeline Tagging a note

The note will be ignored in the following cases:
- no `timeline` tag (or the modified tag from the settings)
- Note does not contain all the tags from the search list
- No timeline `<span>` or `<div>` info block, or frontmatter(YAML) to pull from
- The used date is not VALID, must be in the following format YEAR-MONTH-DAY-MINUTES (check info section below for more details)

## Timeline Event Properties

Timeline events must specify the following: 
- a valid date, YEAR-MONTH-DAY-MINUTES (check info section below for more details)
- a valid class, specifically `ob-timelines` must be specified.
All other fields are optional.
Invalid timeline events will be skipped.

### Timeline Entry

There are 2 ways to register a note with the timeline: Frontmatter or HTML tags. If HTML tags are included in the note,
they will be used. Otherwise the timeline with default to gathering data from the frontmatter.

#### Method 1: Frontmatter

```markdown
---
start-date: 2018-01-18
end-date: 2018-03-02
type: range
color: blue(defalult if not provided) 
tags: [timeline, personal]
title: My Note Title(otherwise uses the title of the note by default)
---
```
![example](https://raw.githubusercontent.com/ReconVirus/obsidian-timelines/main/images/Colors.png)
Color supports `green`, `gray`, `orange`, `pink`, `purple`, `yellow`, `white`  and is blue by default if color is not provided in the frontmatter.
You can add any other colors you want in a CSS snippet with the following example rule:

```CSS
.vis-item.purple {
  background-color: rgb(139, 195, 74);
  filter: none;
  border-color: #628934;
}
.vis-item.vis-selected.purple {
  /* custom colors for selected orange items */
  background-color: rgba(139, 195, 74, 0.7);
  border-color: rgba(98, 137, 52, 0.7);
}
```

Supported `types` are:
- range
- box
- background

#### Method 2: HTML

A timeline entry can be created using a `<span></span>` or `<div></div>` tag, with the following attributes: 

```html
<span 
	  class='ob-timelines' 
	  data-date='2000-10-10-00' 
	  data-title='Another Event' 
	  data-class='orange' 
	  data-img = 'Timeline Example/Timeline_2.jpg' 
	  data-type='range' 
	  data-end='2000-10-20-00'> 
	A second event!
</span>
```
Timeline span and div entries (.ob-timelines class) are hidden in preview by default, however, if you wish to display content, try a snippet like this: 
```css
/* Render the ob-timelines span or div elements as inline blocks that use an italic font */
.ob-timelines {
  display: inline-block !important;
  font-style: italic;
}
/* Use the before pseudo element to display attributes of the span or div */
.ob-timelines::before {
  content: "🔖 " attr(data-date) ": " attr(data-title) ". ";
  color: lilac;
  font-weight: 500;
}
```
Using the above snippet, a span like this: 
```html
<span class='ob-timelines' data-date='1499-03-28-00' data-title="An example"></span>
```
would be rendered as: 
<img width="228" alt="image" src="https://user-images.githubusercontent.com/808713/159139934-e5c7cb5a-da31-4a57-8100-946f944010a3.png">
### Timeline Span block
- Hidden in preview by default in order to keep the note clean

### Date
- The most important and essential info, if it is not valid the note will be ignored

- Valid date format: 
  - `YEAR-MONTH-DAY-HOUR`
  - Only integers (numbers) are allowed in the date other then the 4 seperators `-` used to distinguish the different groups
  - The length of each element i.e. `YEAR` can be of any length for example `1234124314`
  - The same applies to MONTH, DAY and HOUR. this means **if your input time is not valid the plugin will not check that**. Why this choice? If this plugin is used for a fantasy setup where the # of month are not only 12 for example.
  - ALL 4 GROUPS must be specified however if they don't exist / not want to be shown replace them with a zero. For example if an event only has the year and the month it can be written as follows `2300-02-00-00` this will be rendered on the timeline as `2300-02` (the trailing zeros will be removed). For only a year `2300-00-00-00` -> `2300`
  - IMPORTANT: the sorting is based on the date being converted to an integer after parsing out the `-` this means to get the proper sorting, if minutes are not added you need to replace with `00` (the maximum number of integer for that date category.

### Title:
  - Optional
  - If a title is not specified the name of the note will be used

### Description:
  - Optional
  - If a description is not specified an empty section will be shown

### Background Image:
  - Optional
  - If an image is not specified no image will be shown (just text)
  - If an invalid url is given (an empty black section will be seen for that note card)
  - Currently only `http` & `absolute local path` will render, in the current obsidian release of `v0.10.13` obsidian links for background images are blocked from rendering, hopefully it is promised that this will be removed in the upcuming release. 

### CSS Class:
  - Optional
  - Adds the applied css class to the note card associated with this span info block


## Release Notes

### c0.3.5
 -frontmatter support
 -era suffix

### v0.2.1 
- Remove escaping of `quotes / double quotes and ticks` from title and text (no longer needed)
- Additional optional span attrictute `css class` allowing to custumize each card of the timeline
- `Multiple Events per note` are now allowed. All spans on a single note will be displayed on the timeline (previously only first span was presented i.e. 1 card event per note)

PR Contribution by https://github.com/ebullient

### v0.2.0
- Added `timeline` render block:
  - Keeps the note clean by hiding the html text (which can get messy with large timelines)
  - Automatic updates of the timeline everytime the note is opened anew (had to re-insert the timeline in previous version to update it)

- Tags searched will include both `inline` & `YAML` tags

- Removed the `Add Timeline` command as it is no longer needed with the addition of the render block
