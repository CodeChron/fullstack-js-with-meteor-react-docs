# Click to edit the note title

We want to make it possible to edit the note title. This feature will be very similar to what we did when implementing the main content editor. You should try building this on your own first and then compare it to the steps below.

One thing to keep in mind: in contrast to note content, a note title can't be empty. (If it were, then there would be nothing to click on in the notes list.)  Also, while the content block can be multiple lines (textarea) the title will just be a single line (text input)

- create a click to edit component, with only a single line field option for now
- add a handler for updating the title
- update title on keypress
- exit click to edit on return/submit or blur

