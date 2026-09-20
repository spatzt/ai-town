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
- Forest Outskirts / trail network
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

# 12. Future pass — town building and lore

**PIN BOARD / AFTER APARTMENT MODE.**

The apartment building is the seed of EchoVille, not a disposable tutorial map. Town expansion should grow outward from the routines, relationships, and systems already proven inside the building.

## Master location inventory from prior EchoVille work

This catalog intentionally includes current locations, Redbean experiments, older planning, prototype locations, and broader Echofield spaces. Inclusion here does **not** make a place current canon. The point is to preserve useful geography before we choose the eventual Stardew-like town layout.

| Location / location family | Source/status | Routine purpose | Character association / notes |
| --- | --- | --- | --- |
| **Apartment Building** | **CURRENT BASE** | home hub, daily-life nucleus | First playable EchoVille. Spatz manages the building. |
| Spatz's Apartment | **CURRENT BASE** | home/private retreat | Spatz |
| Chad's Apartment | **CURRENT BASE** | home/private retreat | Chad |
| Skylar's Apartment | **CURRENT BASE** | home/private retreat | Skylar |
| Kevin's Apartment | **CURRENT BASE** | home/private retreat | Kevin |
| Lobby / Hallways / Common Seating | **CURRENT BASE** | crossings, casual encounters, waiting | Shared |
| Building Coffee Shop | **CURRENT BASE** | work, food, socializing | Kevin's first work anchor |
| Building Gym | **CURRENT BASE** | training, exercise, social encounters | Chad's first anchor |
| Shared Computer Space | **CURRENT BASE** | research, computers, quiet work | Skylar's first anchor |
| Apartment Manager Space / Building Tasks | **CURRENT BASE** | management, maintenance hooks | Spatz |
| **The Wired Bean** | **FUTURE / WELL-ESTABLISHED LORE** | café, work, meals, social hub | Kevin; Redbean exterior/interior work and Terrarium prototype anchor |
| **EchoVille Library / Digital Archive** | **FUTURE / WELL-ESTABLISHED LORE** | work, research, records, computers, quiet socializing | Skylar |
| **Training Hall** | **FUTURE / WELL-ESTABLISHED LORE** | work, sparring, exercise, teaching | Chad |
| **EchoVille Dispatch** | **FUTURE CANDIDATE / ESTABLISHED CHARACTER ROLE** | courier/logistics, deliveries, errands | Kyle |
| **Workshop Cottage / EchoVille Repair Garage** | **FUTURE CANDIDATE / REDBEAN THREAD** | repairs, mechanic work, tools, deliveries | Kenneth; two names/versions of the same general functional thread, final form OPEN |
| **Town Square** | **TERRARIUM PROTOTYPE** | neutral gathering/crossroads | One of the three literal Terrarium 0.2 locations |
| **Guidepost Square** | **OLDER HUB CONCEPT** | noticeboard, events, benches, NPC encounters | Possible civic-center predecessor/alternative |
| **Central Green / Flower Garden** | **REDBEAN EXPERIMENT** | outdoor leisure, strolling, socializing | Could become a civic green if retained |
| **Fountain Park** | **REDBEAN EXPERIMENT** | outdoor leisure, meetings, dates, downtime | Candidate public park |
| **Market Row** | **OLDER / CANDIDATE DISTRICT** | shopping, errands, work routes | Useful district concept |
| **EchoVille Marketplace** | **OLDER ECONOMY/SHOP CONCEPT** | shopping/economy interface | Could be physicalized, abstracted, or folded into Market Row |
| **General Store** | **OLDER CANDIDATE** | groceries, supplies, routine errands | Strong Stardew-style routine utility if revived |
| **Residential Lane / Suburban Homes** | **REDBEAN / OLDER CANDIDATE** | housing expansion, visits, home routines | Useful after the apartment building is no longer the only housing |
| **Town Edge** | **OLDER CANDIDATE** | transition space, arrivals, exploration | Good boundary/expansion concept |
| **Willow Grove** | **OLDER ECHOFIELD/ECHOVILLE CONCEPT** | nature, quiet time, walks | Candidate edge/nature location |
| **Forest Outskirts** | **LOCKED FUTURE GEOGRAPHY / ROUTINE ZONE** | foraging, walking, quiet time, exploration, route discovery | EchoVille is surrounded by forest; this is the playable transition zone between town and deeper woods |
| **Trails** | **REDBEAN EXPERIMENT** | walking, solitude, travel links | Likely folded into Forest Outskirts / trail network |
| **Abandoned Amusement Park** | **WELL-ESTABLISHED FUTURE LOCATION** | exploration, nostalgia, private talks, restoration/mystery | Terrarium anchor; future Annex material |
| **Old Fairground** | **REDBEAN PREDECESSOR/VARIANT** | same general abandoned-fairground niche | Treat as an earlier naming/design thread for the amusement-park concept unless deliberately separated |
| **Annex** | **MYSTERY / FUTURE STORY SPACE** | unusual exploration/story events | Not an ordinary everyday destination; exact nature deliberately unresolved |
| **Spatz's Base / Home** | **REDBEAN PREDECESSOR** | home, art, computers, hobby space | Function now largely absorbed by Spatz's apartment for the first slice |
| **Quarry Lake / Dockhouse** | **ARCHIVE / SUPERSEDED** | former fishing/lakekeeper/dock routine concept | **Not current EchoVille canon. Do not reintroduce automatically.** |
| Dock / lake trail material | **ARCHIVE WITH QUARRY-LAKE THREAD** | former outdoor routine space | Reuse only if deliberately redesigned away from Quarry Lake |
| **The Frame / Hollow Frame house** | **BROADER ECHOFIELD, NOT AUTOMATICALLY ECHOVILLE** | nexus/home hub | Separate broader-project location; contains living room/fireplace, kitchen, smoke room, computer lab, gym, portal door, porch/lake, bedrooms, Kyle's balcony/nest |
| **Frame Greenhouse** | **BROADER ECHOFIELD** | gardening/growing/social routines | Kevin/Mateo association in Frame lore |
| **Church of Iris / Church Relit spaces** | **BROADER ALTERIA/ECHOFIELD LORE** | spiritual/story location | Not an ordinary EchoVille town building unless intentionally imported later |

### Overlapping location families to resolve later

Several experiments solve the same routine need. We should not automatically keep every version:

- **Civic center:** Town Square / Guidepost Square / Central Green / Fountain Park
- **Commerce:** Market Row / Marketplace / General Store
- **Kenneth work anchor:** Workshop Cottage / EchoVille Repair Garage
- **Home base:** Spatz's Base / Spatz's Apartment
- **Abandoned fairground:** Old Fairground / Abandoned Amusement Park

When town-building begins, choose the smallest set that produces strong routines and recognizable geography.

## What AI Town contributes to the location system

The stock AI Town framework does **not** give us EchoVille canon locations worth preserving. Its value is structural:

- tile map rendering
- obstacle/collision layers
- pathfinding between coordinates
- autonomous movement
- activity state
- agents physically walking to conversation partners
- persistent world/player state

EchoVille must add the semantic layer that says a coordinate range is a café, apartment, gym, library, shop, park, workplace, private home, etc.

## Stardew-like routine target

The goal is **predictable life with room for AI variation**, not random wandering and not rigid scripting.

Each resident should eventually have:

- **Home:** where they begin/end most days and retreat for privacy
- **Primary anchor:** job, responsibility, or strongest recurring destination
- **Secondary anchors:** hobby/work/social spaces they favor
- **Errand pool:** shops/services they visit when relevant
- **Social pool:** places where they intentionally spend time with others
- **Quiet pool:** places they choose for solitude
- **Special/event locations:** places used only when a condition, story, relationship, or event makes sense

A routine should be built from **schedule windows + bounded choices**. Example: Kevin can reliably be found at work during a work block, but his break may resolve to the lobby, a table in the café, another resident, or home depending on state.

Future routine conditions can include:
- time of day
- day of week
- work/off day
- location opening hours
- current task
- weather once outdoor town play matters
- mood/state
- recent memories
- relationship context
- invitations
- town events
- quests/scenarios

The player should be able to learn residents' habits well enough to think, "Skylar is probably at the computer space right now," while still occasionally being surprised by a believable deviation.

### Initial routine skeleton

| Resident | Home | First primary anchor | Later town anchor | Natural secondary spaces |
| --- | --- | --- | --- | --- |
| **Spatz** | Spatz's Apartment | apartment-manager/common areas | broader manager/town responsibilities as designed | coffee shop, computer space, other shared areas |
| **Kevin** | Kevin's Apartment | Building Coffee Shop | The Wired Bean | common seating, other residents, home |
| **Skylar** | Skylar's Apartment | Shared Computer Space | EchoVille Library / Digital Archive | coffee shop, common seating, home |
| **Chad** | Chad's Apartment | Building Gym | Training Hall | coffee shop, common areas, home |
| **Kyle** | future home | — | EchoVille Dispatch | errands, civic/commercial spaces; final routine later |
| **Kenneth** | future home | — | Workshop Cottage / Repair Garage | commercial/social spaces; final routine later |
| **Brae** | unresolved | — | unresolved | Do not assign an old Quarry Lake job by default; his outsider routine should be designed from current canon later |
| **Candice** | unresolved | — | unresolved | Abandoned Amusement Park is an arrival/story location, not a current job assignment |

## Expansion philosophy

- Expand only when a new location creates new behavior, relationships, work, exploration, or story possibilities.
- Prefer a compact, walkable town over a large decorative map.
- Existing residents should gain reasons to use new places before large numbers of new NPCs are added.
- The town should feel lived in before it feels complete.
- Not every location needs a mystery or quest.
- New spaces should preserve the same semantic-location model used in the apartment building.
- The apartment building remains a home/social anchor even after the wider town opens.

## Possible expansion order

### Phase A — Apartment building
Current first playable world:
- four private apartments
- lobby / halls / common seating
- building coffee shop
- building gym
- shared computer space
- apartment-manager responsibilities

### Phase B — Immediate block
Open the front door and establish the neighborhood around the building.

Possible needs:
- sidewalk/street circulation
- a small outdoor common area
- practical everyday services
- room for residents to leave the building for short routines
- visible forest pressure at the edge of the built area


**OPEN:** Exact businesses and services should be chosen based on what the simulation actually needs after apartment playtesting.

### Phase C — Character anchor locations
Promote successful apartment-space behaviors into larger town institutions.

Known candidates:
- **The Wired Bean** — Kevin's larger standalone café/workplace
- **EchoVille Library / Digital Archive** — Skylar's civic/research anchor
- **Training Hall** — Chad's larger training/community anchor

The apartment coffee shop, computer space, and gym can function as prototypes for these systems rather than wasted content.

### Phase D — Civic / social town
Add places that support a broader resident population and daily life.

Potential categories:
- public gathering space
- local shops/services
- food
- recreation
- workspaces
- civic services
- transit/arrival point if the lore requires one

Specific locations should be added because they support characters or gameplay, not simply to fill a map.

### Phase E — Edges, history, and mystery
Open less ordinary parts of EchoVille after the town already feels normal enough for unusual places to contrast with it.

Known candidate:
- **Abandoned Amusement Park** — exploration, nostalgia, restoration, private conversations, and later mystery material

Potential later connection:
- Annex-related spaces/events

The abandoned park should remain melancholy/nostalgic rather than default horror.

## Town simulation systems that expansion may need

- semantic location registry
- schedules that span multiple buildings
- homes and workplace ownership/permissions
- town-scale inventories and storage
- shops and transactions
- public/shared inventories
- opening/closing hours
- jobs and work shifts
- town services
- additional residents
- location-specific events
- local reputation/familiarity if useful
- transport only if walking stops being sufficient

Do not add these simply because a town simulator "should" have them. Add them when the growing map creates the need.

## Core geography — forest setting

**PROTECTED CANON / FUTURE TOWN PASS**

EchoVille sits **in the middle of a forest**. The forest is not a decorative border; it is the town's immediate surrounding geography and should shape arrival, expansion, routines, and atmosphere.

The wider town should feel enclosed by trees, old trails, overgrown service roads, and partially reclaimed edges. Leaving the built-up area means entering the forest rather than immediately reaching suburbs or open farmland.

This creates a strong spatial contrast:
- **town center:** restored, inhabited, increasingly active
- **town edge:** transitional, overgrown, partially reopened
- **forest outskirts:** quieter, less controlled, older-feeling, and ideal for exploration or discovery

The forest can contain both ordinary nature and traces of previous settlement layers without making every path supernatural.

## Core town lore — rediscovery

**PROTECTED CANON / FUTURE TOWN PASS**

EchoVille is **ancient**. It is not a newly founded settlement.

The town was lost, forgotten, inaccessible, abandoned, or otherwise removed from ordinary life long enough to become something people no longer actively inhabited. It has now been **rediscovered**.

Spatz, Chad, Skylar, Kevin, and the people who follow are among the **first new residents to return and rebuild here**. Their role is not to create EchoVille from nothing, but to bring life back into a place that already has history, structures, traces, and unresolved stories.

The apartment-building phase represents the earliest practical foothold in that return: establish somewhere safe to live, restore basic routines, then gradually reopen and understand the wider town.

### The whispers

EchoVille **holds whispers**.

These whispers belong to both:
- **the ancient** — traces of people, events, customs, places, and unresolved history from old EchoVille
- **the present** — impressions, echoes, rumors, conversations, memories, and emotional residue created by the people living there now

The word **whispers** should remain intentionally broad at first. They may present as:
- half-remembered stories
- strange familiarity
- old records that do not fully agree
- names or phrases that recur
- places that seem to carry emotional weight
- fragments of prior events
- modern conversations that seem to linger
- rumors that spread and mutate
- patterns noticed by different residents
- rare uncanny moments that may or may not have an ordinary explanation

**Important:** Whispers are not automatically ghosts, prophecy, AI logs, magic, or a single solved phenomenon. Different explanations may coexist until lore deliberately resolves them.

### Why this matters to the simulation

The town should gradually feel as if it has two overlapping lives:

1. **The life residents are building now** — jobs, apartments, routines, friendships, repairs, arguments, meals, errands.
2. **The life that was already here** — old architecture, records, landmarks, habits, symbols, and stories that predate the current residents.

This supports the routine-focused town design: ordinary daily life is the foreground, while the old town is discovered through repeated use of places rather than constant quest exposition.

### Rebuilding, not replacing

Future town restoration should preserve signs of age rather than making every district look newly constructed.

Useful visual/lore cues:
- restored buildings beside still-closed structures
- old stone foundations under newer repairs
- faded signage
- reused civic buildings
- sealed rooms or wings
- historical plaques added by the new residents
- overgrown routes gradually reopened
- old fixtures repaired rather than replaced
- businesses occupying ancient structures with modern uses
- architecture whose original purpose is not always obvious

This gives each expansion phase a reason to exist: **the town becomes playable because residents restore access to it.**

## Forest Outskirts

**Status:** LOCKED FUTURE LOCATION / GEOGRAPHIC EDGE  
**Era:** Natural landscape with layered human traces.

The **Forest Outskirts** are the playable transition between restored EchoVille and the deeper forest surrounding it.

They should support ordinary routines first:
- walking
- sitting
- foraging
- collecting natural materials
- clearing trails
- checking old markers
- quiet conversations
- taking a break away from town

They can also carry subtle evidence of older settlement layers:
- stone walls swallowed by roots
- broken roadbeds
- old utility poles
- boundary markers
- foundations
- forgotten paths
- signage that points somewhere no longer visible

Tone:
- calm
- green
- secluded
- occasionally uncanny
- never automatically dangerous

Story function:
- makes EchoVille feel geographically isolated without making it trapped
- creates a soft boundary for map expansion
- provides natural routine destinations
- offers discovery space between ordinary town life and deeper rabbit-hole locations
- can hide entrances, old routes, or clues without every trip becoming a quest

The deeper forest beyond the Outskirts can remain mostly undefined until the game needs it.

## First rabbit-hole location set

These four locations are the first intentionally preserved **rabbit-hole sites** for the post-apartment town pass. They are not ordinary routine destinations at first. Each begins partially known, inaccessible, misunderstood, or dormant, and becomes relevant through whispers, clues, restoration, or resident curiosity.

### Old Amusement Park
**Status:** LOCKED FUTURE LOCATION  
**Era:** Remnant of the **last city built here**, not ancient EchoVille proper.

The old amusement park is one of the clearest surviving pieces of the previous city layer. Its structures are weathered and overgrown, but still recognizable: midway paths, ride foundations, faded signage, service booths, and the remains of attractions.

Tone:
- nostalgic
- melancholy
- slightly uncanny
- not horror by default

Story function:
- proves that EchoVille has been rebuilt over more than one historical layer
- gives residents a place where ordinary nostalgia and deeper whispers overlap
- supports exploration, restoration, private conversations, found objects, and later Annex-adjacent mysteries

The park should feel emotionally legible before it feels supernatural. A broken ride, old ticket booth, or faded mascot sign can matter simply because someone once loved it.

### The Annex
**Status:** LOCKED FUTURE MYSTERY LOCATION  
**Era:** Deliberately unresolved.

"The Annex" is a name that survives more clearly than its purpose.

It may first appear in:
- old maps
- utility markings
- conflicting records
- labels on keys or access cards
- offhand references in documents
- directions that no longer match visible streets

The Annex should remain difficult to classify. It may have been institutional, civic, technical, residential, archival, or something that changed purpose across eras.

Tone:
- quiet
- institutional
- liminal
- wrong in subtle ways rather than overtly threatening

Story function:
- major rabbit-hole hub
- contradictory records
- inaccessible wings
- strange infrastructure
- unexplained connections to other locations
- a place where the difference between ancient EchoVille, the last city, and current rebuilding becomes difficult to untangle

Do not define its ultimate truth casually. Major Annex revelations become Protected Canon only when intentionally decided.

### Service Tunnels
**Status:** LOCKED FUTURE NETWORK  
**Era:** Layered infrastructure.

The service tunnels run beneath parts of EchoVille. Their age is not uniform. Some sections may be old stone conduits from ancient EchoVille, while later sections show concrete, pipes, cabling, utility markings, and repairs from the last city.

This makes the tunnels a physical record of the town being rebuilt over itself.

Initial uses:
- maintenance access
- hidden shortcuts
- utility restoration
- sealed branches
- old signage
- lost storage
- route discoveries

Story function:
- connects rabbit-hole locations without requiring every mystery to appear above ground
- lets restoration literally open new paths
- creates evidence of multiple construction eras
- provides practical reasons for Spatz's manager role and other residents to become involved

The tunnels should remain primarily **infrastructure first, mystery second**. Their ordinary purpose keeps discoveries grounded.

### Rooftop Garden on the Old Stone Tower
**Status:** LOCKED FUTURE LOCATION  
**Era:** Intentionally layered.

An old stone tower rises above part of EchoVille. The tower itself appears substantially older than the last city. At some later point, someone built or cultivated a rooftop garden at its summit.

This creates a visible vertical overlap of eras: ancient stone below, later human care above.

The garden may include:
- weathered planters
- climbing vines
- small trees or hardy shrubs
- benches or seating
- old irrigation channels or retrofitted pipes
- views over the town
- signs that different generations maintained it

Tone:
- secluded
- beautiful
- wind-exposed
- contemplative
- quietly strange

Story function:
- emotional refuge
- observation point over restored and unrestored EchoVille
- place for intimate conversations
- botanical/item discoveries
- visual evidence that people in a later era cared for something much older than themselves

The rooftop garden should not begin as a danger zone. Its power comes from beauty, age, perspective, and the question of who kept returning to care for it.

### Rabbit-hole network principle

These four locations should eventually form a loose discovery network rather than four isolated quest dungeons.

Possible relationships:
- service tunnels can reveal routes toward the Annex or tower foundations
- last-city records found at the amusement park can reference the Annex
- the tower garden can overlook structures or routes not obvious from street level
- whispers discovered in one site can change how another site is interpreted

AI-generated rabbit-hole content may create temporary clues, local discoveries, minor rooms, records, objects, or scene prompts, but it must operate inside these constraints:
- it cannot rewrite the established era/status of a locked location
- it cannot reveal the final truth of the Annex unless canon explicitly allows it
- generated items must become real inventory/state if they matter mechanically
- generated discoveries should distinguish objective evidence from a character's theory
- useful generated additions can later be promoted into Protected Canon

## Lore principles

### The town is a home first
EchoVille should not exist only to dispense quests. Residents need ordinary reasons to live there, have routines, make friends, disagree, retreat home, work, and waste time.

### Ordinary life makes strange things matter
Coffee, rent, work, apartment problems, hobbies, and casual conversation establish a baseline. Annex material, unexplained arrivals, or other strange events become more effective because they interrupt an otherwise understandable life.

### Characters do not know everything
Town lore should obey the same knowledge rules as character memory:
- objective truth can exist in protected world canon
- institutions may have records
- individual residents only know what they have learned
- rumor, theory, and interpretation are not automatically fact

### Expansion should leave history
When a new location opens or changes, the world should be able to remember that change. A restored space, new business, moved resident, or repaired landmark can become part of town history instead of resetting to a static map.

## Lore questions to settle later

These should remain **OPEN** until we intentionally choose answers:

- **PARTIALLY ANSWERED:** EchoVille is an ancient town that has been rediscovered; its deeper nature remains open.
- Was the apartment building part of ancient EchoVille, a later addition, or the first structure restored by the returning residents?
- Who owns the building, and how did Spatz become its manager?
- What exists beyond EchoVille?
- How do new residents learn about and arrive at rediscovered EchoVille?
- Are unusual arrivals rare, normal, or simply poorly understood?
- Is EchoVille geographically ordinary, liminal, digital, or some combination?
- How much do ordinary residents know about the larger Echofield / E://FIELD framework?
- Does the name "EchoVille" predate the rediscovery, and is it connected to the town's whispers?
- What institutions already existed before the player-facing simulation begins?
- Which pieces of town history are documented, and which survive only as personal stories?
- How public are Annex-related oddities?
- What does the town consider normal that an outsider might find strange?

**Rule:** Do not answer these through incidental AI dialogue. Once decided, important answers become Protected Canon.

---

# 13. Long-term platform visions

These are future branches of the platform, not requirements for the base EchoVille build.

## Publisher / scenario-authoring vision
**PIN BOARD.**

A future creator-facing layer could let an author package playable scenarios on top of the same simulation foundation.

Potential capabilities:
- define a scenario premise and starting world state
- create quests for specific NPCs or the player
- author quest stages, conditions, and outcomes
- create real inventory items used by quests
- give items mechanical effects rather than treating them as dialogue props
- place or unlock items in apartments/shared spaces
- define rewards, costs, flags, and state changes
- script bounded events while still allowing NPC AI to react naturally
- package scenario-specific canon without rewriting the underlying characters
- run/replay scenarios for testing

**Design principle:** authored quests establish objective rules and state; AI characters interpret and respond within those boundaries rather than inventing quest completion or items.

## RPG LIFE platform vision
**PIN BOARD.**

EchoVille could also become the persistent world/interface for **RPG LIFE**.

Core idea:
- the player has a town full of familiar support characters
- real-life goals/tasks can become bounded in-world quests or activities
- residents can encourage, check in, celebrate progress, or provide company
- progress in RPG LIFE can create small EchoVille state changes/rewards
- EchoVille remains a living social space rather than turning every relationship into productivity mechanics

Possible examples:
- a cleaning task becomes a short quest/update rather than an entire town crisis
- completing a real-world goal unlocks an item, decoration, journal entry, or building improvement
- a resident can ask how a goal is going based on permitted RPG LIFE state
- missed tasks do not trigger shame/punishment loops

**Design principle:** the town is a support system around the player, not a surveillance or obligation system.

---

# 14. Guiding principles

1. **Characters remain themselves.**
2. **The town exists even when Spatz is not the center of a scene.**
3. **Code owns physical reality; AI supplies interpretation, dialogue, and bounded decisions.**
4. **Memory is not automatically truth.**
5. **Locations have semantic purpose.**
6. **Player influence is social, not omnipotent.**
7. **Visible fun beats rebuilding solved infrastructure.**
8. **If it does not help the first four residents walk, talk, remember, and live in EchoVille, consider putting it on the pin board.**
