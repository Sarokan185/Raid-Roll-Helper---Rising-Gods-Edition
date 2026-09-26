# Raid Roll Helper – Rising Gods Edition

**Release: 3.6**  
**World of Warcraft: WotLK 3.3.5a / Interface 30300**  
**Server target: Rising Gods**

## Overview

Raid Roll Helper (Rising Gods Edition) is a raid-loot management and roll
assistant for WotLK 3.3.5a. It combines automatic loot capture, manual loot
entry, configurable rolls, token rolls, session management, raid logs and
trade handling in one compact interface.

The 3.6 release is based on the stable RRH 3.5.x codebase and includes the
finalized Help / Rules / Commands tab, the 15-second roll, session-only loot
capture and the optional automatic Master-Loot assignment preview.

## Loot

- Automatic loot capture for supported Master Loot and Group Loot flows.
- Manual drag & drop from the inventory into the Loot Table.
- Compact icon-based Loot Table with 15 items per row.
- Item tooltip on mouse-over.
- Remaining trade time is displayed on the item icon where available.
- `Delete Item` removes a selected loot entry manually.
- Manual deletion is recorded in the Raid Log.
- Right-click on a loot icon is reserved for the trade action.
- The trade path uses the stock 3.3.5 TradeFrame slot mechanism; RRH does not block a trade based on tooltip wording.
- `Trade Winner` can trade the selected winner's item through the normal
  WoW trade interface.
- Ending a session clears its live loot queue but preserves its historical log.

### Important trade note

RRH does **not** alter WoW or Rising Gods server-side soulbind, loot or trade
rules. Actual item tradeability is determined by the game client/server.

## Loot rules

- Focus Roll: **101–200**, highest priority.
- Main Spec (MS): **1–100**, second priority.
- Off Spec (OS): **1–99**, lowest priority.
- Priority tier is decisive; an OS roll does not beat an MS/Focus roll merely by being numerically higher.
- Highest roll wins within the same tier.
- Ties at the winner cutoff trigger a reroll among the tied players.
- Focus Roll is limited to two Focus Rolls per player where permitted.
- A normal roll cannot be submitted twice by the same player.
- Token Rolls distribute the configured number of token slots according to Focus > MS > OS.
- If only one player has a valid Token Roll, that player receives all token slots.
- Invalid/duplicate rolls are ignored; RRH suppresses repeated identical rejection messages during the same roll.
- Loot enters the RRH Loot Table only while an active RRH session exists.
- Manual loot deletion is recorded in the Raid Log.
- RRH does not override Blizzard/Rising Gods server-side loot, soulbind or trade restrictions.

## Roll system

Supported roll types/functions:

- 25-second roll
- 15-second roll
- Token roll
- Cancel Roll
- Focus Roll
- Main Spec (MS)
- Off Spec (OS)
- Multiroll

Focus/MS/OS priority and reroll handling follow the established RRH roll
engine. Roll winners and detailed roll histories are stored in the Raid Log.

## Token rolls

Token rolls can process multiple identical items from the current session.

The roll engine supports:

- configurable token count
- Focus/MS/OS priority
- unique-player handling according to the established token-roll rules
- boundary ties and rerolls
- individual assignment of token entries
- individual token logging and trade tracking

## Sessions

### Automatic raid sessions

Inside an actual raid instance, RRH automatically creates or restores the
appropriate raid session.

The active session displays:

- raid name
- session number
- Raid Instance ID

A session is not randomly created outside a raid.

### Manual sessions

`New Raid Session` can be used to create a named session manually.

### Ending a session

`End Current Session`:

- ends the active session
- cancels remaining active roll state
- removes the session's live loot/queue entries
- resets live bag-loot tracking for the session
- preserves the historical Raid Log

Items still physically present in the player's bags are not automatically
re-added to a later session. They can intentionally be added again by
drag & drop.

## Raid Log

The Raid Log records, where applicable:

- loot entries
- roll starts
- individual player rolls
- roll type
- winners
- winning roll
- rerolls
- manual loot deletions
- relevant trade/status events

Features:

- Older / Newer session navigation
- Save Log
- Delete Log
- scrollable log view
- historical sessions remain available independently of the active session

## Copy Raid

The Copy view provides a detailed, copyable session report including the
individual roll history for each item.

It is scrollable and is intended for copying a complete raid record into
chat, a document or another log.

## Loot methods

### Master Looter

RRH supports automatic loot capture and raid-loot management for Master Looter.

### Group Loot

RRH supports automatic capture of received Group Loot items and rebinding
the tracked item to the corresponding bag entry.

The actual distribution remains controlled by WoW/Rising Gods.

## Minimap button

The RRH minimap button supports:

- **Left-click:** open the main RRH window (`/rrh`)
- **Right-click:** switch between Group Loot and Master Looter when the
  required raid/group authority is available
- **Hold right mouse button:** move the button around the edge of the minimap

The button position is saved in the addon database.

## Slash commands

| Command | Function |
|---|---|
| `/rrh` | Open/close the main RRH window |
| `/raidrollhelper` | Alias for `/rrh` |
| `/rr1` | Start a roll using the roll parser |
| `/rrlog` | Open the Raid Log |
| `/rrloot` | Open the Loot window |
| `/rrcancel` | Cancel the active roll |
| `/rrnew` | Open New Raid Session |
| `/rrtest` | Internal test/debug command |

`/rr1` accepts the established roll-input syntax, including explicit roll
duration and item link, for example `/rr1 25 [Item]`.

## Preview feature: automatic Master-Loot assignment

Version 3.5.55-preview2 adds an optional, safety-first automatic Master-Loot assignment feature.
In New Raid Session, the raid leader can configure target players for:

- Boss-Loot (epic/legendary boss items)
- Saros / Primordial Saronite (item 49908)
- Shadowfrost Shards / Splitter (item 50274)

The configured targets are saved globally in Settings and apply to both automatic raid sessions and manually created sessions.
The feature is disabled by default. When enabled, a confirmation preview is shown before
items are distributed unless the user explicitly disables confirmation.

Only the actual Master Looter processes the assignment. RRH re-checks the target player and
current loot slot immediately before each `GiveMasterLoot()` call, so loot-slot changes do
not cause the wrong item to be assigned. If a configured target is not currently available
as a Master-Loot candidate, RRH leaves that item in the loot window.

The feature is a preview and should be tested with non-critical loot before being used in a
live raid.

- Duplicate/repeated invalid roll events are announced only once per player and reason during an active roll.

## Help / Rules tab

The main menu now contains a scrollable **Help / Rules / Commands** tab that
summarizes the addon rules, major functions, minimap controls and slash
commands directly in-game.

## Settings

The Settings page controls:

- roll sound
- epic boss-loot announcements
- session-log saving

The settings are persisted in `RaidRollHelperDB`.

## Compatibility

- Interface version: **30300**
- Target client: **WotLK 3.3.5a**
- The code avoids modern APIs that are unavailable on the target client.
- The release was statically audited for the previously encountered
  3.3.5a-incompatible calls such as `SetClipsChildren` and `FormatLeft`.

## Known limitations

- Server-side loot/trade rules are outside the control of the addon.
- Actual in-client execution cannot be reproduced in this development
  environment; final validation must therefore be performed on Rising Gods.

## Release history

### 3.5.54 – Final Release
- Automatic sessions are created only while physically inside a real raid instance.
- A new automatic session is created only by the raid leader or a raid assistant.
- Open-world zones such as Northrend can no longer create automatic sessions.
- Existing active raid sessions are preserved across temporary exits/deaths as before.
- Final trade-path correction: RRH no longer rejects a normally tradeable item based on a localized tooltip/bind-text heuristic.
- Trade insertion follows the classic WotLK TradeFrame flow; server/client tradeability remains authoritative.
- Trade path hardened: removed the locale-sensitive tooltip gate that could falsely report a tradeable item as non-tradeable.
- Trade now uses the native 3.3.5 `PickupContainerItem` + `ClickTradeButton` path and returns rejected items to their original bag slot.
- Based directly on stable 3.5.52.
- Added the scrollable Help / Rules / Commands tab.
- Added explicit EditBox font/rendering for the New Raid Session field so
  typed text is visible.
- Preserved all 3.5.52 session, queue, loot, roll, log, minimap and trade
  functionality.
- Localization changes from the experimental 3.5.53–3.5.59 builds are **not**
  included.

### 3.5.52
- End Current Session clears all live loot-queue entries belonging to the
  ended session.
- Automatic bag-loot tracking is reset at session end and new-session creation.
- Items remaining in bags are not automatically re-added to a new session.
- Manual drag & drop remains available for intentionally re-adding items.

Earlier development history is retained in the project history rather than
in the release README.

## Credits

Based on Raid Roll Helper - 3.3.5a by dapeda_94.

This Rising Gods Edition contains subsequent compatibility, UI and feature
work for the Rising Gods WotLK 3.3.5a environment.


## Disconnect-/Crash-Recovery (3.6.1 Preview)

RRH now includes the bundled **RRH SafeGuard** recovery layer. It keeps a
previous complete database snapshot in a separate SavedVariables file and
marks the active database as clean or interrupted. On the next login RRH can
recover from a damaged SavedVariables file instead of silently starting with
an empty database.

This protects the persisted session, loot table and raid logs against the
common forced-disconnect / SavedVariables-corruption scenario. A hard process
kill that happens before WoW writes any SavedVariables cannot be solved by a
normal addon API; WoW does not expose a general-purpose live file-write API.

- Identical boss-loot items are tracked as separate loot slots; delayed Master-Loot assignment processes each identical slot individually.
