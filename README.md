# GroupCart

A shared shopping cart for groups. One person creates a room, friends join with a code, everyone adds items to the same cart, and at checkout the bill is split three ways. It's one HTML file with no backend and no install.

This is an unofficial demo made for learning. It's styled like a shopping site but has no connection to Amazon, and nothing here can place a real order.

## Why this exists

Buying things as a group is messy. Say a family is ordering for a festival, or roommates are stocking a new flat. Usually one person ends up with the cart and the card, while everyone else sends item links over WhatsApp. The same item gets picked twice, nobody knows the running total, and at the end the person who paid has to work out who owes what and then chase each person for it.

Shopping sites mostly assume one person, one cart. GroupCart tries a different setup:

- **One cart everyone can see.** People add things themselves, so there are no links to forward and no duplicates.
- **The total is always visible.** Everyone can see what the order costs, including GST, before it's placed.
- **The split is worked out for you.** Pick by item, equal, or custom percentages, and the app shows who owes the person who paid and how much.
- **Chat sits next to the cart,** so you can talk about a product without switching apps.

It started as a learning project. I wanted to practice real-time updates between users and the logic behind splitting a bill, without needing a server. It's a demo of the idea, not a finished product (see Limitations below).

## What it does

- **Rooms.** Create a room and get a code like `SHOP-4821`. Others join with that code and a name.
- **Shared cart.** Anyone in the room can add items, change quantities or remove them, and everyone's cart updates live. Each item shows who added it.
- **Group chat** with a typing indicator, plus an activity feed of joins, leaves and cart changes.
- **Admin checkout.** Only the person who created the room can check out. Everyone else sees "waiting for admin".
- **Three ways to split the bill:**
  - *By item:* each person pays for what they added
  - *Equal:* the total is divided evenly
  - *Custom:* set each person's share with sliders (it warns you if the percentages don't add up to 100)
- **Settlement view.** Since the admin pays upfront, it lists who owes whom and how much.
- **Totals in INR** with 18% GST added, and 18 products across 6 categories with search and category filters.

## How to run it

1. Download or clone the repo.
2. Open `index.html` in a browser.

To see the live sync:

1. Open the page in two tabs of the same browser.
2. In the first tab, enter a name and create a room.
3. In the second tab, enter another name and the room code to join.
4. Add items in either tab and watch the other one update.

## How it works

- **Live sync** uses the `BroadcastChannel` API. Each room is its own channel, and tabs send messages for cart updates, chat, joins and checkout. A new joiner asks for the current cart so it can catch up.
- **Split logic** lives in `calcSplit()`, which handles the three modes and corrects rounding so the shares always add up to the grand total.
- **Settlement** is in `calcSettlement()`. It finds who owes money and who is owed, then pairs them off, largest amounts first.
- Everything is plain JavaScript, with no framework or build step.

## Limitations

- `BroadcastChannel` only works between tabs of the **same browser on the same device**. Two different phones or laptops won't see each other. Real multi-device sync would need a backend such as WebSockets or Firebase.
- Data lives in memory, so refreshing the page ends the session.
- Products are hard-coded, and checkout doesn't take payments.

## Ideas for next

- Real multi-device rooms with a Node.js and WebSocket server
- Saving carts and past orders in a database
- A "mark as paid" status for each person's share

## Tech

HTML, CSS, JavaScript, BroadcastChannel API

## Author

Husain Chatiwala, [GitHub](https://github.com/Husain53100)
