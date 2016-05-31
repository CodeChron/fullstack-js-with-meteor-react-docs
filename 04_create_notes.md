# Form for creating notes

A good approach to implementing a feature is often to "follow the data."  Here, we'll do so first creating a form for creating a note and then adding a handler for inserting that note into our database.



Here we will create a single "Controller" component for managing:
- Creating a note
- Listing existing notes

Add a NotesContainer
**Render NotesContainer as a top-level component (this is new from the preview version, we need to do this because we want to have a top-level container structure to handle component state for the entire page)**

(when we add users, we'll switch this to users container)

Add SingleFieldSubmit to AppLayout and handle create note via props.






