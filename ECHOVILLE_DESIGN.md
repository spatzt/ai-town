# EchoVille on AI Town — Living Design Document

> **Purpose:** Keep one evolving source of truth for what AI Town already provides, what EchoVille wants to preserve from earlier prototypes, what we need to change, and what belongs on the future pin board.
>
> **Working rule:** Mod first. Extend second. Replace solved systems only when EchoVille actually needs something different.

## Status key

- **AI TOWN** — already exists in the base project.
- **KEEP** — use largely as-is.
- **MODIFY** — AI Town has the system, but EchoVille needs different rules/data/UI.
- **ADD** — not present in the needed form yet.
- **PIN BOARD** — future implication; explicitly outside the first playable slice.
- **OPEN** — design decision still to make.

---

# 1. Current project state

## Development foundation

- **AI TOWN / KEEP:** Forked from `a16z-infra/ai-town` into `spatzt/ai-town`.
- **AI TOWN / KEEP:** Convex backend, realtime database, simulation engine, vector search, and scheduled/background functions.
- **AI TOWN / KEEP:** PixiJS / React browser renderer.
- **AI TOWN / KEEP:** GitHub Codespaces can be the main development machine for phone-first work.
- **CURRENT SETUP:** Codespace created and dependencies installed. Convex account login/setup is the next installation step.
- **KEEP:** AI Town code is the simulation chassis; EchoVille supplies its own characters, world, art, rules, and identity.
- **KEEP:** We will generate our own EchoVille imagery rather than relying on AI Town's stock visual identity.

## Existing EchoVille design work worth carrying forward

The earlier EchoVille Terrarium prototype remains a **design reference**, not wasted work.

- **ADD:** Protected Canon — immutable character/world truths.
- **ADD:** Objective event history — what actually happened.
- **ADD:** Character interpretation — how each participant remembers/understands the same event.
- **ADD:** Structured relationships with six dimensions:
  - affection
  - trust
  - comfort
  - respect
  - irritation
  - tension
- **ADD:** Semantic locations — locations have purposes/activities, not just graphics.
- **ADD:** Player nudges/suggestions rather than omnipotent NPC control.
- **ADD:** Story seeds — interesting events can become possible future scenes without automatically resolving them.

---

# 2. What AI Town can already do

## World and simulation

- **AI TOWN / KEEP:** Persistent server-side worlds.
- **AI TOWN / KEEP:** Game state advances continuously while the world is active.
- **AI TOWN / KEEP:** 60 simulation ticks per second with lower-frequency persisted steps for smooth motion without writing the database every frame.
- **AI TOWN / KEEP:** Server-authoritative game state. Humans and agents submit inputs; the game engine decides and applies valid state changes.
- **AI TOWN / KEEP:** World state, players, agents, conversations, maps, messages, and memories are stored in Convex.
- **AI TOWN / KEEP:** The world can be paused/stopped, resumed, archived, wiped, and reinitialized for development.
- **AI TOWN / MODIFY:** The default world idles/stops after inactivity. We will decide later whether EchoVille should remain active longer or simulate catch-up differently.

## Map, movement, and collision

- **AI TOWN / KEEP:** Tile-based world map.
- **AI TOWN / KEEP:** Background tiles and object/collision layers.
- **AI TOWN / KEEP:** A* style pathfinding across walkable tiles.
- **AI TOWN / KEEP:** Characters move smoothly rather than teleporting.
- **AI TOWN / KEEP:** Collision detection with the environment and other characters.
- **AI TOWN / KEEP:** Characters can replan/wait when another character blocks their route.
- **AI TOWN / KEEP:** Facing/orientation is tracked and characters face conversation partners.
- **AI TOWN / MODIFY:** EchoVille will replace the stock map and tiles with our own map and imagery.
- **AI TOWN / KEEP:** Tiled maps can be converted into AI Town's map format with the included map conversion script.

## Human player mechanics already present

- **AI TOWN / KEEP:** Human players can join and leave a world.
- **AI TOWN / KEEP:** Human-controlled movement to a selected destination.
- **AI TOWN / KEEP:** A human can select another resident.
- **AI TOWN / KEEP:** A human can invite another resident to a conversation.
- **AI TOWN / KEEP:** A human can accept or reject a conversation invitation.
- **AI TOWN / KEEP:** A human can leave a conversation.
- **AI TOWN / KEEP:** Human/NPC conversation messages are supported.
- **AI TOWN / KEEP:** Previous conversation history can be displayed.
- **AI TOWN / KEEP:** AI Town supports multiple human players, although EchoVille is currently conceived primarily as a private personal world.

## NPC movement and activity behavior already present

- **AI TOWN / KEEP:** NPC agents act without the player directing every action.
- **AI TOWN / KEEP:** NPCs can wander to destinations on their own.
- **AI TOWN / MODIFY:** NPCs currently choose from a very small hard-coded activity list:
  - reading a book
  - daydreaming
  - gardening
- **AI TOWN / MODIFY:** Default activities are random rather than strongly tied to identity, jobs, location, schedule, or relationships.
- **AI TOWN / MODIFY:** EchoVille needs location-aware and character-aware activity selection.

## NPC-to-NPC conversations already present

- **AI TOWN / KEEP:** NPCs can autonomously decide to start conversations.
- **AI TOWN / KEEP:** A conversation invitation has explicit states:
  - invited
  - walking over
  - participating
- **AI TOWN / KEEP:** Characters physically walk toward one another before conversation begins.
- **AI TOWN / KEEP:** Conversation participants stop and face one another.
- **AI TOWN / KEEP:** Agents can accept or reject invitations.
- **AI TOWN / MODIFY:** Agent-to-agent acceptance is currently probability-based, not personality/relationship-aware.
- **AI TOWN / KEEP:** Cooldowns prevent constant repeated conversations.
- **AI TOWN / KEEP:** Agents wait for the other participant rather than constantly talking over each other.
- **AI TOWN / KEEP:** Typing state is represented.
- **AI TOWN / KEEP:** Conversations have maximum duration/message limits and agents can end them.
- **AI TOWN / LIMITATION:** Conversations currently support exactly two participants.
- **ADD / LATER:** Group conversations.

## Character prompting already present

Each AI agent currently has:

- **AI TOWN / MODIFY:** a name
- **AI TOWN / MODIFY:** a sprite/character reference
- **AI TOWN / MODIFY:** an identity description
- **AI TOWN / MODIFY:** a plan / goal

Conversation prompts already include:

- the resident's identity
- their current conversational goal/plan
- information about the other AI resident
- prior conversation timing
- relevant retrieved memories
- the current conversation history

EchoVille will replace the stock identities/plans with richer character data and stronger canon boundaries.

## LLM support already present

- **AI TOWN / KEEP:** LLM work happens outside the deterministic simulation loop.
- **AI TOWN / KEEP:** Agents schedule one asynchronous operation at a time for long-running work.
- **AI TOWN / KEEP:** Conversation generation is handled as an agent operation and returns through controlled game inputs.
- **AI TOWN / KEEP:** Configurable LLM providers.
- **AI TOWN / KEEP:** Ollama support.
- **AI TOWN / KEEP:** OpenAI support.
- **AI TOWN / KEEP:** Together.ai support.
- **AI TOWN / KEEP:** Other OpenAI-compatible endpoints can be configured.
- **KEEP:** The model should propose dialogue/meaningful decisions; deterministic game code remains authoritative over physical world state.

## Memory already present

- **AI TOWN / KEEP:** Conversation history is archived.
- **AI TOWN / KEEP:** After a conversation, each AI participant can generate a first-person summary from its own perspective.
- **AI TOWN / KEEP:** Memories receive an importance score.
- **AI TOWN / KEEP:** Memories are embedded and stored for vector search.
- **AI TOWN / KEEP:** Relevant memories can be retrieved for later conversations.
- **AI TOWN / KEEP:** Recency, relevance, and importance can influence memory retrieval.
- **AI TOWN / KEEP:** Agents can generate reflection memories/insights from existing memories.
- **AI TOWN / KEEP:** Retrieved memories are currently explicitly treated as untrusted historical data rather than instructions.
- **MODIFY:** EchoVille should not let generated summaries become the sole record of reality.

### EchoVille memory target

**Protected Canon → Objective Event → Character Interpretation → Searchable Memory**

AI Town's vector retrieval is useful. EchoVille's factual/subjective separation should sit underneath it.

---

# 3. First EchoVille playable cast

## Spatz

- **Role:** Player resident and apartment manager.
- **Player fantasy:** Spatz has a practical responsibility for the building, but is not the mayor, chosen hero, or social authority over the residents.
- **Core:** creative, curious, playful, observant.
- **Player control:** movement, conversation, exploration, invitations/suggestions.
- **Building role:** Gives Spatz a natural reason to know the residents, move through shared spaces, notice problems, and interact with the building without making everyone revolve around her.
- **OPEN:** How much apartment-management work is simulated in the first slice versus kept as light flavor.
- **OPEN:** Whether Spatz also receives AI-authored interpretation/memory assistance or remains purely human-controlled in the first build.

## Chad

- Strong-willed, passionate, competitive, direct, stubborn, dependable.
- Fiercely protective of people and EchoVille.
- Favors action and conviction over extended analysis.
- Close friend-rival relationship with Skylar.
- Future work anchor: Training Hall.

## Skylar

- Librarian / digital archivist.
- Thoughtful, observant, analytical, curious, approachable, quietly stubborn.
- Strong interest in computers, research, archives, and inconsistencies.
- Close friend-rival relationship with Chad.
- Work anchor: EchoVille Library.

## Kevin

- Adult trans man.
- Barista, baker, and cook.
- Warm, observant, loyal, anxious, creative, teasing, quietly protective.
- Often a grounding influence among friends.
- Work anchor: The Wired Bean.

---

# 4. Locations

## First-world target: the apartment building

The first playable EchoVille is intentionally compact: one apartment building that functions as a miniature social world. Every core resident has a private apartment, with shared spaces that naturally create routines and chance encounters.

### Spatz's apartment
**Purpose:** Spatz's private home and player starting space.

### Chad's apartment
**Purpose:** Chad's private retreat.

### Skylar's apartment
**Purpose:** Skylar's private retreat.

### Kevin's apartment
**Purpose:** Kevin's private retreat.

### Lobby / hallways / common seating
**Purpose:** neutral circulation and casual social space.

Possible activities:
- pass through
- wait
- talk
- sit
- check on the building

### Building coffee shop
**Purpose:** café, work anchor, and main social hub.

Important behavior:
- Kevin has a real work relationship to the coffee shop.
- Residents can visit for food, drink, breaks, or company.

Possible activities:
- work
- bake/cook
- make coffee
- eat
- drink
- sit
- talk
- decompress

### Building gym
**Purpose:** exercise, training, sparring, and Chad's natural anchor.

Possible activities:
- exercise
- train
- spar
- stretch
- cool down
- talk

### Shared computer space
**Purpose:** computers, research, digital projects, and Skylar's natural anchor.

Possible activities:
- research
- use computers
- read
- work on projects
- troubleshoot
- talk quietly

### Apartment manager space / building tasks
**Purpose:** Gives Spatz a practical relationship to the whole building.

Potential early uses:
- check shared spaces
- notice maintenance issues
- talk with residents
- coordinate access or simple building concerns

Keep this lightweight in the first slice unless management gameplay proves fun.


### Apartment inventories
**ADD:** Each apartment should have its own persistent inventory, separate from the resident's personal inventory.

This lets the simulation distinguish between:
- items carried by a resident
- items owned by a resident but stored at home
- shared/apartment fixtures and furnishings
- building-owned items in common spaces

Initial apartment inventories:
- Spatz's apartment inventory
- Chad's apartment inventory
- Skylar's apartment inventory
- Kevin's apartment inventory

Examples of apartment inventory:
- furniture
- appliances
- books/media
- tools
- decorations
- stored food/supplies
- personal belongings not currently carried

**Design rule:** AI dialogue may reference apartment possessions, but code/database state remains authoritative about whether an item actually exists.

## Later world expansion

These remain EchoVille destinations for later growth rather than first-map requirements:
- The Wired Bean as a larger standalone café
- EchoVille Library / digital archive
- Training Hall
- Abandoned Amusement Park
- Annex-related locations

## Explicitly not current

- **No Quarry Lake in the current EchoVille plan.**

---

# 5. Player mechanics: EchoVille target

## What the player should be able to do

### Movement and presence
- **KEEP:** Walk around the map as Spatz.
- **KEEP:** Enter social proximity naturally rather than teleporting into scenes.
- **ADD:** Clear location/interior transitions as the map grows.

### Conversations
- **KEEP:** Select a resident and start a conversation.
- **KEEP:** Accept/reject invitations.
- **KEEP:** Leave conversations.
- **MODIFY:** Character voice and canon must come from EchoVille data, not a single loose prompt.

### Suggestions / nudges
The player should influence, not puppet.

Examples:
- "Want to go to the Wired Bean?"
- "Come check out the park with me."
- "You should talk to Skylar."
- "Want to train?"

**Target behavior:** The NPC may accept, decline, postpone, or counter depending on:
- current activity
- schedule
- mood/state
- relationship
- location
- personality

A successful suggestion becomes an ordinary game intention/input, not teleportation.

### Observation
- **AI TOWN:** Active and previous NPC conversations can be surfaced in the UI.
- **OPEN:** Decide how much NPC↔NPC conversation Spatz can read when she was not physically present.
  - Terrarium/observer mode may allow it.
  - Diegetic mode may keep private conversations private.
  - Could support both as UI modes later.

### Exploration
- **ADD:** Interactable location points/objects.
- **ADD:** Investigate/examine actions for places such as the abandoned park.
- **ADD:** Story seeds can emerge from exploration without forcing a quest resolution.

---

# 6. NPC AI / mechanics: EchoVille target

## Keep from AI Town

- autonomous movement
- autonomous conversation invitations
- physical walk-over before talking
- one substantial async AI operation at a time
- conversation turn-taking/cooldowns
- LLM-generated dialogue
- persistent conversation records
- vector memory retrieval
- reflection capability

## Modify

### Daily decisions
Current AI Town:
- wander randomly
- perform one of a few random activities
- sometimes approach a free resident

EchoVille target:
1. Check obligations/schedule.
2. Check current location and available activities.
3. Consider needs/preferences/state.
4. Consider nearby people and relationships.
5. Consider recent/relevant memories.
6. Choose among:
   - work
   - personal activity
   - visit a location
   - approach somebody
   - accept an invitation
   - remain alone
   - investigate something interesting

The LLM should not need to decide every footstep.

### Jobs and schedules
- **ADD:** Kevin works at The Wired Bean.
- **ADD:** Skylar works at the Library.
- **ADD:** Chad has Training Hall duties.
- **ADD:** Work hours/routines should influence destination and activity selection.
- **ADD:** Characters still have discretionary time and can deviate when something meaningful happens.

### Semantic locations
- **ADD:** Location tags and available activities should be machine-readable.
- **ADD:** NPC decisions should understand that a café, library, training hall, and abandoned park offer different possibilities.

### Relationships
- **ADD:** Directed six-dimensional relationship state.
- **ADD:** Relationship state should influence invitations, reactions, memory interpretation, and conversational tone.
- **KEEP:** Relationships do not erase established canon labels such as Chad/Skylar being close friend-rivals.

### Mood/state
- **ADD:** Lightweight temporary state/mood.
- **OPEN:** Decide how much mood is numeric versus descriptive.


### Inventory and currency foundation
- **ADD:** Persistent personal inventory for each resident.
- **ADD:** Persistent inventory for each apartment.
- **ADD:** Currency/wallet support for residents when needed.
- **ADD:** Building-owned inventory for shared spaces can be added when apartment management begins.
- **RULE:** The LLM can read relevant inventory/currency facts but cannot invent, grant, spend, transfer, or delete items/money without validated game actions.

### Memory
- **MODIFY:** Preserve objective facts separately from AI interpretations.
- **MODIFY:** Each character retrieves only information they legitimately know.
- **KEEP:** Search relevant subjective memories semantically for conversation context.
- **ADD:** Canon validation before generated dialogue/events become accepted history.

### Conversation partner choice
Current AI Town largely relies on availability, cooldowns, and simple candidate selection.

EchoVille target should eventually include:
- relationship
- recency
- current purpose
- personality
- shared location/activity
- unresolved social tension
- curiosity
- desire for solitude

---

# 7. Visual / asset plan

## Replace

- stock AI Town character sprites
- stock map
- stock environment tiles
- stock UI identity where appropriate
- stock character names/lore
- stock activity flavor

## EchoVille art pipeline — OPEN SPECS

We still need to lock:

- base tile size
- character sprite dimensions
- walk-cycle frame layout
- idle/facing frames
- building scale
- interior map strategy
- portrait dimensions, if portraits are used
- interaction icons
- map/tileset palette
- mobile readability

**Rule:** Generate/paint assets to the engine's actual expected format rather than making finished art first and forcing the engine around it afterward.

---

# 8. First playable vertical slice

The first slice should prioritize visible life over architectural completeness.

## Target

- Spatz as the player and apartment manager.
- Chad, Skylar, and Kevin as autonomous AI residents.
- Each of the four has a private apartment.
- Shared spaces: lobby/halls, coffee shop, gym, and computer space.
- Our own basic sprites and apartment-building art.
- NPCs walk around under AI Town's existing movement system.
- NPCs initiate conversations without Spatz.
- Spatz can approach and converse with them.
- Kevin visibly uses the coffee shop as his work anchor.
- Skylar naturally uses the computer space.
- Chad naturally uses the gym.
- Spatz has lightweight manager reasons to move through and check on the building.
- NPC dialogue uses EchoVille character identities.
- Conversations become memories.
- Basic EchoVille memory/canon safeguards begin replacing stock memory assumptions.

## "It feels alive" success test

Open the town and, without scripting the scene by hand:

1. Kevin heads to the coffee shop and works.
2. Skylar spends time in the shared computer space.
3. Chad uses the gym, then moves elsewhere on his own.
4. Residents cross paths in the lobby, halls, or shared spaces.
5. Two NPCs meet and have a character-appropriate conversation without Spatz initiating it.
6. Later dialogue can reference that earlier interaction.
7. Spatz can move through the building as manager/resident and participate without becoming the center of every relationship.

---

# 9. Pin board — future implications

These matter, but they do **not** block the first playable EchoVille.

## Digimon companions
**PIN BOARD.**

Future systems may include:
- partner/follower actors
- companion tethering/follow behavior
- companion autonomy
- partner-specific communication
- companion participation in scenes
- Mewamon speaking/interjecting
- Vorvomon's nonverbal communication
- Jinxie's autonomous mischief

For now, do not build companion mechanics into the first slice.

## Larger cast
Later additions:
- Kyle
- Kenneth
- Brae
- Candice
- others as EchoVille expands

## Social expansion
- group conversations
- gatherings/events
- richer invitations
- conflict/reconciliation patterns
- private vs observable conversations

## World expansion
- interiors
- homes
- additional businesses/civic spaces
- Annex systems
- restoration/change over time
- offline/catch-up simulation

## Apartment management economy
**PIN BOARD / LATER MANAGEMENT PHASE.**

When apartment-management gameplay is added, support:
- a separate **building budget**
- **rent** paid by residents/units
- building income and expenses
- repairs and maintenance costs
- shared-space upgrades/furnishings
- clear separation between Spatz's personal money and building funds

**Design rule:** Building money belongs to the property/system, not automatically to Spatz's personal wallet.

---

# 10. Development order

## Milestone 0 — Vanilla AI Town
- Finish Convex login/setup.
- Run stock AI Town.
- Confirm movement, agents, conversation, and memory work in our fork.
- Make a clean checkpoint before EchoVille modifications.

## Milestone 1 — Visible EchoVille reskin
- Rename project-facing identity.
- Replace one stock resident with Spatz.
- Replace one sprite.
- Replace/rename one visible location.
- Confirm we can immediately see modifications in the running world.

## Milestone 2 — Core four
- Spatz player.
- Chad.
- Skylar.
- Kevin.
- Remove stock residents from the active world.
- Add core character identities and voices.

## Milestone 3 — Daily life
- Add semantic location data.
- Add Wired Bean / Library / Training Hall routines.
- Replace random generic activities with location/character-aware choices.

## Milestone 4 — EchoVille social memory
- Add Protected Canon.
- Add objective event records.
- Add character-specific interpretations.
- Keep AI Town vector retrieval on top of those interpretations.
- Add structured relationships.

## Milestone 5 — Player influence
- Add invitations/nudges for places and activities.
- NPC can accept/decline based on state rather than being directly commanded.

---

# 11. Questions to answer as we build

- Is Spatz always human-controlled, or can she optionally become an AI resident when the player is away?
- Should the player be able to read NPC conversations they were not present for?
- Do buildings use seamless map zones or separate interior maps?
- How much schedule should be fixed versus emergent?
- Which activities are deterministic and which should involve an LLM?
- What is the minimum relationship system needed before the first AI conversation feels like EchoVille?
- How long should the town continue running while nobody is watching?
- What should the player be able to directly request versus merely suggest?
- When does a generated interaction become accepted objective history?
- What information is allowed to cross between characters' private memories?

---

# 12. Guiding principles

1. **Characters remain themselves.**
2. **The town exists even when Spatz is not the center of a scene.**
3. **Code owns physical reality; AI supplies interpretation, dialogue, and bounded decisions.**
4. **Memory is not automatically truth.**
5. **Locations have semantic purpose.**
6. **Player influence is social, not omnipotent.**
7. **Visible fun beats rebuilding solved infrastructure.**
8. **If it does not help the first four residents walk, talk, remember, and live in EchoVille, consider putting it on the pin board.**
