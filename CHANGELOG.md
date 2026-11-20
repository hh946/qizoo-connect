# Changelog

## 0.2.0rc1 — prerelease

- Add sign-in-free pairing with confirmation on Qizoo Watch preview build 53 and the matching relay. The connection code and polling credential stay in memory. The CLI saves the revocable device credential after watch approval.
- Keep browser confirmation available for Watch build 52 with `connect --browser`. Add `service` to read service announcements.

## 0.1.1

- Add an optional `--campaign` flag for source links during pairing.

## 0.1.0

- Pair a computer with Qizoo on Apple Watch through a short-lived watch code and a separate website confirmation code.
- Send notifications and approvals from a computer AI. Retrieve replies by request ID and resume interrupted requests.
- Share a connection between authorized AI tools running under the same computer user. Revoke the connection when needed.
- Provide the public HTTPS interface and versioned, SHA-256-verified CLI downloads for the initial macOS target.
