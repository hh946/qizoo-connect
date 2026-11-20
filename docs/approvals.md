# Notifications, approvals and reconnecting

A notification ID means the Qizoo service accepted the message. Check the watch to verify delivery.

```sh
qizoo-watch notify --source codex --task "Report" --body "The draft is ready."
qizoo-watch ask-wait --source codex --task "Report" --body "Use this draft?" --option "Approve" --option "Decline"
```

Use `ask` to send a request and wait later. The CLI stores the ID and payload before sending. Save the ID with the originating AI task.

| Command | Use |
| --- | --- |
| `qizoo-watch pending` | List unfinished requests and their IDs |
| `qizoo-watch resume <id>` | Resume or retry the original request after a disconnect or uncertain send |
| `qizoo-watch wait-reply <id>` | Wait for a reply to an accepted request |
| `qizoo-watch get-reply <id>` | Check the current state of a request |

Exit code 2 means the operation failed. Exit code 130 means it was interrupted. After timeouts, sleep, or an uncertain send, resume by the original ID to avoid duplicate requests. The default wait is 300 seconds and the default request lifetime is 3600 seconds. Adjust them with `--timeout` and `--expires-in`.

When the status is `answered`, read `reply.choice` or `reply.text` and apply the original task’s rules before continuing. Report `expired` or `cancelled` to the user. A revoked computer needs to pair again. Request IDs belong to the connection that sent them.
