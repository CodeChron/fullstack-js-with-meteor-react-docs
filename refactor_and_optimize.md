# Refactor and Optimize

Discuss Kent Beck: "Make it Work.  Make it Right.  Make it Fast."

Optimize the list view to lock the count on create.
Look for other optimization opportunties.


## Lock the note count before creating to prevent the note from appearing before redirect

- get note count before create
- set list limit to current count - via subscription
- reset limit after redirect



