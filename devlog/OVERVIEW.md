# Magic Pocket Pistol — how the game was built

**2026 Epic AI MegaJam · Unreal Engine 5.9 · one entrant and two AI coding agents (team "Magic Pocket Pistol")**

> Written by Claude Code on 2026-11-15 from the project's shared files and git history. The entrant accepted the first version at 14:32, and this one at 18:48 after reading a Chinese summary of the additions. Times are UTC+8. Quotes from the entrant are translated from Chinese, and marked as such.

This page is the short version of the dev log and can be read on its own. The full record:

- [`DEVLOG.md`](../DEVLOG.md): the task-by-task log kept by Codex, with the evidence for each check (logs, screenshots, videos).
- [`devlog/conversations/`](conversations/): every message between the entrant and Claude Code, in the original Chinese, unedited except for passages about another project and private matters, which the entrant asked to hide (each one is marked). The entrant asked for this and said (translated): "I am not necessarily right. All of this can serve as a reference, positive or negative."
- [`DECLARATION.md`](../DECLARATION.md): the materials made before the jam, the AI tools, and the sourced assets with their licenses. It is kept in the "Kit" repository, which holds the tools made before the jam, and is published next to this dev log.
- Not published, and kept for the organizers, who may ask to see them: `DESIGN.md` (the entrant's decisions in their own words, in Chinese, each with its time, and the open questions with the values used until they were answered), `relay/` (the 53 task instructions and reports the two agents exchanged), and the git history of the game project and of the Kit.

## The game

- **Name:** MagicPocketPistol.
- **Theme:** Director's Cut. The entrant's reading (translated): "With AI's assistance, turning what was impossible into something possible, and presenting the world one has in mind better." The entrant's view (translated): "I think my design fits the theme."
- **Where it comes from** (the entrant, 11-12 15:34, translated): "The original target was people who are not good at Fortnite but whose friends all play it. … When I play a battle royale in a team with friends, I am often the first to die, and after that I can only watch my teammates and do nothing. Then my friends, not wanting me to wait too long, take high-risk actions. So I added the original 'ghost system', now the 'small animal system', so that some players dying, hiding and dancing can also be part of the tactics."
- **The core** (the entrant, translated): "The real core of the game is that players who are relatively weak at it still have something to do, and the outcome is not decided by only a few abilities."
- **How it plays:**
  - A battle royale on an island: 50 contestants in ten teams of five. The player and four AI teammates play against nine AI teams, inside a shrinking zone.
  - **Everything is a magazine.** The magic pocket pistol is loaded with objects from the map; bigger objects give more shots. Objects used up as magazines disappear, so the cover on the map keeps shrinking.
  - **Nobody gets hurt.** Shots are light effects. Contestants have a "theoretical" health; when it runs out, the contestant turns into a small animal.
  - **Being out is still playing.** A small animal can't be hurt. It can ping enemies for its team, shove other small animals, carry items to teammates, and stand in front of teammates to block shots. A teammate who finishes a 30-second dance brings every knocked-out teammate back.
  - **Wild animals.** Petting one gives the best ammunition ("friendship bullets"), and the animal then follows its contestant in a line.
  - **Airdrops** carry a disco ball and a speaker. Each one cuts 10 seconds off the dance, and makes the dancer easier to spot.
- **Designed as a multiplayer game, built as a single-player game** because of the jam. The AI contestants stand in for human players: they play by the same rules and use the same abilities as the player.

## Who did what

| | Role in this project |
| --- | --- |
| **The entrant** (one person) | Every design, scope and art decision. Chose the 3D models and made them with AI tools: the contestant in Tripo, rigged in Blender; buildings, furniture, props and the airdrop crate in Hyper3D Rodin. Separated the character's hair, ears and tail and made their physics; edited the dance in Cascadeur; chose the music and sounds. Played the game in the Editor and in packaged builds and reported what looked or felt wrong. Did every account step, and does the submission |
| **Claude Code** (Anthropic; Claude Opus 5.5, and Claude Fable 5.2 for two stretches on 11-12 and 11-13) | Wrote nearly all of the game's own C++: six modules, about 11,500 lines (`Contestant`, `LightShot`, `Magazine`, `ContestantAI`, `Match`, `HUD`). Recorded each of the entrant's decisions word for word in `DESIGN.md`, and listed what was still undecided as numbered questions with the values used in the meantime. Wrote Codex's task instructions (what to change, what to check, when to stop) and reviewed its reports. Wrote these documents |
| **Codex** (OpenAI; `gpt-6-astra`, the model it reported at the end of the jam) | Worked in the Unreal Editor through Unreal MCP: Blueprint subclasses, materials, Niagara effects, the island, PCG, imports, retargeting, physics. Checked the features in Play-In-Editor with logs, screenshots and videos. Built, packaged and smoke-tested the game, and requested the generated images. Keeps `DEVLOG.md` |

**How the agents worked together.** They share no chat, only files.
- `AGENTS.md` holds the rules. `STATUS.md` is the task board: who holds the Editor, the tasks, the decisions waiting for the entrant, and a hand-off log. `relay/` holds one instruction-and-report file per task.
- Each file has one writer, one agent at a time holds the Editor, and packaging takes a lock file.
- The agents never use the entrant's accounts, never download or install software, and never upload or submit. Those steps go on the entrant's list.

**Some numbers** (2026-11-12 13:00 to commit `9b1f025`, 2026-11-15 18:16):
- 378 commits in the game repository: 245 by Claude, 132 by Codex, and the bootstrap commit.
  - By calendar day: 11-12, 18 by Claude and 9 by Codex; 11-13, 61 and 46; 11-14, 90 and 35; 11-15 to 18:16, 76 and 42.
- 97 dated entries recording the entrant's decisions and notes (`DESIGN.md` section 1), and 98 numbered open questions (section 2.2).

## Before the jam (2026-10-25 → 2026-11-12 13:00)

Only tools were prepared, as Official Rules 7(h) allows: agent rules, MCP settings, build, package, smoke-test and backup scripts, a prompt library, and a shortlist of licensed free assets. They were tested on a throwaway project with generic test content; nothing from it is in the game. At 01:34 on 11-12 the entrant moved the jam to Unreal Engine 5.9, after four checks passed on 5.9: MCP, C++, assets and packaging. The theme was revealed at 03:00, and the design talk that followed was talk only. The full account is in `DEVLOG.md`, "Before the jam".

## Day 1 — 11-12 13:00 → 11-13 13:00

**Built**
- 15:52: the game project, from the Unreal Engine 5.9 Third Person C++ template. Bootstrap and task board followed.
- 21:41: the first contestant, with light shots and theoretical health; at zero health it turns into a placeholder animal. Checked in Play-In-Editor (PIE). The shoulder camera and crosshair followed at 23:01.
- 02:49–08:11: the contestant's look. Concept images were generated through the OpenAI Images API, round after round, as the entrant asked for changes. The entrant chose one at 06:59.
- Checked in PIE: everything-is-a-magazine at 08:44, AI contestants at 10:02, random start positions at 10:34, and the dance revive at 11:43.

**The entrant's decisions** (a selection; all of them are in `DESIGN.md` section 1 and in the conversation record)
- 15:34: the project's background and its core (quoted above).
- 21:24 to 02:34: answered the first 23 open questions. The answers covered the dance, the airdrop items, the zone, teams of five in ten teams, and "everything is a magazine". At 22:28 the entrant chose the dance music, Fun House Music from Fab.
- 07:38: the character would be built in Tripo Studio's character template, instead of the planned pipeline that generated part images first.
- 07:51, 09:03 and 10:48: chose which feature to build next.
- 11:17: "Help me check and reorganize the jam's work schedule; I'm afraid there isn't enough time" (translated). At 12:43 the entrant agreed to the new order.

**Problems and fixes**
- The Unreal build tool could not start from the agents' shell at first: the child process lacked normal command extensions. Codex fixed it with a setting that applies only to that child process.
- A template map created extra external-actor packages, so the first test map was made from the engine's non-partitioned template.
- See `DEVLOG.md` for every check that did not pass the first time.

## Day 2 — 11-13 13:00 → 11-14 13:00

**Built**
- 13:25: the first packaged build, plus the zone and the match flow. The first match map followed at 13:59, and at 19:22 an island with a stand-in town and forest. The island build passed the smoke test on DirectX 12 and DirectX 11.
- Evening: small-animal controls, and 26 reference images of building parts and furniture for the entrant's models.
- 20:52 and 22:20: the entrant's character, first and second versions. It was in the game by 23:38 and in a packaged build by 23:46. The seven animals came in at 23:56.
- 00:08 to 07:16:
  - the pre-match screen, carrying, and pings;
  - a multi-storey town with varied buildings;
  - a forest scattered with PCG;
  - roads, items and armour.
- 07:52 to 12:12:
  - colours for the placeholder items, and punches;
  - aiming, interaction and on-screen control hints;
  - the entrant's Rodin buildings and furniture;
  - a build for a friend to try.

**The entrant's decisions** (a selection)
- 13:22: the map is a small island with a town and a forest.
- 19:02: the entrant moved Claude's work to Claude Opus 5.5 ("let Opus 5.5 do it", translated). The Claude Fable 5.2 session had run out of usage three times that day.
- 23:00: the animals come from one free Fab pack, Animals FREE.
- 04:10: added parts of the original design that had not been written down: healing items, tiers of armour and shields, shield recharges, pet treats. At 04:18 the entrant chose wild animals and treats first.
- 05:20: the forest uses Quixel Megaplants from the entrant's Fab library.
- 06:05: health, armour, items and ammo must be on screen, and the pistol grip looked unnatural. At 07:25: punches when out of ammo.
- 10:50–10:56: building parts and furniture made in Hyper3D Rodin.
- 11:15–12:38:
  - hit sounds for punches and shots;
  - an island sculpted with Landscape, with beaches and Unreal's ocean;
  - one 30-second dance;
  - music and animal sounds from WeLoveIndies;
  - visible tracers on every shot;
  - a circular progress ring for every timed action.

**Problems and fixes**
- In the first multi-storey town (01:04), navigation passed 165 of 180 probes. The upper floors of three buildings stayed unconnected, and no AI went upstairs in the test match. On day 3 the town was rebuilt from the entrant's Rodin parts. Navigation probes then passed on all seven large buildings (11-15 00:43).
- With the forest, the first full cook made a 1,031,280,505-byte ZIP, over the 1 GB limit. Capping the world texture groups at 2048 brought it to 973,590,139 bytes, and every plant species and tree stayed in.

## Day 3 — 11-14 13:00 → 11-15 13:00

**Built**
- 13:38 to 15:46:
  - the boombox and animal cries;
  - airdrops with the disco ball;
  - buildings at their original model size;
  - a Landscape island in Unreal's ocean.
- Evening: the entrant's ten Rodin props in the game, with HUD icons made from their reference images.
- 23:16 to 01:24:
  - a lossless compression setting that brought the ZIP to 910,484,253 bytes;
  - Megascans terrain;
  - punches that hit every enemy in front;
  - stone roofs;
  - animal hit patterns for friendship bullets;
  - the entrant's 30-second dance.
- 01:06 to 04:54:
  - physics for the hair, cat ears and tail;
  - roads painted into the terrain;
  - walk and run animations for the cat and dog, copied from the tiger;
  - a combined build that passed three launch tests.
- 05:21–07:07: the entrant's airdrop crate model, in a packaged build.
- From 07:31: at the entrant's request, Claude and Codex worked in a 15-minute review-and-fix loop until the entrant stopped it. Fixed in that loop (details below): shots, the upper-body lean, punches, sprint and aim, and the lowered gun when not aiming. The deer's cry was connected. The wind on the plants was restored after one more decision from the entrant.

**The entrant's decisions** (a selection)
- 13:21–13:33:
  - music plays only from a contestant who has the speaker;
  - airdrops first.
- 22:41–23:44:
  - punches hit every enemy in front and knock them back;
  - the order of the remaining features;
  - reload only with an empty magazine;
  - roads drawn on the ground instead of tiles.
- 00:06–00:41: finished the dance in Cascadeur, separated the hair, and made the physics assets.
- 07:18–07:29: reported that shots hit the shooter's own head, that only one punch animation played, that there was no sprint or aim, and that the upper body leaned to one side in PIE. Then asked for a timed loop, for Claude and Codex "until the problems are fixed" (translated).
- Morning: a sprint lasts at most 3 seconds, followed by a 2-second cooldown. Letting go earlier starts no cooldown.
- 11:17: the AI's roles in each team:
  - two **evasive** contestants: follow teammates, hide to be revived, and fetch items as animals;
  - two **aggressive** ones: ping nearby enemies as animals.
  - Every AI animal first carries a wall to a dancing teammate, and the wall blocks shots.
- 11:40: the feature freeze planned for 13:00 moved to when this batch is done ("extend it until it's done", translated).
- 12:30–12:51:
  - a countdown bar for the sprint, which goes grey during the cooldown;
  - evasive AI guard the player, aggressive AI roam wider, and each AI team has a commander that the others centre on;
  - the wind turned on;
  - wild animals wander and pause.
  - The entrant asked Claude to think as a player about how the AI could make the game more fun and more of a challenge. Of Claude's six proposals the entrant first picked three: enemies go for dancers first; a contestant low on health retreats; the AI gets sharper as the zone closes.
  - Checked in PIE at 13:24 (task I): the sprint bar, the roles, guards and commander, the wandering, and enemies going for dancers. The retreat stopped after half a metre; it was fixed afterwards (`6c7f3ee`) and passed the recheck in task J (day 4).

**Problems and fixes in the loop**
- **Shots stopped at the muzzle.** The shot and aim traces counted an invisible PCG volume as a wall. Now they keep only hits on things that actually block (C++ `782f6cb`).
- **The upper body leaned.** The lean came from the upper-body animation layer. The animation Blueprint now has a C++ parent class that blends between a relaxed set and an aiming set; the gun is lowered when not aiming and raised for a moment on hip fire (`e846089`, `f2c51f7`).
- **Only one punch animation.** The code had one punch slot, and later a cooldown dropped the third punch. Now there are three punches in turn, and a press during a punch is kept for the next one (`9c04330`, `d0b7228`, `f2c51f7`).
- **Wind on the plants.** Three attempts through the Editor tools failed:
  - a format mismatch;
  - an Editor crash while writing a material's pin list;
  - a version withdrawn after an incomplete check.
  - A fourth version added about 2.4 ms of GPU time per frame, over the limit set for the check, so it was switched off. At 12:42 the entrant chose to turn it on.
- **Triangle fragments in the foliage under DirectX 11.** Codex found that these are Nanite fallback meshes. On day 4 the entrant chose to leave them as they are.

## Day 4 — 11-15 13:00 → 11-16 03:00 (internal target)

**Built and checked**
- 13:00–13:36: the AI changes the entrant picked (below), in C++. Codex checked them in PIE in tasks J, K and L (13:30–14:50, task G-20261115-26):
  - the small animal's buttons: passed;
  - AI contestants go towards gunfire they hear: passed;
  - a contestant at 30% health retreated from 14 m to 97 m, stopped out of the enemy's sight behind a wall, then used its healing items: passed (the day-3 fix);
  - small animals harass the enemy: an AI animal shoved the player's animal, and one that came up 6 m behind a human went round and stopped 1.4 m in front of them, jumping. Passed after one fix (`c5a633a`);
  - AI animals carrying walls to a dancing teammate: the two facing the enemies stood 7.4–7.5 m out, the third 3.3 m away, and both shots fired at the teammate's feet hit a wall. Passed after one fix (`c5a633a`);
  - AI teammates go to the player's ping: fixed twice (`c5a633a`, `a51b115`). In the last check both aggressive teammates set off at once; one got within 12.7 m and the other stopped at 19.4 m, for a reason not found. Under the project's rules, a fix that fails twice goes back to the entrant; at 14:57 the entrant accepted it as it is.
  - Not verified: AI fights at 20–40 m in a natural match (no enemy came into range in the sample).
- 15:44–17:14: at the entrant's request, Codex changed the scene:
  - **Houses** (task G-20261115-28): roofs matched to the entrant's reference images: 19 narrow houses with a single-ridge roof, and 13 large ones with a flat roof, railings and stairs up to it. Each of the 17 houses with upper floors got an entrance facing its stairs (15 new doorways). A player start that sat inside a building was moved. An AI-driven contestant walked up to the top floor or the roof in all 17 houses, and the player went in, up, down and out in three of them.
  - **Terrain** (task G-20261115-29): the six ground surfaces repeat less visibly. Their 19 texture samples go through the engine's own texture-variation function. Measured on packaged builds: 0.04–0.05 ms more GPU base-pass time per frame.
- 17:32: the conversation record was exported for this log.
- 17:38–17:55: **the final package**, from commit `5819e2e`:
  - ZIP 931,409,638 bytes, under the 1,000,000,000-byte limit. BuildCookRun took 147 s and compression 28 s.
  - One 60-second smoke test each: the default start with the menu, direct entry on DirectX 12, and direct entry on DirectX 11. All three passed, with no engine error, ensure or fatal entry and no AI "stuck" line.
  - The forest view used for the wind measurement, at 1920×1080, Epic quality, VSync off and the wind on, on DirectX 12: 10.4 ms per frame (about 96 FPS), GPU 9.7 ms. DirectX 11 was not measured.
  - At 18:03 Claude checked the ZIP's size and SHA-256 and accepted it. These are short checks: a full match and a friend's computer were not checked on this build.
- 18:18: Codex filled in the dev log's opening fields and sorted its entries under the day headings (task G-20261115-27). Claude checked the asset declaration against the final build's list of cooked packages: every sourced asset in the build is declared.
- 18:39, and again after 18:48: the conversation record was exported. The last export, made after the entrant's last answers at 18:48, is the published one; later messages are not in it.

**The entrant's decisions** (a selection)
- 13:00–13:02: picked Claude's other three AI proposals: AI teammates go to the player's pings; the AI goes towards gunfire it hears; small animals harass the enemy. Swapped the small animal's buttons: left shoves, right pings.
- 13:35–13:36: AI animals carrying walls stand 3–8 m out towards the enemy and share out the sides round the teammate, so the gap under the wall does not leave the teammate's feet open.
- 13:40: asked Claude to start the English dev log and the asset list.
- 14:08: WeLoveIndies' free license asks for its trailer at the start of the game. The entrant (translated): "The jam rules come before WeLoveIndies. Putting their trailer at the start does not seem to fit the jam rules." The game does not show it.
- 14:27–14:32: the dev log is to be posted on itch.io. In the conversation record, passages about another project and private matters are hidden, and each one is marked.
- 14:57: accepted the AI's response to pings as it is.
- 14:58: asked for the final package. Before it was made, the entrant had Codex change the houses and the terrain (above), and at 17:16 said it could be packaged now.
- 18:15: chose "Accept as is" for the triangle fragments in the foliage under DirectX 11.
- 18:35: the full record (this overview, `DEVLOG.md`, the conversation record and the declaration) goes on GitHub.
- 18:48: chose not to hide the three mentions of another project in `DEVLOG.md`, and accepted this overview.

**Still to come** (at the time of writing)
- The entrant: the GitHub repository for the full record, the gameplay video, a friend's test of the itch.io download, the itch.io page and the submission (internal target 03:00 on 11-16; the jam closes at 09:59).

## Features: where they stand

The list is the entrant's (`DESIGN.md` section 6). "Features may end up unfinished, but none is cut": unfinished items stay on the list. "PIE" means Codex checked the feature in Play-In-Editor and Claude reviewed the report; the matching entry in `DEVLOG.md`, found by the task name, holds the evidence. Packaged builds were checked with the smoke test (the game starts and runs for a fixed time without crashes or engine errors). Those results, and the packaged checks of single features, are in `DEVLOG.md`.

| # | Feature | Status (2026-11-15, final build `5819e2e`) |
| --- | --- | --- |
| 1 | Move, shoot light shots, theoretical health; a ring of particles on hit | PIE (G-20261112-03, G-20261112-08). Punches: G-20261114-06, G-20261115-26; sprint and aim: G-20261115-26 |
| 2 | Knocked out → turn into a small animal that can jump and can't be hurt | PIE (G-20261112-03; the animal models G-20261113-25) |
| 3 | A teammate's 30-second dance revives every knocked-out teammate | PIE (G-20261113-10; the entrant's dance G-20261114-18) |
| 4 | AI contestants on the same rules, 10 teams of 5 | PIE (G-20261113-07, G-20261113-08). The day-3 roles: PIE (G-20261115-26, task I). The day-4 additions: PIE (G-20261115-26, tasks J–L); going to the player's ping reached it in one of two cases, which the entrant accepted. AI fights at 20–40 m in a natural match: not verified |
| 5 | Everything is a magazine | PIE (G-20261113-05) |
| 6 | Shrinking zone, elimination, victory | PIE (G-20261113-12) |
| 7 | Pick animal ears before the match; ears and tail swappable | The pre-match screen, where each contestant picks its animal form and music: PIE (G-20261113-17). Swapping the ears and tail: **not finished** (the entrant: "if there is time") |
| 8 | Small animals ping enemies | PIE (G-20261113-15, G-20261113-20). The player's animal pings with the right button: PIE (G-20261115-26, task J) |
| 9 | Small animals carry one item at a time, airdrops included | PIE (G-20261113-20) |
| 10 | Small animals block shots for teammates | Shots stop on a small animal in the way: PIE (G-20261113-15). An AI animal moving between on its own: partly verified (same task). AI animals carrying walls to block: PIE (G-20261115-26, tasks J and K) |
| 11 | Small animals shove each other | PIE (G-20261113-15). AI animals shove the other team's animals: PIE (G-20261115-26, task J) |
| 12 | Wild animals: petting gives friendship bullets; hits show that animal's pattern and cry | PIE: petting G-20261114-03; cries G-20261114-11; patterns G-20261114-18; the deer's cry G-20261115-26 |
| 13 | Petted animals follow in a line | PIE (G-20261114-03) |
| 14 | Turn into a small animal on purpose and call the same kind of wild animals | PIE, controlled checks (G-20261114-18) |
| 15 | Airdrops with a disco ball and a speaker; each cuts 10 s off the dance | PIE (G-20261114-13, G-20261114-18); the crate model in a packaged build (G-20261115-24) |
| 16 | A small animal hit many times scratches or licks itself, with a resigned look | **Not finished**: the animal models have no such animations |
| 17 | The disco ball over the dancer | PIE (G-20261114-13) |
| 18 | The speaker plays the chosen music, pulsing to the beat | PIE (G-20261114-11) |
| 19 | Choose the music before the match | PIE (G-20261113-17) |
| 20 | The four nearest animals dance along | Implemented (C++ `43e967d`); **PIE evidence not located** |
| 21 | Bullet tiers by colour (wood, stone, iron, friendship) | PIE (G-20261113-05; friendship bullets G-20261114-18) |
| 22 | The contestant model | In the game (G-20261113-18; made in Tripo by the entrant) |
| 23 | Animal models | In the game (G-20261113-25; a Fab pack chosen by the entrant) |
| 24–27 | Healing items, armour and shield tiers, shield recharges, pet treats | PIE (G-20261114-04, G-20261114-18) |

**Still open at the time of writing**
- The airdrop's balloon is a white placeholder sphere.
- Eight item models have no Nanite data and are drawn from their fallback meshes.
- Under DirectX 11 the trees are drawn from their Nanite fallback meshes, which show large triangle fragments; the entrant chose to leave this. DirectX 12, the default, is not affected.
- An AI teammate may stop short of a ping placed high on a wall; the entrant accepted this.
- Some AI contestants still get stuck now and then; each case is logged. The final build's three 60-second smoke tests logged none.
- Not checked on the final build: a full match, and a friend's computer.

## How the AI tools were used

- **Claude Code** wrote the game's own C++, the design record and the task instructions, and reviewed Codex's work. It ran on Claude Opus 5.5, and on Claude Fable 5.2 from about 06:00 to 15:30 on 11-12 and 12:10 to 19:02 on 11-13 (from the model named in each commit).
- **Codex** did the Editor work through Unreal MCP and checked each feature in PIE.
  - MCP calls go through the plugin's toolsets; UE 5.9 lists 105 of them. Codex also used the Editor's Python scripting, and took screenshots to look at its own work.
  - Codex also built, packaged and smoke-tested the game.
- **OpenAI Images API** (`gpt-image-2`; requested by Codex with the entrant's key):
  - the contestant concept images;
  - reference images for the buildings, furniture, props and airdrop crate;
  - the animal patterns used on friendship-bullet hits.
  - The props' reference images also serve as the HUD item icons.
- **Tripo**: the contestant. The entrant built it in Tripo Studio's character template, assembled by Tripo's own AI agent from the chosen concept image.
- **Hyper3D Rodin**: buildings, furniture, props and the airdrop crate, made by the entrant from the reference images.
- **Not generated:** the animals, plants, terrain surfaces, music and sounds are sourced assets, listed with their licenses in the declaration ([`DECLARATION.md`](../DECLARATION.md)).

## Rules the entrant set for the agents

- "Features may end up unfinished, but none is cut." Agents may not drop a feature or replace it with a simpler version.
- The design is the entrant's. Agents record the entrant's words as given, and ask before making a proposal.
- Every design is judged as if it were in the multiplayer game; nothing is built that only makes sense in single-player.
- Every conversation goes into this dev log, in the original Chinese.
