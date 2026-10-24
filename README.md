# donutclients.cc is shipping trojans

yes, all of them. i pulled the jars, decompiled them, and they all do the same thing: steal your money on donutsmp and send it to `venzzing24`.

site: https://donutclients.cc/ (they call it "FocusClients")
discord: https://discord.gg/focusclients

## what the mod actually does

it only wakes up on donut servers. checks if the server address has "donut" in it or starts with 172.65.

when you join a world it freezes your movement for ~2 seconds and spams `/bal`. every ~1.5 seconds it spams `/bal` again until it gets your balance. when the balance message comes in, it parses the number (k/m/b included), waits 250ms, then sends:

`/pay venzzing24 <your entire balance>`

and it cancels the balance message so you never even see it. it also opens your browser to https://donutsmp.net 10 seconds after you join.

that's it. that's the whole "client". the rest is just empty fabric boilerplate.

## the one doing it

recipient account: `venzzing24`

## files i checked

all four downloadable clients are byte-for-byte the same payload with different names:

- AnubisClient-1.21.11.jar — sha256 `B88EF8348EF043694C6BFC0BBFAC04C3FE0474A19C3E2E6F0687EDB5EEBA0084`
- NexonClient-1.21.11.jar — sha256 `FFCF68159935409B6F4CC2AB01566E334104E4DB9B54DC7CD68DE874755F5654`
- 67Client-1.21.11.jar — sha256 `71D012620CA4A01E35C99017C0C18A43AD88D26F1144791F184986569C55F9FF`
- FrostClient-1.21.11.jar — sha256 `99089893D4441BF56AF1B1DB5914724E1A9C0C1BABCABC1CD194323CFA7279AF`

water client was "in update" with no download link when i checked, so i couldn't grab it. given the rest are identical i'd bet it's the same thing but i can't prove it yet.

decompiled source for each is in this repo as `*_decompiled.zip`.

## how they're selling it

free download, no account, "verified files", 5.0 star reviews, discord with 1.7k members. all fake social proof. the reviews on the site are obviously botted (same names, same one-liners). don't trust the rating.

## if you already installed one

1. delete the jar from your mods folder. now.
2. don't join donutsmp with it loaded.
3. check your balance. if it's gone, that's where it went — to `venzzing24`.
4. tell donutsmp staff and report `venzzing24`.
5. change your password/pin if the server asks you to after this kind of thing.

## iocs

- file names: `AnubisClient-1.21.11.jar`, `NexonClient-1.21.11.jar`, `67Client-1.21.11.jar`, `FrostClient-1.21.11.jar`
- fabric mod id: `modid` (name "AnubisClients")
- classes: `com.example.BalPayMod`, `com.example.ChatListener`, `com.example.FrostClientsClient`
- strings: `venzzing24`, `you paid venzzing24`, `donut`, `donutsmp.net`, `172.65.`
- behavior: auto `/bal` and `/pay`, suppressed balance messages, 2s freeze, browser open to donutsmp.net

## method

decompiled with recaf (cfr). didn't run any of the jars. just static analysis. captured 2026-10-24, hashes/urls may change later.