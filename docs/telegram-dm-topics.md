# Telegram DM Topics Guide

How to use **Telegram forum topics inside a direct chat** with OpenClaw.

This is different from the usual supergroup/forum setup.

## What this is

Telegram now supports forum-style topics in some direct chats. OpenClaw can work with them, but there are a few gotchas:

- each topic behaves like a separate conversation surface
- topic messages need an initial message after creation or Telegram may not visibly show the topic
- access/pairing checks may trigger when a new topic appears for the first time

## What OpenClaw can do

OpenClaw exposes Telegram topic creation through the message tool / channel actions.

Relevant Telegram action:

- `createForumTopic` (`chatId`, `name`, optional `iconColor`, `iconCustomEmojiId`)

Ergonomic alias:

- `topic-create`

After topic creation, immediately send a message into the topic thread.

If you skip that step, the topic may not show up properly in Telegram.

## Recommended creation flow

1. Create the topic
2. Capture the returned `topicId`
3. Send a first message into that topic using `threadId=<topicId>`

Conceptually:

```text
1. topic-create -> returns topicId
2. send message with threadId=topicId
```

## Example flow

Create a topic named `Coding`, then send the first message into it.

Pseudo-example:

```text
action=topic-create
channel=telegram
target=<chat>
name=Coding
```

Then:

```text
action=send
channel=telegram
target=<chat>
threadId=<returned-topic-id>
message=Coding topic ready
```

## How to use DM topics

Once created, treat each topic like a separate thread.

Typical uses:
- `Coding`
- `Research`
- `Leads`
- `Ops`
- `Personal`

Use the returned topic/thread id when sending follow-up messages.

## Access / pairing gotcha

A newly created topic in a DM may trigger OpenClaw access control or pairing checks.

Why this happens:
- OpenClaw can treat a new DM topic as a new conversation surface
- if access policy is not already configured for that surface, Telegram pairing/auth may fire

Typical symptom:
- a pairing or "access not configured" message appears right after using the new topic

What to do:
- approve/pair the new surface if required by your Telegram access policy
- then retry the first message in the topic

## Internal owner vs visible sender

If you later route DM topics with native topic `agentId` config:

- `agentId` selects the **internal OpenClaw agent**
- the Telegram account/bot still controls the **visible sender**

So topic routing changes the brain, not the face.

## Best practices

- always send an initial message right after topic creation
- record topic ids if you plan to automate follow-up messages
- expect new-topic pairing behavior in stricter access setups
- use DM topics for organization, not as a substitute for multi-bot identity

## When this is useful

DM topics are great when you want:
- one person chatting privately with one bot/account
- multiple organized lanes inside that same chat
- clean separation between coding, research, ops, etc.

DM topics are not ideal when you want:
- multiple visible specialist bot identities
- public team presence with different bot personas

For that, use the multi-bot supergroup model instead.
