---
title: "Create your first TA code in Obsdian"
collection: talks
type: "Workshop"
permalink: /talks/resbaz-obsidian/code
venue: "QUT Kelvin Grove campus"
date: 2026-06-25
location: "Brisbane, Queensland, Australia"
---

> Only read this after your vault is set up! Obviously you also need a note in `Transcripts` containing a text transcript you would like to work from.
{: .notice--danger}

## 1. Select a piece of meaningful text

For example, I'll select the first sentence in my selected passage, '[[I have a dream]]':

> **Rev. Martin Luther King Jr.:** Five score years ago, a great American, in whose symbolic shadow we stand today, signed the Emancipation Proclamation.[^1] This momentous decree came as a great beacon light of hope to millions of Negro slaves who had been seared in the flames of withering injustice. It came as a joyous daybreak to end the long night of their captivity.

## 2. Copy and highlight the text

With the text selected, copy it to the clipboard (e.g. with 'Ctrl+C'). 

Then, with it still selected, type `==` (that's the equals sign, twice). Two equals signs will appear at either end of the selected text and it will be highlighted in yellow. 

> **Rev. Martin Luther King Jr.:** ==Five score years ago, a great American, in whose symbolic shadow we stand today, signed the Emancipation Proclamation==. This momentous decree came as a great beacon light of hope to millions of Negro slaves who had been seared in the flames of withering injustice. It came as a joyous daybreak to end the long night of their captivity.

> [!NOTE]
> Highlighting the text is optional but I find it helps me to remember what I have already coded and where I am up to, especially in a longer text.

## 3. Create a footnote

1. Click at the end of the highlighted passage to move the text selection point there.
2. Insert a footnote in one of these three ways:
	1. ==Basic way==: Right-click and select **Insert > Footnote**.
	2. ==Slightly cooler way==: Type Ctrl+P (or Cmd+P on Mac) and type `insfoo` to filter to the **Insert footnote** command, and press return.
	3. ==Coolest way==: Type the keyboard shortcut you selected in the [[1. Setting up your vault for Thematic Analysis|setup phase]] (like `Cmd+Option+Shift+F`)
3. A footnote marker will appear along with a text box for you to write in (see below)

![[Creating a footnote.png]]

## 4. Create the code

1. Inside the text box, type two ==left square brackets==. This starts the process of creating a new Obsidian note. 
2. Type the name of your code, then end by typing two ==right square brackets==. It should end up looking like this: `[[My great code]]`. 
3. Press return to finalise the note creation.

## 5. Add the code template

1. Scroll to the bottom of the page to find your numbered footnote with a link to the code. Alternatively, open the Footnotes view on the right sidebar (this is actually much easier). 
2. Click on the name of the code. It will take you to a new, empty note. 
3. Insert your code template in one of these ways: 
	1. ==Slightly cool way==: Type Ctrl+P (or Cmd+P on Mac) and type `temp` to filter to the **Templates: insert template** command, and press return.
	2. ==Coolest way==: Type the keyboard shortcut you selected in the [[1. Setting up your vault for Thematic Analysis|setup phase]] (like `Cmd+Option+T`)
	3. Select your `Semantic code` template.
4. The properties will populate. The Auto note mover plugin will move your code into the `Codes` folder and you will see a little notification that it has been moved.
5. Click into the 'Extract' property and paste (e.g. with 'Ctrl+P') the snippet of text you copied earlier. If it has dropped out of your clipboard for any reason you can always go back to the text, select it and copy again. 
6. Optionally, type something in the 'Context' property that will help you remember the context in which the quote was made. It should look like the below screenshot, but without the 'Themes' property filled in (you'll do that next).

![[Creating a theme.png]]

> [!SUCCESS] Ta-da!
> You've done it! That's your first code! Now, just rinse and repeat with the next text snippet.

