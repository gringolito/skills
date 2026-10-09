---
name: capture-ui-evidence
description: >-
  Show reviewers a UI change with screenshots or screen recordings attached where they read it.
  Use when opening a pull request with UI changes or responding to review feedback with a fix
  that changes the UI.
---

Let the reviewer judge a UI change without checking out the branch. When you finish, the pull
request or review reply shows every meaningful visual change with screenshots or recordings that
render inline on GitHub.

Capture the running application using the code under review. Mockups and design files don't
count, because they can't show a bug you introduced. Recapture evidence after any change that
could affect the appearance or behavior it shows. Use realistic data so the layout looks the
way users will see it, and keep secrets, tokens, and personal data out of the frame.

Match the media to the change. Use screenshots for static changes, such as a resized table, a
moved button, or new copy. Crop them to the affected area, leaving enough of the page around it
that the reviewer can tell where it is. For behavior changes, use a short recording showing the
interaction and its visible result. Include the starting state when the reviewer needs it to
understand the change. Add a before and after pair when the difference is too subtle to spot
alone. Skip changes with nothing visible to show.

Give each image alt text that describes the state it shows, such as "save button in the page
header". Put a recording alone in its paragraph so GitHub renders it as a player rather than a
link. Say in a sentence what the reviewer should look at when it isn't obvious.

Upload evidence as GitHub attachments rather than committing it to the repository. Keep review
replies in their original threads, because that is where the reviewer expects the answer. If
attachments cannot appear there, link to a conversation comment containing the evidence.

Read [GitHub attachments](github-attachments.md) when uploading evidence to a description,
conversation comment, or review-thread reply.

Verify that every attachment uploaded successfully, renders correctly, and appears next to the
text it supports. Delete the local captures only after verifying the published evidence.
