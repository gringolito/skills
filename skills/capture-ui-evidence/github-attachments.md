# GitHub attachments

For a pull request description or a comment on the main conversation, the GitHub CLI's
`--attach` flag uploads each file and rewrites local paths in the Markdown body to the uploaded
URLs.

Review thread replies have no attach option. Upload each file to GitHub's user attachments
endpoint on `uploads.github.com`, the same one the CLI uses, passing the repository ID, file
name, and content type. Embed the URL it returns in the reply. That endpoint is undocumented.
If it fails, post the media on the main conversation with `--attach`, and reply in the thread
with a link to that comment.
