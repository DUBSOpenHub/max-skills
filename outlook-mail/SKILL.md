---
name: outlook-mail
description: Read and send Outlook email via Microsoft Graph. All outbound email requires explicit user approval before sending.
---

# Outlook Mail Skill

You can read and send email through Microsoft Graph API via the `microsoft-graph` MCP server.

## Available Actions

### Reading Email
- List recent emails from inbox
- Read a specific email by subject or sender
- Search emails by keyword, sender, or date range
- List unread emails
- Get email attachments

Use the Microsoft Graph MCP tools directly for read operations. No approval needed for reading.

### Sending Email — ⚠️ APPROVAL REQUIRED

**CRITICAL RULE: You must NEVER send an email without explicit user approval.**

When the user asks you to send, reply, or forward an email, follow this exact workflow:

1. **Draft the email** — compose the full email (to, cc, subject, body)
2. **Present the draft for review** — show it to the user in this exact format:

```
📧 DRAFT — awaiting your approval
─────────────────────────────────
To:      recipient@example.com
Cc:      (none)
Subject: Your subject here
─────────────────────────────────
Body:

Your email body here...

─────────────────────────────────
Reply "send" to approve, "edit [changes]" to revise, or "cancel" to discard.
```

3. **Wait for the user's response:**
   - **"send"** or **"approve"** or **"yes"** → Call the Graph API to send the email. Confirm with "✅ Sent."
   - **"edit [changes]"** → Apply the requested changes, show the updated draft, and wait for approval again.
   - **"cancel"** or **"no"** → Discard the draft. Confirm with "🚫 Draft discarded."

4. **If the user's intent is ambiguous**, ask for clarification. Never assume approval.

### Reply & Forward
Same approval workflow applies. When replying:
- Show the original message context
- Draft the reply
- Present for approval before sending

## Channel-Specific Behavior

### Telegram
- Keep email previews concise — truncate long bodies to first 200 chars with "... (truncated)"
- Use short confirmations: "✅ Sent to sarah@contoso.com"

### TUI
- Show full email bodies
- Can display more detailed headers

## Common Patterns

**"Check my email"** → List the 10 most recent emails with sender, subject, date, and read/unread status.

**"Any emails from Sarah?"** → Search for emails from Sarah, show matches with subjects and dates.

**"Reply to Sarah's email about the Q2 report"** → Read Sarah's email, draft a reply, present for approval.

**"Email the team about the standup change"** → Ask who "the team" is (or check memory), draft the email, present for approval.

**"Summarize my unread emails"** → Fetch unread emails, provide a concise summary of each.

## Security Notes
- Never include email credentials, tokens, or auth details in responses
- Never forward emails to addresses the user hasn't specified
- If asked to send to a large number of recipients, confirm the list before drafting
- Treat email content as confidential — don't store email bodies in memory unless the user explicitly asks
