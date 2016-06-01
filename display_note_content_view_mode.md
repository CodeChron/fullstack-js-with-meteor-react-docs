# Display Note Content View Mode

We now want to be able to exit the content editor and display our notes in view mode.  Additionally, we want to be able to click on the viewer to return to edit mode.

## Add a content viewer
The first we need is a component that we can use to view note content.

```
	modified:   imports/components/forms/content_editor.jsx
	modified:   imports/components/pages/note_details_page.jsx

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	imports/components/content/editable_content.jsx
	imports/stylesheets/helpers.css


```


## Exit edit mode and display

These are our general requirements for this feature:

- Allow for clicking on note content to edit it.
- Content auto-saves as you type.
- Click on Done or anywhere outside the content to exit edit mode. (discuss: the done button is just a dummy.)

## Display a clickable message if there is no content

What if I delete all content?  I will still need something to click on so I can add content.


## Use Shift + Return to exit edit mode



