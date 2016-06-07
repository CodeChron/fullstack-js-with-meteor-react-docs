# Fix Empty Note title bug

You might have noticed that there is a bug, or issue, in how we are handling updating of note titles.

Currently it is possible to completely empty the title and save it.

This is a problem, since that means there will be nothing to click on in the list.

We could prevent the user from removing the last character but that would allow for completely renaming the note.

Instead, let's give the user the freedome to completely empty the note title and write a new title

If they decide to exit the editor and the title is empty, we'll replace it with "Untitled note"


