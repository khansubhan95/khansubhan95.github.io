---
layout: post
title:  "My Notes Setup"
date:   2018-12-31 22:22:32 +0530
permalink: /my-notes-setup/
---
I use a plain text approach for notes with nvALT on Mac and 1Writer on iPhone. nvALT itself is based on an app that follows a plain text approach to note taking: [Notational Velocity](http://notational.net).

I have setup nvALT so that it reads notes (stored as text files) from a folder nvALT in Dropbox. Images are stored inside a subfolder of this nvALT folder in Dropbox(i.e. nvALT/nvALT-images). The notes can be written in plaintext or markdown but are stored as .txt files. nvALT can render these text files in Markdown. Here is my folder hierarchy-

Dropbox -> nvALT(stores notes) -> nvALT-images(stores note images)

Notes in nvALT have two parts: title and content. nvALT also provides really fast note searching. Simply use ⌘+L to search for notes based on note title and content. If you press return during this, a new note with the typed content as the title is created(provided a note with the same title does not already exist). 

On iPhone I use 1Writer to read these notes from the Dropbox folder. 1Writer can render these notes in .txt in Markdown. Basically any app that can render text into markdown can be used on iPhone (iA writer, Editorial etc). 1Writer provides fast searching as well as a tags for filtering.

You can install nvALT from [here](http://brettterpstra.com/projects/nvalt/).

To read the images from the subfolder nvALT-images in nvALT you need to edit the template.html in ~/Library/Application Support/nvALT/ and add 

```
  <base href="file:///Users/subhan/Dropbox/nvALT/“>
```
 before `</head>`
 
 Images can be inserted using `![](nvALT-images/image_name)`
 
 This approach allows notes to be read by both nvALT and 1Writer.
 
You can also change the how the markdown is rendered by editing template.html and custom.css in ~/Library/Application/nvALT.

For filtering I use a tag based approach. I add tags by adding `#tagname` (# and tag name with no space in between) to the bottom of the note. 1Writer can read these tags and provide tag based filtering. In nvALT I again use ⌘+L to search for `#tagname` to find notes matching the tag.

Another useful feature is adding links to other notes. This can be done by using `[[note name]]`. This approach works in both nvALT and 1Writer.

Earlier, I used to use Apple Notes for note taking. However I found that I was increasingly wasting time on formatting and other bells and whistles. I found this plaintext note taking approach to be time saving and more customisable. 

**Update**

I have started using Bear notes instead of nvALT. Bear is focused on markdown, but also provides additional features like tags, multiple export option, a dozen themes and a beautiful UI.
