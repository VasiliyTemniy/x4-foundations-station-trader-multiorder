# Station Trader Multi-Order

Station traders assigned to a player station haul one ware at a time in the base
game. With this mod, once the base game has picked a trade for the trader, it
tops the rest of the hold up with other wares the same station needs (when
importing) or sells (when exporting) on that same round trip — pulling from, or
delivering to, as many stations as it takes to fill up. One trip moves a full
load instead of a sliver, without changing what the trader was already going to
do.

## Why

A station trader with a 6,000-unit hold happily departs on a round trip using
less than one percent of its capacity. Several mods fix this by adding custom
"trader" jobs — but none of them group those traders under stations or
commanders, so every one of those ships is *unassigned*. The map's property
view tucks subordinates away in their commanders' submenus while unassigned
ships sit in plain sight at the top level, so custom traders pile up and
clutter the list fast. This mod is one way around both problems at once: the
ships stay ordinary station-trader subordinates — grouped and tidy — and their
trips actually fill the hold.

## Game version compatibility

- 9.00 release - **supported**
- 9.00 betas and release candidates - **supported**
- 8.00 release - **supported**

## Dependencies

- **SirNukes Mod Support APIs** ([link](https://www.nexusmods.com/x4foundations/mods/503)) — hard
  dependency. Provides the Simple Menu / Options helpers used by the options menu.

## How it works

Every extra trade obeys the same rules a normal trade run would: your trade rules
(allowed buy-from / sell-to factions), sector and station blacklists, and a
station's own trade restrictions are all respected, and unreachable stations are
skipped. The mod never sends a trader anywhere the base game wouldn't.

It runs automatically for station-trader subordinates — there are no menu
actions. Everything is tuned from Extension Options, and turning it off restores
exactly vanilla behaviour.

Works for every cargo bay type: container freighters, solid haulers (mineral
miners), and liquid haulers (gas miners — the gases are liquid-transport wares).
A mineral or gas miner assigned to *Trade for Commander* tops up and multi-drops
on its own bay just like a freighter does.

## Options (Extensions → Mod Options → Station Trader Multi-Order)

| Option | Default | What it does |
|---|---|---|
| Enable for player owned ships | on | Apply to your own traders. Off leaves them on vanilla single-ware behaviour. |
| Enable for NPC owned ships | on | Apply to NPC traders too. Off is lighter on performance and leaves the NPC economy untouched. |
| Enable for traders | on | Apply to ordinary station-trader subordinates. |
| Enable for build storage traders | on | Apply to build-storage-trader subordinates. |
| Max wares per trip | 3 (1–7) | How many distinct wares a trader may add to one round trip. |
| Min cargo usage | 40 % (0–100) | Skip a run whose total load is below this share of cargo capacity. 0 = never skip. Build-storage runs always go. |
| Max extra trades | 5 (0–10) | How many extra pickup/delivery legs a trip may add beyond the base trade (one ware can use several). |
| Extra-trade target fill | 80 % (0–100) | Stop adding extras once the trip reaches this fill. Lower = less searching. 0 = fill until other limits hit. |
| Per-extra minimum fill | 10 % (0–50) | Reject any single extra leg smaller than this share of cargo, so tiny scraps don't bloat the route. 0 = no floor. |
| Drop underfilled vanilla order | off | Also apply the per-extra floor to the base-game trade itself, not just the extras. On = a base trade below the floor is dropped (and the matching delivery to/from your station is trimmed to match). Build-storage runs are exempt. |
| Debug logging | off | Writes per-ship logs under `VAS_StationTraderMultiorder/`. |

> **Min cargo usage tip:** set it too high and a trader can get stuck — if no
> reachable mix of wares ever fills it to the threshold, it keeps finding a
> trade, rejecting it as too small, and idling instead of trading. If a trader
> seems idle, lower this value. `0` disables the check.

> **Drop underfilled vanilla order:** normally the base-game trade is always
> kept, even if tiny — only the *extra* legs obey the per-extra floor. Enable
> this to floor the base trade too, so a trader never makes a dedicated stop for
> a sliver. If every reachable trade is below the floor the trader idles and
> retries later (same as Min cargo usage), so leave it off if you'd rather a
> small trade than none.

## What it does not touch

- Ware baskets — the station's own basket still decides which wares a trader considers.
- Mining subordinates, free traders (no commander), and other non-trade orders are untouched.
- Trade prices — base-game pricing is unchanged.
- When the mod is disabled (globally or per side), traders behave exactly as vanilla.

## Compatibility

This mod patches a vanilla file: `aiscripts/order.trade.routine.xml`, the
TradeRoutine order that every station-trader subordinate runs. That is not a
side effect — it is the whole point. The mod improves the existing trader role
where it lives instead of bolting a separate custom trader job on top, which is
exactly what keeps the ships grouped under their stations. The injections are
written so that with the mod disabled in options, the patched code paths
reproduce vanilla behaviour exactly.

Other mods that modify the same script may conflict. Mods that add their own
custom trader jobs are unaffected.

## Credits

- Inspired by the trade-grouping idea in **mbleichner's Warehouse Fleets**. All
  implementation is original.
- By VasiliyTemniy.

## Source

https://github.com/VasiliyTemniy/x4-foundations-station-trader-multiorder
