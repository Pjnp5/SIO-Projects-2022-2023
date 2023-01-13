# **Message Protocol**

This document synthesizes the way messages work, using a table containing.

<br>

| **MESSAGE**            | **SENDER**   | **RECEIVER** | **Command**     | **Args**                                       |
| ---------------------------- | ------------------ | ------------------ | --------------------- | ---------------------------------------------------- |
| AuthorizationRequestMessage  | Player<br />Caller | PA                 | authorizationRequest  | user type, nickname, user public key, cc certificate |
| ValidAuthorizationMessage    | PA                 | Player<br />Caller | validAuthorization    | PA public key                                        |
| InvalidAuthorizationMessage  | PA                 | Player<br />Caller | invalidAuthorization  | -                                                    |
| InvalidAuthenticationMessage | PA                 | Player<br />Caller | invalidAuthentication | error message                                        |
| StartMessage                 | Caller             | PA                 | start                 | deck size                                            |
| CardShareMessage             | Caller             | PA                 | cardShare             | card                                      |Player             |                       |                                                      |
| DeckSendMessage              | Caller             | PA                 | deckSend              | deck                                      |
| DeckShareMessage             | Caller             | PA                 | deckShare             | deck, signature, caller symmetric key                                     |
| KeyShareMessage              | Player<br />Caller | PA                 | keyShare              | user symmetric key                             |
| AuditLogRequestMessage       | Player<br />Caller | PA                 | auditLogRequest       | -                                            |
| AuditLogResponseMessage      | PA                 | Player<br />Caller | auditLogResponse      | audit logs                                |
| UserListRequestMessage       | Player<br />Caller | PA                 | userListRequest       | -                                            |
| UserListResponseMessage      | PA                 | Player<br />Caller | userListResponse      | users list, signature                              |
| UserListSigningMessage      | Caller                 | PA | userListSigning      | final users list, signature                              |
| KickMessage      | Caller                 | Player | kick      | nickname                              |
| ResultsSendMessage      |         Player         | Caller | resultsSend      | deck, winners                              |