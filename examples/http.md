# HTTP integration

Hosts that make HTTPS requests and securely store credentials can use the [public API definition](../openapi.json).

1. Generate an installation UUID and a random secret of at least 32 bytes. Save both in the current user’s protected storage.
2. Call `POST /watch-connect/desktop/start` with the watch pairing code as `user_code`, along with `name`, `installation_id`, and `device_token`.
3. Verify that `verification_uri` is the official Qizoo account page at `https://qizoo.tipuu.com/watch-connect`. Ask the user to open the returned URL, sign in, enter the returned `browser_code`, check the device details, and confirm. Poll `POST /watch-connect/desktop/poll` with `poll_token` as the Bearer credential and `{}` as the body. Follow the returned `interval` and back off on throttling or network timeouts. If pairing expires, ask the user for a fresh watch code.
4. After confirmation, use the saved `device_token` as the Bearer credential for notification, approval, and reply calls on this connection. Disable redirects on credentialed requests.

Save each request’s complete payload and ID before sending. If the send outcome is uncertain, retry with the same payload and ID.

```http
POST /messages HTTP/1.1
Host: qizoo.tipuu.com
Authorization: Bearer <device_token-from-protected-storage>
Content-Type: application/json

{"id":"draft-approval-001","kind":"ask","source":"my-agent","task":"Draft","body":"Use this draft?","options":["Approve","Decline"],"expires_in":3600}
```

Retrieve the answer using `GET /replies/{id}`. Check the ID, source and task against the original request. Use `GET /watch-connect/desktop/status` to check the connection and `DELETE /watch-connect/desktop` to revoke it. Keep credentials out of logs, chats and public examples.
