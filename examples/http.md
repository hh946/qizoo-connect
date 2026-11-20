# HTTP integration

Hosts that make HTTPS requests and securely store credentials can use the [public API definition](../openapi.json).

## Watch confirmation with build 53

This flow requires Qizoo Watch preview build 53 and the matching relay on `https://qizoo.tipuu.com`, or the equivalent relay in a test environment.

1. Generate an installation UUID and a separate random polling credential of at least 32 bytes. Keep the polling credential and watch connection code in memory.
2. Call `POST /watch-connect/direct/desktop/start` with `user_code`, `name`, `installation_id`, and `poll_token`. Retry an uncertain start with the same payload.
3. Show the returned `verification_code`. The user checks the code and computer name on their watch and confirms there.
4. Send `{}` to `POST /watch-connect/direct/desktop/poll`, using `poll_token` as the Bearer credential. Follow the returned `interval` and back off on throttling or network failures. Use `DELETE` on the same path to cancel a pending connection. Ask for a new watch code if the attempt expires or is cancelled.
5. On `connected`, save the returned `device_token` in the current user's protected storage. Use it for notification, approval, reply and revocation calls.

## Browser confirmation with build 52

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
