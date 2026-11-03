# Staff and contractor access on ion.id

**Status:** draft
**Date:** 3 November 2026
**Audience:** Domain Infra Admin, and developers of services that run on ion.id

This document says how people sign in to ION services, and how those services decide what each person may do. Staff stay on the existing `ion.id` Google Workspace. Contractors get addresses on `s.ion.id`. Both live in the one Google organization. Applications trust Google for the sign-in, and trust Google Groups for access.

---

## Executive summary

Each ION service would otherwise keep its own user table, its own passwords, and its own access list. A person who joins or leaves then has to be added or removed in every service, and a contractor needs a separate account in each one. Password resets, 2-step verification, and stale accounts all become per-application problems.

Use the `ion.id` Google organization as the single directory instead.

| | Each application keeps its own accounts | This directory |
| --- | --- | --- |
| Where the account lives | A user row in that application | One Google account, `name@ion.id` or `name@s.ion.id` |
| Passwords and 2-step verification | Implemented again in every service | Enforced once, in Google |
| A contractor joining | An account in every service they will use | One Cloud Identity user, then membership of the groups for those services |
| A person leaving | A deletion in every service, easy to miss | Suspend the Google user. Every service stops accepting new sign-ins |
| Who may do what | An access list inside each service | A Google Group. Adding or removing a member changes every service that checks that group |
| A whole team | The same people typed into each service | A team group nested inside the service groups |
| What contractors cost | Whatever each product charges for a seat | No Google Workspace licence. Cloud Identity Free is enough to sign in |
| What staff keep | A second set of credentials beside Gmail | The Google account they already have |

The application still chooses which group protects which action. It does not store a password, and it does not keep a second list of who is allowed in.

Contractors are ordinary users in the same organization, on the secondary domain `s.ion.id`. They receive a Cloud Identity Free licence and no Workspace licence, so they can sign in and be placed in groups, and they do not get Gmail, Calendar, Drive, or Meet.

---

## How the directory is arranged

* `ion.id` is the primary domain of the existing Google Workspace customer. Leave it as the primary domain.
* `s.ion.id` is a **secondary domain** of that same customer. It is where contractor addresses are created.
* Do not register `s.ion.id` as its own Google Workspace or Cloud Identity customer. That would be a second directory.
* Do not add `s.ion.id` as a domain alias. An alias is another spelling of a user who already exists. It does not create a separate population.
* Staff keep a paid Workspace licence.
* Contractors get only Cloud Identity Free.
* A Google Cloud project inside the `ion.id` organization holds the OAuth clients and one read-only service account.
* Each service checks an ID token, then asks the Admin SDK whether that user is in the required group.

Google sign-in reports the organization in the `hd` claim. For every user in this customer, including `name@s.ion.id`, `hd` is `ion.id`.

---

## Domain Infra Admin

You need a super administrator of the `ion.id` Workspace customer, and the ability to add a DNS record for `s.ion.id`.

### 1. Add the contractor domain

1. In the Admin console, open **Account > Domains > Manage domains**.
2. Add `s.ion.id` as a **secondary domain**. Do not choose the user-alias option.
3. Publish the TXT record Google shows you, and finish verification.

Users created after verification can have a primary address of `name@s.ion.id`.

### 2. Turn off automatic Workspace licensing

Do this before the first contractor exists.

1. Open **Billing > Subscriptions**.
2. Open the paid Google Workspace subscription.
3. Turn **automatic licensing** off.

If this stays on, every new user also consumes a paid Workspace seat.

### 3. Add Cloud Identity Free

1. From **Billing**, add the **Cloud Identity Free** subscription to this customer.
2. Confirm, on **Billing > Subscriptions**, that Cloud Identity shows a remaining-user count.

New users then receive a Cloud Identity Free licence on their own. You do not assign it by hand, and you cannot remove it. Delete the user when the seat should be released.

The free subscription raises the number of Cloud Identity users you can add by 50. Paid Workspace seats raise that cap further. If you need more free users, request additional Cloud Identity Free licences from Google. That request has no licence fee.

### 4. Create a contractor

1. Open **Directory > Users > Add new user**.
2. Set the primary email on `s.ion.id`, for example `ada@s.ion.id`.
3. Confirm the user has Cloud Identity Free and does **not** have a Google Workspace licence. If a Workspace licence was attached, remove it.
4. Require 2-step verification. The simplest place to enforce that is an organizational unit named `Contractors`, with the requirement applied to that unit.
5. Hand the person the first-sign-in instructions. After they enrol a second factor, they can be added to groups.

Suspending the user stops new sign-ins everywhere. Removing the user from a group removes only the access that group grants.

### 5. Create the access groups

Turn on **Groups for Business** under **Apps > Google Workspace** if it is not already on. It is included with Workspace and with Cloud Identity Free.

Use one group per permission, with the address on `ion.id`:

| Group | Members who should be in it |
| --- | --- |
| `<service>-users@ion.id` | Anyone who may sign in to that service |
| `<service>-admins@ion.id` | Anyone who may perform the service's administrative actions |

Keep team and vendor groups separate, and nest them in the service groups. Example: `vendor-acme@ion.id` is a member of `registry-users@ion.id`. Adding a person to `vendor-acme@ion.id` then admits them to every service group that contains the vendor group.

For every access group:

* Only administrators may add or invite members.
* Members outside this organization are not allowed.
* Treat the group as an access list. Leave conversation and posting features off unless the group is also a mailing list on purpose.

A contractor at `ada@s.ion.id` can be a member of a group at `registry-users@ion.id`. Both belong to the same customer.

### 6. Give the servers a read-only caller

Do this in a Google Cloud project that belongs to the `ion.id` organization.

1. Create a service account, for example `directory-reader@PROJECT_ID.iam.gserviceaccount.com`.
2. Enable the **Admin SDK API** on that project.
3. In the Admin console, open **Account > Admin roles > Groups Reader**.
4. Assign that role to the service account.

Groups Reader is enough. Do not make this service account a super administrator. Do not use domain-wide delegation: with the admin role, the account calls the Directory API as itself, and the audit log shows the service account.

Each application also needs an OAuth client in this project:

1. Set the OAuth audience to **Internal**. Only users in this organization can complete sign-in. That includes `@s.ion.id`.
2. Create one OAuth client per application. The client ID is the audience (`aud`) the application checks.
3. Record the authorized redirect URIs for that application.

If a server runs outside Google Cloud, the Domain Infra Admin places the service account JSON key on that server by the server's own secret mechanism. The key does not go into a repository, a chat, or a disk image. A server on Google Cloud should use an attached service account and should not have a key file.

### 7. When someone leaves

1. Remove them from the access groups that should stop immediately. Running services notice on the next check. Developers cache that check for a few minutes, so allow that delay.
2. Suspend the Google user so no service will accept a new sign-in.
3. Delete the user when the contract is finished and the Cloud Identity seat should be freed. Deleting the user does not delete the groups.

### 8. Checks before the first contractor signs in

* `s.ion.id` is a verified secondary domain, not an alias and not a separate customer.
* Automatic Workspace licensing is off.
* A test user at `…@s.ion.id` has Cloud Identity Free only.
* That user can complete 2-step verification and appear as a member of a test group.
* The `directory-reader` service account can perform the membership call in the developer section below.
* The test user has no Gmail mailbox.

---

## Developers

Google proves who the person is. Your service asks Google which groups they belong to, and decides from that. The ID token does not contain groups. You make a second call.

Fail closed. Allow the request only when the token checks pass **and** the membership call returns `isMember: true`. A timeout, a 403, or a malformed token is a denial.

### Sign-in

Use the OAuth client ID issued for this application. After Google redirects back, validate the ID token with Google's published certificates. Accept the token only when all of these hold:

* the signature and expiry are valid
* `aud` is this application's OAuth client ID
* `iss` is `https://accounts.google.com` or `accounts.google.com`
* `email_verified` is true
* `hd` is `ion.id`

`hd` is `ion.id` for staff and for contractors. An address ending in `@s.ion.id` with a missing or different `hd` is not a user in this organization.

Then keep your own session. Re-check group membership on a short interval, and always re-check it before an administrative action. A group removal has no effect on a session that never asks again.

### Which group

Keep the mapping in the service, next to the action it protects:

* signing in, and ordinary use, requires `<service>-users@ion.id`
* an administrative action requires `<service>-admins@ion.id`

Nested members count. `members.hasMember` follows nesting when the user and the group are in this same Workspace customer, which staff and `s.ion.id` contractors are. Do not call Cloud Identity `checkTransitiveMembership`. That method requires a paid Workspace Enterprise or Cloud Identity Premium subscription, and Cloud Identity Free returns 403.

### Calling the Directory API from Go

The process needs Application Default Credentials for the `directory-reader` service account, and this scope:

`https://www.googleapis.com/auth/admin.directory.group.member.readonly`

```go
package authz

import (
	"context"
	"errors"
	"fmt"

	"google.golang.org/api/idtoken"
	admin "google.golang.org/api/admin/directory/v1"
	"google.golang.org/api/option"
)

const hostedDomain = "ion.id"

// GoogleUser is the identity taken from a validated ID token.
type GoogleUser struct {
	Email string
}

// ValidateIDToken checks that raw was issued to this OAuth client
// for a user in the ion.id organization.
func ValidateIDToken(ctx context.Context, raw, oauthClientID string) (GoogleUser, error) {
	payload, err := idtoken.Validate(ctx, raw, oauthClientID)
	if err != nil {
		return GoogleUser{}, err
	}
	if payload.Claims["hd"] != hostedDomain {
		return GoogleUser{}, errors.New("user is outside the ion.id organization")
	}
	if payload.Claims["email_verified"] != true {
		return GoogleUser{}, errors.New("email is not verified")
	}
	email, _ := payload.Claims["email"].(string)
	if email == "" {
		return GoogleUser{}, errors.New("token has no email")
	}
	return GoogleUser{Email: email}, nil
}

// InGroup reports whether email is a direct or nested member of group.
// group is the group address, such as "registry-users@ion.id".
// Any API error is returned to the caller, and the caller must deny access.
func InGroup(ctx context.Context, group, email string) (bool, error) {
	svc, err := admin.NewService(ctx,
		option.WithScopes(admin.AdminDirectoryGroupMemberReadonlyScope))
	if err != nil {
		return false, err
	}
	resp, err := svc.Members.HasMember(group, email).Context(ctx).Do()
	if err != nil {
		return false, fmt.Errorf("hasMember %s: %w", group, err)
	}
	return resp.IsMember, nil
}
```

Cache a true or false result for a few minutes, keyed by the user email and the group address. Do not cache a transport error as "not a member" for long: the next request should try again. Bypass the cache for an administrative action.

`HasMember` takes the group address and the user's primary address or alias. `registry-users@ion.id` and `ada@s.ion.id` are both valid.

### Try a membership check

From a machine whose Application Default Credentials are the `directory-reader` service account:

```bash
curl -fsS \
  -H "Authorization: Bearer $(gcloud auth application-default print-access-token \
        --scopes=https://www.googleapis.com/auth/admin.directory.group.member.readonly)" \
  "https://admin.googleapis.com/admin/directory/v1/groups/registry-users@ion.id/hasMember/ada@s.ion.id"
```

A member looks like this:

```json
{"isMember": true}
```

Anyone else, including a person in the organization who was never added to that group, is `{"isMember": false}`. Confirm the same fact in the Admin console under the group's member list before treating a surprising `false` as an application bug.

A 403 from this call means the service account is missing the Groups Reader role, the Admin SDK API is disabled, or the token was minted without the scope above. It does not mean the user is absent.

### What the service must not do

* Do not accept an email address, or an `hd` value, from the browser without validating the ID token.
* Do not keep a local password, and do not keep a second allow-list of email addresses that an operator has to edit by hand.
* Do not commit a service account key. On a developer workstation, use a key issued for a non-production project, or impersonate `directory-reader` for the duration of the test.
* Do not grant access because the address ends in `@ion.id` or `@s.ion.id`. Group membership is the access decision.
