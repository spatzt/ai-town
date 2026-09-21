# EchoVille on AI Town — Living Design Document

> **Purpose:** Current source of truth for the private EchoVille simulation being built on the AI Town codebase.
>
> **Working rule:** Mod first. Extend second. Replace solved systems only when EchoVille genuinely needs something different.
>
> **North star:** Open EchoVille, look around, and discover what the characters have been doing.

## Status key

- **CURRENT** — active design / implementation direction.
- **LOCKED** — intentionally established canon or system behavior.
- **PLANNED** — expected later, but does not block the next playable build.
- **CANDIDATE** — useful option, not yet canon.
- **OPEN** — intentionally unresolved.
- **ARCHIVE** — superseded history kept only to prevent accidental reintroduction.

---

# 1. Current project state

## Technical foundation

- **CURRENT:** Fork: `spatzt/ai-town`.
- **CURRENT:** AI Town remains the simulation chassis.
- **CURRENT:** Convex handles backend/state/database/vector memory.
- **CURRENT:** PixiJS / React handles the browser renderer.
- **CURRENT:** Server-authoritative simulation: humans and AI agents submit inputs; ordinary code validates and applies world-state changes.
- **CURRENT:** Tiled/tile-map movement, collision, pathfinding, facing, activity state, conversations, and persistent world state are useful existing systems.
- **CURRENT:** LLM work stays outside the deterministic movement loop.
- **CURRENT:** Browser-first is preferred because phone access matters.
- **CURRENT SETUP:** Codespace exists and dependencies are installed. The next environment step is Convex login/setup, then run stock AI Town before modifying it.
- **RULE:** Do not change EchoVille code until vanilla AI Town is visibly running and checkpointed.

## What AI Town already gives us

Keep or adapt:
- persistent worlds and players
- smooth tile movement and collision
- A*-style pathfinding
- human movement and NPC selection
- human/NPC and NPC/NPC conversations
- NPC autonomous invitations and physical walk-over before talking
- accept/reject/leave conversation states
- conversation cooldowns and turn-taking
- archived conversation history
- vector memories, importance, retrieval, and reflections
- async LLM operations outside the hot simulation loop
- configurable model providers

Modify:
- stock map, art, names, lore, and generic activities
- random wandering into character/location-aware routines
- probability-only social decisions into relationship/personality-aware choices
- memory so generated summaries never become the sole source of objective truth

Current AI Town limitation:
- ordinary conversations are two-person only; group conversations are **PLANNED**.

---

# 2. Core design architecture

## Authority split

**Code owns physical reality. AI owns interpretation and bounded choice.**

The AI may:
- choose among valid activities
- express wants
- decide whether to approach someone
- accept/decline/postpone/counter an invitation
- generate dialogue
- interpret events
- form memories and theories
- choose to buy/use/give something through a valid game action

The AI may **not** silently:
- create or delete money
- create, duplicate, consume, or move items without a validated action
- teleport characters
- rewrite Protected Canon
- invent completed quests
- promote rumors into facts
- grant itself access to information it never learned

## Canon and memory

EchoVille uses four layers:

1. **Protected Canon** — established truths that the simulation cannot overwrite.
2. **Objective Event** — what actually occurred in the current EchoVille timeline.
3. **Character Interpretation** — what a specific person believes, remembers, suspects, or felt about it.
4. **Searchable Memory** — summaries/reflections used for retrieval and conversation context.

Memory is not automatically truth.

Characters only retrieve information they legitimately know.

## Relationships

Relationships are directed and multidimensional rather than one Friendship score.

Core dimensions:
- affection
- trust
- comfort
- respect
- irritation
- tension

Optional dimensions can be added for specific character dynamics when useful, such as dependency, jealousy, or control.

## Randomness rule

**Randomness may choose an opportunity. Character logic chooses the response.**

Useful randomness:
- who happens to cross paths
- which optional routine opportunity appears
- which minor town event becomes available

Character logic should decide:
- whether they engage
- what they say/do
- how they interpret it
- whether it matters later

This keeps some Tomodachi-Life-style surprise without letting arbitrary randomness replace characterization.

---

# 3. Cast and current roles

## First playable core

### Spatz
- **CURRENT:** human-controlled player resident
- **CURRENT:** apartment manager
- creative, curious, playful, observant
- manages the building, not the people
- has natural reasons to inspect shared spaces, notice maintenance issues, coordinate access, and meet residents
- **LOCKED:** when the player is away, Spatz is treated as away rather than automatically AI-piloted
- **LOCKED:** the player controls Spatz's personal currency, not everyone else's

### Chad
- strong-willed, passionate, competitive, direct, stubborn, dependable
- close friend-rival relationship with Skylar
- first apartment anchor: Building Gym
- later town work/community anchor: **Training Hall**

### Skylar
- librarian / digital archivist
- thoughtful, analytical, curious, observant, quietly stubborn
- close friend-rival relationship with Chad
- first apartment anchor: Shared Computer Space
- later work anchor: **EchoVille Library / Academy / Digital Archive**
- natural default study buddy for RPG Life learning sessions, while other residents may also be invited

### Kevin
- adult trans man
- barista, baker, cook
- warm, observant, loyal, anxious, creative, teasing, quietly protective
- **LOCKED:** The Wired Bean is a standalone town location outside Apartment Mode
- **LOCKED VISUAL:** its signature feature is a skylight / glass upper-roof section
- Kevin was pulled into the project largely because of Kyle
- his practical response is people-centered: if people are living here, they need food, breakfast, coffee, and somewhere warm to gather
- later work/social anchor: **The Wired Bean**

## Later residents with defined roles

### Kyle
- Kevin's older brother
- **LOCKED:** hired by the company/research project to help keep things together
- **LOCKED:** works through **EchoVille Dispatch**
- Dispatch is EchoVille's main practical bridge to the outside world:
  - company communication
  - research/project paperwork
  - incoming funding notices
  - shipments
  - supply requests
  - outgoing packages
  - newcomer intake/logistics
- carries substantial operational pressure because delays, shortages, sponsor demands, and failed logistics can land on him
- has materially greater Project Awareness than Kevin
- exact formal title remains **OPEN**

### Kenneth
- **LOCKED:** Workshop / Handyman
- mechanic, fabricator, repairer, salvage assessor, and practical fixer
- work anchor: **Workshop Cottage / Repair Garage**
- handles apartment repairs, public-space repairs, equipment maintenance, restoration work, utilities, and odd jobs
- naturally collaborates with:
  - Spatz on building maintenance
  - Kyle on parts and shipments
  - Chad on equipment
  - Skylar on old devices/mechanisms
- keep his playful/flirty/sussy personality; the role must not flatten him into a stoic maintenance NPC

### Brae
- 16
- **LOCKED:** begins living in the **Forest Outskirts**
- his home is self-chosen and functional, not exile
- normal routines may include trails, solitude, clearing paths, collecting/foraging/scavenging, and selective town visits
- **PLANNED POSSIBILITY:** he may later choose to take an apartment, share housing, or use a temporary room
- exact form of his Forest Outskirts home remains **OPEN**

### Candice
- 13
- future arrival/newcomer
- Old Amusement Park remains an arrival/story location, not a job
- **PLANNED POSSIBILITY:** she may be given a room in the apartment building
- exact guardian/household arrangement must be intentionally established rather than invented by routine AI

## Digimon

**PLANNED / not first build.**

Partnerships remain canon, but follower/companion mechanics do not block the first playable slice.

Future considerations:
- first-class companion actors
- following/tethering
- autonomous partner behavior
- scene participation
- partner-specific communication

---

# 4. World premise and lore

## Rediscovered EchoVille

**LOCKED CANON**

EchoVille is ancient. It is not a newly founded town.

It was lost, forgotten, inaccessible, abandoned, or otherwise removed from ordinary habitation long enough to cease being an active town. It has now been rediscovered.

The current residents are among the first people returning to live here and restore it.

The apartment building is the first practical foothold: establish somewhere safe to live, restore daily life, then reopen the wider town.

## Historical layers

EchoVille can contain at least three overlapping layers:

1. **Ancient EchoVille**
2. **The last city built here**
3. **The current rediscovery/rebuilding**

The Old Amusement Park is a surviving site from the **last city**, not ancient EchoVille proper.

## The forest

**LOCKED CANON**

EchoVille sits in the middle of a forest.

Spatial gradient:
- restored town center
- town edge / partially reopened routes
- Forest Outskirts
- deeper forest, mostly undefined until needed

The forest should feel calm, green, secluded, and occasionally uncanny rather than automatically dangerous.

It can preserve traces of prior settlement:
- old roadbeds
- foundations
- swallowed walls
- utility poles
- boundary markers
- forgotten routes
- signage pointing somewhere no longer obvious

## Whispers

**LOCKED CANON**

EchoVille holds **whispers** from both the ancient past and the present.

Whispers can appear as:
- half-remembered stories
- contradictory records
- repeated names/phrases
- emotional residue around places
- lingering modern conversations
- rumors that mutate
- strange familiarity
- event fragments
- rare uncanny moments

Whispers are **not automatically ghosts, prophecy, AI logs, magic, or one solved phenomenon**.

## Rebuilding principle

Restore rather than erase the town's age.

Useful visual/world cues:
- restored structures beside sealed/closed ones
- old stone foundations under newer work
- reused civic buildings
- faded signage
- old fixtures repaired rather than replaced
- later businesses operating inside much older shells
- overgrown routes gradually reopened

---

# 5. Research-project framing and awareness

## Project premise

**LOCKED DIRECTION**

The current rebuilding effort is funded and observed by an outside research team/company.

Residents may have entered EchoVille very differently:
- hired/recruited
- attached to someone already involved
- practical contractor/staff role
- roped in
- accidental arrival
- unknown/unclear arrival

Not every resident is a company employee.

Research funding creates resources and pressure without giving the sponsor ownership over every personal decision in town.

Possible funded categories:
- early apartment restoration
- utilities
- research equipment/computers
- relocation/startup support
- restoration grants
- formal staff wages/contracts
- scenario-specific funding

## Two awareness axes

Each character has two independent awareness levels.

### Project Awareness
How much they understand the formal project:
- funding
- company/team
- rules/protocols
- official roles
- infrastructure
- who is staff/recruited/present accidentally

### Meta Awareness
How much they notice or understand deeper constructed/system-like/observed qualities of EchoVille.

Meta Awareness does **not** automatically mean "knows this is a video game."

Possible expressions:
- notices repeating systems
- recognizes improbable coincidences
- suspects outside intervention
- understands some abstractions metaphorically
- recognizes an observer/player
- at the highest level, reasons deliberately about the simulation/meta layer

### Scale

Use the same 0–4 scale for both:
- **0 — None**
- **1 — Vague**
- **2 — Functional**
- **3 — Informed**
- **4 — Insider**

Major changes should come from real disclosure, discoveries, role changes, or events—not random AI drift.

Known relative placement:
- Kyle has substantially more **Project Awareness** than Kevin.
- Exact starting values for the wider cast remain **OPEN**.

---

# 6. Location master plan

## Apartment Building — CURRENT first world

**LOCKED:** no separate apartment coffee shop. The Wired Bean is outside Apartment Mode.

Core spaces:
- Spatz's apartment
- Chad's apartment
- Skylar's apartment
- Kevin's apartment
- additional/temporary units
- lobby / hallways / common seating
- Building Gym
- Shared Computer Space
- apartment-manager/building-task space

The apartment remains important after the town opens because it is:
- home
- newcomer arrival point
- temporary lodging
- household-management space
- a routine social crossing point

## Wider town — consolidated plan

| Location | Status | Primary function | Main association |
| --- | --- | --- | --- |
| **Guidepost Square** | PLANNED | civic crossroads, notice/quest board, events | shared |
| **Central Green / Fountain Park** | PLANNED | one consolidated public green/park | shared |
| **The Wired Bean** | LOCKED DIRECTION | standalone café, food, social hub; signature skylight/glass-roof element | Kevin |
| **Library / Academy / Digital Archive** | LOCKED DIRECTION | library, study, records, computers, learning | Skylar |
| **Training Hall** | LOCKED DIRECTION | training, exercise, teaching, community | Chad |
| **EchoVille Dispatch** | LOCKED | logistics, shipments, outside-project link | Kyle |
| **Workshop Cottage / Repair Garage** | LOCKED FUNCTION / OPEN final name-art | repairs, tools, fabrication, salvage, restoration | Kenneth |
| **Market Row + General Store** | PLANNED | routine shopping, supplies, rotating stalls | shared |
| **Residential Lane** | PLANNED | later housing expansion | future residents |
| **Forest Outskirts** | LOCKED | trails, solitude, resources, transition zone | Brae |
| **Old Amusement Park** | LOCKED FUTURE | last-city remnant, exploration, nostalgia | mystery/story |
| **The Annex** | LOCKED FUTURE / truth OPEN | major mystery hub | mystery/story |
| **Service Tunnels** | LOCKED FUTURE | infrastructure, shortcuts, restoration | town-wide |
| **Old Stone Tower + Rooftop Garden** | LOCKED FUTURE | refuge, views, layered history | story/social |

## Small town-life structures

These are mostly **CANDIDATES** unless promoted later. Many can be semantic points without full interiors.

Useful candidates:
- Guidepost Quest Board
- town clock/bell marker
- mail/parcel kiosk tied to Dispatch
- laundry/washhouse
- gazebo/picnic shelter
- community garden beds
- small greenhouse/orchard
- pantry/root cellar
- rotating market stalls
- creator atelier/art shed
- material depot/reclaim yard
- donation/reuse shed
- restoration ledger desk
- history/memorial plaques
- trailhead board
- benches/seating nodes
- utility/public-works shed
- Forest Watch/Ranger Shed
- clinic/apothecary
- modest arrival/bus shelter if outside travel needs a visible stop

## Consolidated older concepts

Do not keep duplicate locations just because earlier prototypes had them.

- **Town Square → Guidepost Square**
- **Central Green + Fountain Park → one public green**
- **Marketplace + General Store → Market Row / General Store system**
- **Workshop Cottage + Repair Garage → one Kenneth work complex**
- **Old Fairground → Old Amusement Park**
- **Spatz's Base → Spatz's Apartment / current home role**
- **Trails → Forest Outskirts trail network**

## Archive / superseded

- **Quarry Lake / Dockhouse — ARCHIVE.** Do not reintroduce automatically.
- lake-trail material tied specifically to Quarry Lake is also archive unless intentionally redesigned.
- The Frame / Hollow Frame and Church of Iris material belong to broader Echofield/Alteria continuity, not automatic EchoVille town geography.

---

# 7. Apartment households and newcomer system

## Apartments are households, not one-NPC slots

**LOCKED**

A unit may hold:
- one resident
- roommates
- a couple
- a family/guardian household where canon supports it
- a temporary resident
- a temporary resident sharing with an established household if everyone involved agrees

Housing changes are explicit world-state events.

Relationship scores alone must never auto-move characters together.

Possible changes:
- friends become roommates
- a couple chooses to share a unit
- roommates split
- someone joins an existing household
- Brae chooses to move inward
- someone keeps another home while using a room temporarily
- a unit becomes vacant again

## Temporary-stay system

**LOCKED**

The apartment building itself provides temporary lodging. A separate guesthouse is not needed.

Temporary residents:
- receive a real room assignment
- receive room/apartment storage
- join ordinary AI routines
- use appropriate common spaces
- meet residents
- explore the town
- discover preferences, work interests, and routines
- are not automatically permanent residents

Possible outcomes:
- extend stay
- move into a permanent solo unit
- become a roommate/join a household
- move elsewhere in EchoVille
- leave
- return later

The player chooses **who is offered lodging**, not who that person becomes.

## Newcomer intake loop

1. Newcomer becomes available through discovery, referral, scenario, return visit, or town growth.
2. Spatz may offer an available temporary room.
3. The newcomer moves in with an initial inventory and normal autonomy.
4. Their routines reveal preferred people/places/work.
5. The town learns about them; they learn about EchoVille.
6. Later housing resolves into permanent solo housing, shared housing, relocation, extended stay, or departure.

The intended feeling is **meeting a potential new neighbor**, not recruiting a stat block.

---

# 8. NPC daily life and jobs

## Routine philosophy

Target: **predictable life with room for believable variation**.

Routine structure:
**obligation / anchor → preferred spaces → bounded free choice**

Each resident can have:
- home
- primary anchor
- secondary anchors
- errand pool
- social pool
- quiet pool
- special/event locations

Possible routine conditions:
- time/day
- work/off day
- business hours
- current task
- mood/state
- recent memories
- relationship context
- invitations
- town events
- scenarios
- weather later

The player should be able to learn habits without the world becoming scripted.

## Current routine skeleton

| Resident | Home | Main anchor | Secondary behavior |
| --- | --- | --- | --- |
| **Spatz** | apartment | apartment management / player-directed | shared spaces, town tasks |
| **Chad** | apartment | Building Gym → later Training Hall | common areas, social/training |
| **Skylar** | apartment | Shared Computer Space → later Library/Academy | study, Wired Bean, quiet spaces |
| **Kevin** | apartment | later Wired Bean | food/coffee, social spaces, home |
| **Kyle** | future home | Dispatch | errands, receiving, outside-project logistics |
| **Kenneth** | future home | Workshop / Repair Garage | repair calls, public works, salvage, town sites |
| **Brae** | Forest Outskirts | outskirts/trails | selective inward trips |
| **Candice** | unresolved | unresolved | town discovery; park is story location, not job |

## Jobs are learning systems

A job is not just a schedule destination.

Work can produce:
- skill/familiarity
- new routines
- preferences
- social knowledge
- useful memories
- small town improvements

Examples:
- Kevin learns recipes and customer preferences.
- Skylar catalogs records and notices contradictions.
- Chad develops training routines and learns how others prefer to exercise.
- Kyle learns routes, delivery habits, and supply constraints.
- Kenneth learns recurring repair problems, salvage value, ancient construction, and which fixes are temporary.
- Brae learns the Outskirts through repeated use.

Job learning must never silently overwrite Protected Canon.

---

# 9. Communication and player influence

## Cellphone / text system

**LOCKED DIRECTION**

Social nudges and off-location coordination should happen through an in-world cellphone/text system.

Residents may:
- text/call Spatz
- text/call other residents
- catch up
- ask to meet
- invite someone somewhere
- coordinate errands or work
- follow up after earlier events

This directly helps the town avoid becoming socially quiet just because characters are not already standing together.

## Player suggestions

Spatz influences socially rather than puppeting.

Examples:
- coffee?
- come see this
- talk to Skylar
- want to train?
- meet me at the library

NPCs can:
- accept
- decline
- postpone
- counteroffer

Decision inputs can include:
- activity
- schedule
- mood/state
- relationship
- location
- personality
- recent memory

A successful invitation becomes an ordinary intention/action. No teleportation.

## Observation privacy

**OPEN**

Possible modes:
- terrarium/observer: player can inspect off-screen NPC interactions
- diegetic: private conversations remain private
- support both as a selectable presentation mode

---

# 10. RPG Life integration

## Current direction

RPG Life is no longer merely a distant pin-board idea. It is a **core long-term layer** around the town, added after the base simulation is visibly working.

The town remains a social world first. RPG Life should support the player without turning every relationship into productivity mechanics.

## Dual progression

### Player progression
- **XP** — personal progression/leveling
- **Gold** — RPG Life reward currency earned only from intentionally gamified goals/quests

Rules:
- no negative XP
- no failure debt
- missed goals do not damage relationships
- Gold is not ordinary town cash

### Resident/town progression
AI residents grow independently through:
- jobs
- routines
- learning
- relationships
- town history
- discoveries
- personal preferences

Resident growth is not purchased with player XP.

## RPG Life → town spaces

| RPG Life function | EchoVille face |
| --- | --- |
| Daily Orientation / quest choice | Guidepost Square + Quest Board |
| Home / Fortress Maintenance | Spatz's apartment + Apartment Building |
| Health & Self Care | Training Hall / Central Green / home |
| Creativity | Creator Atelier/Art Shed if retained |
| Money / Treasury | ledger interface / restoration desk |
| Work | work-launch rituals, Dispatch/arrival routes as appropriate |
| Relationships | Wired Bean, park, square, homes |
| Pets / Party Camp | primarily home/apartment context |
| Adventure & Learning | Library/Academy, Forest Outskirts, rabbit holes |
| Rewards | General Store/reward interface/rotating stall |
| Campaigns & Goals | town restoration/project board |

## Library as Academy

The **Library / Academy / Digital Archive** is the learning hub rather than a separate academy building.

Possible spaces:
- stacks
- digital archive/computer room
- quiet study tables
- small study rooms
- workshop/class table
- old-records section
- later sealed/older wing if lore supports it

Study modes:
- study alone
- ask Skylar
- invite another resident
- quiet co-working
- take a break together afterward

The companion remains a character, not a productivity coach.

## Support pattern

Examples:
- study → Library / Skylar or invited resident
- train → Training Hall / Chad or invited resident
- create → Atelier / invited company
- home project → apartment / Kenneth when appropriate
- coffee/decompress → Wired Bean / Kevin or whoever is there
- walk/reset → Green or Forest Outskirts
- errands → Market Row
- quest choice → Guidepost Board

**Rule:** support, not surveillance.

---

# 11. Currency and economy

## Separate resource layers

Do not collapse every number into Gold.

### Meta/player layer
- **XP** — progression
- **Gold** — RPG Life reward currency

### In-world layer
- **Resident Wallets** — ordinary personal money
- **Business Accounts** — sales, wages, supplies, upkeep when enabled
- **Apartment Building Budget** — rent, utilities, maintenance, shared upgrades
- **Town Treasury** — assessments/taxes, fees, grants, public spending
- **Research/Company Funding** — external project money, grants, formal contracts, startup support

**LOCKED:** Spatz's personal wallet is separate from the building fund and town treasury.

**LOCKED:** the player controls Spatz's money, not NPC wallets.

NPCs may make their own purchases through validated game actions.

Gold should not silently convert into ordinary town money.

If a Gold reward becomes a physical EchoVille object, the item still needs a valid authoritative acquisition path.

## Apartment building income

Basic flow:

**Resident income → Resident Wallet → Rent → Apartment Building Budget**

Building expenses may include:
- maintenance reserve
- utilities/shared services
- repairs
- common-space furnishings/upgrades
- optional contractor/staff costs
- town property assessment/tax

Surplus remains with the building for reinvestment rather than becoming Spatz's personal cash.

## Town treasury

Possible income:
- property/building assessments
- business assessments
- market permits/stall fees
- optional service fees
- research grants
- donations/scenario funds

Possible spending:
- roads/paths/lighting
- utilities/public works
- parks/square/trails
- civic structures
- town-owned restoration
- service-tunnel work
- events
- scenario/emergency response

## Tax design

Start simple:
- flat periodic assessment per property/business
- visible upcoming charges
- transparent use of town money

Taxes should create restoration choices rather than accounting anxiety.

Possible later complexity:
- property value
- business revenue bands
- exemptions
- restoration grants
- service fees
- scenario-specific funding

---

# 12. Inventory and item system

## Inventory scopes

Items may belong to:
- carried personal inventory
- private personal storage
- apartment/household storage
- shared household inventory
- business stock/storage
- building/common inventory
- Workshop inventory
- Dispatch shipment/storage inventory
- public/town restoration inventory
- scenario inventory

Ownership and location are separate concepts.

Example:
- Kevin owns a cookbook.
- Kevin and a roommate may jointly own a couch.
- The apartment building owns a common-area refrigerator.
- A replacement pump can be owned by the project while physically sitting in Dispatch.

## Pokémon-inspired category pockets

Portable/storage UI should use category pockets rather than one giant unsorted list.

Suggested categories:
- Food & Drinks
- Medicine / Care
- Tools
- Materials / Parts
- Gifts & Personal Items
- Books / Documents / Media
- Key / Story Items
- Scenario / Quest Items
- Miscellaneous

Rules:
- category is organizational, not a duplicate inventory
- Key/Story Items are protected from accidental sale/disposal
- common supplies may stack
- unique objects may have individual instances and provenance
- scenario items remain real world-state objects

## Item definitions vs instances

Common item definitions can be reused.

Example definition:
- Coffee Mug
- household
- non-stackable
- actions: drink_from, place, give

A meaningful individual instance can additionally store:
- unique ID
- owner
- current location
- condition
- custom name
- provenance/history

Use individual instances when identity matters:
- Kenneth's favorite wrench
- old park token
- Annex access card
- recovered photograph
- one-of-a-kind gift

## Three physical object states

1. **Portable Item**
   - carried/transferred/stored
   - keys, books, food, tools, documents

2. **Placeable Furnishing**
   - stored until placed
   - then gains room position, orientation, interactions, and ownership context
   - couch, desk, bookshelf, TV, lamp

3. **Fixture / Property Object**
   - part of a building/location
   - changed through management/build mode
   - apartment door, built-in stove/counter, utility panel

## Sims-inspired Build/Buy and furniture

Furniture is gameplay, not just decoration.

Possible furnishing data:
- owner/household/organization
- property + room
- footprint
- orientation
- condition
- style/variant
- value
- container contents if relevant
- interaction/activity tags
- optional comfort/privacy/social/work/fun/study/cooking/sleep/decor effects

Furniture creates AI affordances.

Examples:
- bed → sleep/rest
- sofa → sit/talk/read/relax
- desk → study/write/draw/computer use depending on attached objects
- bookshelf → browse/read
- dining table → eat/talk
- stove → cook
- TV + console → play/invite someone to play
- workbench → repair/craft
- coffee equipment → make drink
- cabinet → storage

Adding/removing furniture can therefore change resident routines.

## Item authority rule

AI may discuss, request, desire, buy, use, gift, or react to an item.

Authoritative code must determine:
- whether it exists
- where it is
- who owns it
- whether enough money exists
- whether a transfer is valid
- whether use consumes/changes it

Important objects can retain provenance and character-specific interpretations without confusing either with objective state.

---

# 13. Rabbit-hole locations

## Old Amusement Park

**LOCKED FUTURE LOCATION**  
**Era:** last-city remnant

Tone:
- nostalgic
- melancholy
- slightly uncanny
- not horror by default

Functions:
- exploration
- restoration
- private conversations
- found objects
- last-city records
- Annex-adjacent mysteries

## The Annex

**LOCKED FUTURE MYSTERY LOCATION**  
**Truth:** intentionally OPEN

"The Annex" survives more clearly as a name than as a known purpose.

It may first appear through:
- old maps
- utility markings
- documents
- access cards/keys
- conflicting records
- directions that no longer match visible streets

Do not casually define its ultimate truth.

## Service Tunnels

**LOCKED FUTURE NETWORK**

Layered infrastructure:
- older stone sections
- later concrete/pipes/cabling/repairs

Functions:
- maintenance
- hidden shortcuts
- sealed branches
- utility restoration
- storage
- route discoveries
- links between other rabbit-hole sites

Infrastructure first, mystery second.

## Old Stone Tower + Rooftop Garden

**LOCKED FUTURE LOCATION**

The stone tower appears substantially older than the last city; the rooftop garden is a later layer of care.

Tone:
- secluded
- beautiful
- wind-exposed
- contemplative
- quietly strange

Functions:
- refuge
- observation
- intimate conversations
- botanical/item discoveries
- visible evidence of multiple eras

## Rabbit-hole generation rule

AI-generated rabbit-hole content may create:
- temporary clues
- minor rooms
- records
- objects
- local discoveries
- scene prompts

It may not:
- rewrite a locked location's era/status
- reveal the final Annex truth without deliberate canon approval
- create mechanically meaningful items without authoritative state
- confuse evidence with a character's theory

Useful generated additions can later be promoted to Protected Canon.

---

# 14. Scenario / publisher direction

## Scenarios

**PLANNED, not first build**

Scenarios should layer story/state over a persistent town rather than create disposable saves.

A scenario may define:
- starting town state
- participating residents
- available/unavailable locations
- temporary NPCs
- objectives
- optional objectives
- mechanical items
- town modifiers
- time window if useful
- reward pool
- branching outcomes
- town-history/memory changes
- cleanup/end-state rules

Rules:
- scenarios do not wipe the town on failure
- unfinished objectives can expire, pause, or branch
- AI residents keep their ordinary routines while reacting to scenario state
- objective changes still obey Protected Canon and authoritative state

## Publisher vision

Future authoring layer may support:
- premise/start state
- NPC/player quests
- quest stages and conditions
- real scenario inventory items
- mechanical effects
- placement/unlocks
- rewards/costs/flags
- bounded scripted events with AI reactions
- scenario canon packages
- replay/testing

Authored quest rules set objective state. AI interprets and reacts; it does not invent completion.

## Research references — systems, not templates

Useful inspirations:
- **AI Town** — autonomous movement/conversation/memory chassis
- **Stardew Valley** — learnable routines and clear town anchors
- **Tomodachi Life** — apartment observation, residents using personal objects, spontaneous social scenes; avoid excessive randomness
- **Pokémon** — inventory pocket organization and protected key/story items
- **The Sims** — households, room ownership, Build/Buy, furniture as usable world objects
- **Animal Crossing** — useful precedent for separating reward/progression currency from ordinary money
- **Project Highrise / The Tenants** — property rent, upkeep, tenant/property management
- **Foundation / Against the Storm / Frostpunk** — town growth and scenario framing ideas

EchoVille should borrow system lessons, not clone tone or content.

---

# 15. Visual and implementation plan

## Replace stock identity

Replace:
- stock AI Town character sprites
- stock map/tiles
- stock names/lore
- stock generic activity flavor
- stock UI identity where appropriate

## Open visual specs

Still need to lock against the actual engine:
- base tile size
- sprite dimensions
- walk-cycle layout
- idle/facing frames
- building scale
- interior strategy
- portrait dimensions if used
- interaction icons
- palette
- mobile readability

**Rule:** make assets for the engine's real expected format rather than finishing art first and forcing the engine around it.

## Map philosophy

- compact and walkable
- forest visibly encloses the town
- restored center, rougher edges
- ordinary life before mystery
- hidden rabbit-hole content should not all appear on the player's map before discovery
- larger expansions should create new behavior, not just decorative acreage

---

# 16. First playable slice

## Apartment Mode target

The first successful EchoVille slice is intentionally smaller than the full town.

Include:
- Spatz as human player/apartment manager
- Chad, Skylar, Kevin as autonomous AI residents
- private apartments
- lobby/halls/common seating
- Building Gym
- Shared Computer Space
- apartment manager/building-task context
- basic temporary-room data model, even if no newcomer is active yet
- EchoVille character prompts
- visible movement
- NPC/NPC conversations
- human/NPC conversations
- persistent conversation memory

**Do not include a Building Coffee Shop.** The Wired Bean opens as a separate town location later.

## "It feels alive" test

Without hand-authoring the scene:

1. Skylar spends plausible time in the computer space.
2. Chad uses the gym and later goes elsewhere.
3. Kevin moves between home/common/social contexts without being forced into a fake apartment café job.
4. Residents cross naturally in halls/common spaces.
5. NPCs initiate a character-appropriate conversation without Spatz.
6. Later dialogue can reference earlier interaction.
7. Spatz can participate without becoming the center of every relationship.
8. When Spatz is away, town life can continue/catch up without AI puppeting her.

---

# 17. Development order

## Milestone 0 — Vanilla AI Town
- finish Convex login/setup
- run stock build
- verify movement, agents, conversations, memory
- checkpoint

## Milestone 1 — Visible EchoVille reskin
- EchoVille-facing identity
- Spatz as player
- first custom sprite
- first visible map/location change

## Milestone 2 — Core four / Apartment Mode
- Spatz, Chad, Skylar, Kevin
- private apartments
- lobby/common circulation
- gym
- computer space
- simple character-aware routines

## Milestone 3 — Communication and social life
- cellphone/text system
- NPC-to-NPC calls/texts
- player nudges through phone
- relationship/personality-aware invitations
- away-state handling for Spatz

## Milestone 4 — EchoVille memory
- Protected Canon
- Objective Events
- Character Interpretations
- searchable subjective memories
- multidimensional relationships

## Milestone 5 — Wider town
- open outside apartment building
- Wired Bean
- Library/Academy
- Training Hall
- Dispatch
- Workshop
- town/forest routes

## Milestone 6 — Items and economy
- personal wallets
- building fund
- inventory scopes
- Pokémon-style pockets
- placeable furniture/build mode
- authoritative transfers/purchases
- initial rent/tax/ledger model

## Milestone 7 — Newcomers and growth
- temporary stays
- shared households/roommates/couples
- later cast
- town restoration
- scenario hooks

---

# 18. Open questions

Keep these intentionally unresolved until they matter:

- How long does the town simulate while nobody is watching?
- How much off-screen NPC conversation can the player inspect?
- Separate interior maps vs seamless zones?
- Exact first values for Project Awareness and Meta Awareness?
- Exact formal Kyle/company title?
- Exact source/name of ordinary in-world currency?
- Exact shape of Brae's Forest Outskirts residence?
- Who formally owns the apartment building, and how did Spatz become manager?
- Exact mechanism by which EchoVille became lost/inaccessible?
- What exists beyond the surrounding forest?
- How do new residents learn about EchoVille?
- Is EchoVille geographically ordinary, liminal, digital, or some combination?
- Does the name EchoVille predate rediscovery?
- Which institutions existed before the current return?
- How public are Annex-related oddities?
- Clinic/apothecary?
- arrival/bus service?
- Forest Watch/Ranger Shed?
- final Workshop name/art direction?

Important answers must not emerge accidentally from incidental AI dialogue.

---

# 19. Guiding principles

1. **Characters remain themselves.**
2. **The town exists when Spatz is not the center of the scene.**
3. **Code owns physical reality; AI supplies interpretation, dialogue, and bounded choice.**
4. **Memory is not automatically truth.**
5. **Locations have semantic purpose.**
6. **Player influence is social, not omnipotent.**
7. **Randomness creates opportunities, not arbitrary characterization.**
8. **The town is a home first and a quest system second.**
9. **RPG Life supports the player; it does not punish missed life tasks.**
10. **Player Gold, personal money, building money, town money, and company funding stay conceptually separate.**
11. **Important items exist in authoritative state and can have history.**
12. **Furniture should create resident activities, not merely decorate.**
13. **Visible fun beats rebuilding solved infrastructure.**
14. **Restore and expand only when a new system/location creates meaningful behavior.**
