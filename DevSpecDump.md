**Dev Specification: Trait Secondary Marketplace**

**Version:** v1

**Feature Owner:** CTO

**Technical Leads:** Frontend Dev, Backend Dev

**Sprint Blockers:** None currently

**Dependencies:** Supabase schema finalization, wallet connection flow

**Priority:** MVP-critical

**Target Completion:** May 5, 2025

---

**Module Overview**

This module enables users to list unequipped NFT traits for sale, browse global/collection trait marketplaces, and purchase traits from other users. The system must support trait ownership tracking, dynamic rarity calculations, secure escrow listing, and customization logic integration.

---

**Technical Scope Breakdown**

**🔧 Backend Responsibilities**

**1\. Database Schema Updates**

• trait\_items table: (actually user\_traits\_inventory)

• Add: listed (bool), price (decimal), currency, listing\_id (uuid), created\_at, updated\_at

• listings table:

• listing\_id

• item\_id

• seller\_id

• price

• currency

• status (active, sold, cancelled)

• buyer\_id

• created\_at, updated\_at

• Ensure trait → user → NFT → collection relations are intact

**2\. API Routes (4.18.25)**

| Method | Route | Description |  |
| ----- | ----- | ----- | ----- |
| GET | /traits/market | Global listings | Not done |
| GET | /traits/market/:collection\_id | Listings per collection | Not done |
| GET | /traits/:item\_id | Trait detail view | Not done |
| POST | /traits/:item\_id/list | List trait | Not done |
| POST | /traits/:item\_id/unlist | Cancel listing | Not done |
| POST | /traits/:item\_id/buy | Purchase trait | Not done |
| POST | /traits/:item\_id/equip | Equip to NFT | Yes |
| POST | /traits/:item\_id/unequip | Move to inventory | Yes |
| GET | /users/:id/inventory | User’s unequipped traits | Yes |
| GET | /traits/:item\_id/history | Trait ownership \+ usage log | No |

**3\. Escrow Logic**

• When listed: lock the trait (prevent customization)

• On purchase:

• Transfer ownership

• Update sold \= true

• Unset escrow flag

• Log transaction

• Prevent race conditions with atomic DB operations

**4\. Dynamic Rarity Tracking**

• Background worker or event-based trigger

• Recalculate rarity % when traits are:

• Equipped

• Unequipped

• Created (new stock)

• Burned

**5\. Ownership History Logging**

• On each:

• Equip / unequip

• Purchase

• Listing

• Log user\_id, nft\_id, event\_type, timestamp

**6\. Admin Support**

• Internal API to allow creator dashboard to:

• Create trait stock

• Remove unsold items

• Issue redemptions

---

**🎨 Frontend Responsibilities**

**1\. Inventory Page Updates**

• Display unequipped traits with filtering by type, collection, etc.

• Add “List for Sale” modal:

• Set price, confirm listing

• Call /list route

• Disable “Equip” while listing is active

**2\. Trait Marketplace Page**

• Marketplace browser:

• Collection filter

• Trait type filter

• Price slider

• Sort (price, rarity, newest, etc.)

• Trait cards:

• Image

• Trait name

• Price

• Collection

• Quick buy button

• Pagination/infinite scroll

**3\. Trait Detail View (4.23.25)**

• Expanded modal or page with:

• Trait image

• Type \+ subtype

• Collection name

• Rarity % and supply breakdown

• Ownership history (NFTs it’s been equipped to)

• Price chart (use charting library or placeholder MVP)

• Purchase button (calls buy route)

**4\. Post-Purchase Updates**

• Update inventory state

• Show success confirmation

• Prevent repurchasing

**5\. Error Handling & Edge Cases**

• Already purchased by someone else

• Trait no longer exists or has been unequipped since

• Purchase fails due to wallet or currency mismatch

**6\. Connected Wallet Flow (5.2.25)**

• Users must be signed in with wallet to list/buy

• Trigger wallet confirmation at purchase

• Display appropriate prompts and locked state for guest users

---

**Suggested Development Flow (To Mitigate Blockers)**

**Week 1–2**

• Backend:

• Schema migrations \+ API scaffolding

• Basic listing and ownership logic

• Dynamic rarity formula

• Frontend:

• Trait card components

• Inventory updates and list modal

• Marketplace page base layout

**Week 3–4**

• Backend:

• Purchase logic with escrow \+ atomic updates

• Admin and creator APIs

• Rarity re-indexing logic

• Frontend:

• Trait detail modal

• Marketplace filters/sorting

• Wallet auth integrations

**Week 5**

• Testing and bug fixes

• Inventory → marketplace → purchase end-to-end test

• Styling polish and error states

• Rarity % validation \+ drip score scaffolding (if ready)

---

**Jira Mapping (Sample)**

**Epic:** Trait Marketplace MVP

**Stories & Tasks:**

| Story | Team | Task |
| ----- | ----- | ----- |
| Setup DB schema for trait listings | Backend | trait\_items \+ listings schema migration |
| Build API to list/unlist/buy traits | Backend | Implement REST routes |
| Add trait listing UI | Frontend | Inventory → “List” modal |
| Build global trait marketplace UI | Frontend | Marketplace page, filters, cards |
| Build trait detail modal | Frontend | Trait detail popup |
| Handle purchase logic \+ escrow | Backend | Transfer, log, update listing status |
| Hook wallet flow to trait purchase | Frontend | Phantom connect \+ transaction trigger |
| Calculate rarity dynamically | Backend | Trait rarity % calculation logic |
| Build ownership \+ equip history | Backend | market\_events log and trait history view |
| QA test full flow | All | Full listing → purchase test across teams |

---

**PM Tips**

• Each user-facing flow (inventory, list, buy, view trait) should have a corresponding Jira story.

• Tag backend-heavy stories with backend, same for frontend.

• Use blocked status for frontend tasks waiting on API completion.

• Developers should link commits \+ PRs to their stories.

• PM can track readiness by watching /list, /buy, and /inventory endpoints hit readiness.

---

**Dev Specification: Candy Mint Launchpad (User-Facing)**

**Version:** v1

**Feature Owner:** CTO

**Technical Leads:** Backend Dev, Frontend Dev

**PM Support:** Project Manager

**Target Completion:** May 1, 2025

---

**Module Overview**

This module allows users to mint dynamic, customized NFTs from Candy-partnered Solana collections. It includes:

• A launchpad landing page

• A trait customization interface

• Real-time image generation from user-selected traits

• Metadata generation and upload

• Minting logic to create and send the NFT to the user’s wallet

• Syncing that NFT to the user’s Candy account backend

---

**📦 Scope & Assumptions**

• Collection setup (trait definitions, pricing, stock) is handled by a separate creator dashboard system (out of scope).

• All trait images exist as layered PNGs with transparent backgrounds.

• The launchpad supports dynamic metadata generation per NFT (not pre-generated).

• NFTs must be minted using compliant Metaplex metadata and Solana best practices to ensure marketplace compatibility.

• Trait inventory is limited by supply caps defined per trait.

---

**🔧 Backend Responsibilities**

**1\. Image Generation Pipeline**

• Stack selected PNGs based on collection layer order.

• Output single PNG with transparent background.

• Save file locally or buffer for upload.

**2\. Metadata Generation**

• Generate Metaplex-compliant JSON using:

• Trait values (type \+ value pairs)

• IPFS URL of generated image

• Collection name, symbol, creators, royalty, etc.

• Ensure uniqueness per mint.

**3\. IPFS Upload Service**

• Upload generated PNG \+ metadata JSON to:

• Pinata (via API key auth), or

• DigitalOcean Spaces (using CDN endpoint)

• Return public image\_url and metadata\_url.

**4\. Mint Handler (Solana)**

• Use Metaboss CLI or custom mint signer (Docker-based or RPC)

• Mint NFT to connected wallet with:

• Name

• URI (from uploaded metadata)

• Collection assignment

• Verified creator

• Royalty / seller\_fee\_basis\_points

• Ensure collection NFT metadata is set correctly

**5\. Trait Stock Enforcement**

• On mint attempt:

• Lock/validate trait availability (e.g., if “Red Hoodie” has 0 supply left, reject)

• Traits should decrement supply post-successful mint.

**6\. Candy Backend Sync**

• Store newly minted NFT:

• NFT public key

• user\_id

• metadata\_url

• image\_url

• selected trait IDs

• Used for later inventory tracking \+ customizations

---

**🎨 Frontend Responsibilities**

**1\. Launchpad Page**

• Display all active or upcoming collections:

• Title, art, description

• Mint button with whitelist/wallet gating

• Countdown to mint start

• Supply progress bar

**2\. Trait Customization Interface**

• Load available traits for collection via API

• Display categories (e.g., background, headwear)

• Show image previews \+ availability

• Optional point system:

• Display current point total

• Show per-trait cost

• Deduct on selection

• “Randomize” button to auto-generate combo

• Live preview panel of final NFT

**3\. Mint Flow**

• User presses “Mint” button

• Trigger API:

• Submit trait selections

• Begin image \+ metadata generation

• Upload to IPFS

• Call backend mint handler

• Display modal: “Uploading…”, “Minting…”, “Success\!”

• Show final minted image \+ Solana tx link

**4\. Error States**

• Trait out of stock

• Upload failed

• Mint failed

• Wallet disconnected

---

**📡 API Routes**

| Method | Endpoint | Purpose |
| ----- | ----- | ----- |
| GET | /collections/:id/traits | Fetch available traits for mint customization |
| POST | /mint | Accept user selections, generate metadata/image, mint NFT |
| POST | /mint/reserve-traits | Optional: reserve trait stock pre-mint |
| GET | /mint/:tx\_id/status | Poll mint status |
| POST | /mint/finalize | Sync minted NFT with backend and user account |

---

**📁 Data Schema Updates (Needs adjustment)**

• nfts table:

• nft\_id (Solana pubkey)

• user\_id

• collection\_id

• metadata\_uri

• image\_url

• minted\_at

• minted\_traits table:

• nft\_id

• trait\_item\_id

• trait\_type\_id

• trait\_items table:

• Add stock column

• Add locked bool (for reservation system)

---

**🧠 Suggested Development Flow**

**Week 1–2**

**Backend**

• Set up PNG stacking logic (Pillow, Sharp, etc.)

• Metadata JSON generation

• IPFS integration

**Frontend**

• Launchpad overview page

• Collection config fetcher

• Basic trait selector grid \+ preview

**Week 3**

**Backend**

• Mint handler (Metaboss integration or custom)

• Trait reservation and locking logic

• Trait stock validation

**Frontend**

• Final preview \+ randomizer

• Mint button → full flow

• Mint progress/status modals

**Week 4**

• Error handling

• QA mint flow

• Connect wallet \+ backend sync testing

• Deploy and test with dummy collection

---

**🗂 Sample Jira Structure**

**Epic:** Mint Launchpad MVP

**Stories → Tasks:**

| Story | Team | Task |
| ----- | ----- | ----- |
| Launchpad Overview Page | Frontend | Grid UI for live collections |
| Trait Customization UI | Frontend | Build category tabs, trait cards, preview panel |
| Image Stack Engine | Backend | Layer PNGs → generate preview image |
| Metadata Builder | Backend | Format \+ upload metadata to IPFS |
| Minting Function | Backend | CLI mint to Solana |
| Mint Status Flow | Frontend | Uploading → mint → confirm modals |
| Point-Based Trait System | Full Stack | Optional story w/ backend config and point logic |
| Inventory Sync | Backend | Link NFT to user\_id \+ traits |
| Trait Stock Locking | Backend | Lock traits on selection, decrement on mint |
| QA \+ Smoke Tests | All | Run 10+ test mints with random traits |

---

**PM Notes**

• All frontend trait logic should block the mint button unless selections are valid.

• Backend must enforce trait compatibility, stock, and uniqueness.

• Consider caching common trait combinations to speed up image rendering.

• PM should tag each frontend and backend task for sprint assignment, and track blocked status on mint handler.

---

Perfect. Here’s your **Dev Specification (Dev Spec)** for the **Candy Creator Launcher Tool**. This document is written for your **development team and project manager** to break the feature into actionable work, define responsibilities across frontend and backend, and prepare it for Jira epics, stories, and task assignments. I’ve omitted timelines as requested, and kept it focused on execution-ready segmentation.

---

**Dev Specification: Candy Creator Launcher Tool**

**Version:** v1

**Feature Owner:** CTO

**Intended Audience:** Backend Team, Frontend Team, PM

---

**🎯 Module Purpose**

The **Creator Launcher Tool** allows internal team members (and eventually external creators) to configure a Candy-powered NFT collection. This one-time setup defines everything needed for a successful mint: trait structure, pricing, rarity, stock, naming logic, and on-chain collection creation.

---

**🧱 Core Functional Areas**

**1\. Collection Metadata Setup**

• Name

• Symbol

• Description

• Naming format: customizable logic (e.g., Name (\#47))

• Enable naming on mint?

• Enable renaming post-mint?

• Banner and thumbnail upload

• Creator wallet capture (to link user with collection)

---

**2\. Trait Asset Upload & Organization**

**Frontend**

• Upload folder with trait assets

• Validate structure:

```
traits/
  [trait_type]/
    [trait_name].png
```

• UI parses folder → list view of trait types and individual assets

**Backend**

• Parse folder

• Assign:

• Trait type → nft\_traits

• Trait item → trait\_items

• Store initial config in Supabase

• Return data structure for UI configuration

---

**3\. Trait Configuration UI**

**Frontend**

• Render dynamic table per trait type:

• Trait name

• Preview image

• Stock (or “unlimited” toggle)

• Point cost

• Price in SOL

• Rarity percentage (if randomized)

• Dropdown or badge picker for rarity presets (e.g., “Common”, “Rare”, etc.)

• Trait incompatibility UI:

• Open modal → search by trait name → multi-select

• Save incompatible mappings

**Backend**

• Store all trait config in:

• nft\_traits

• trait\_items (for fixed stock)

• trait\_incompatibilities table (new if needed)

• Validate conflicts on finalize

---

**4\. Minting Logic Configuration**

**Frontend**

• Toggle: random vs custom mint

• If custom:

• Set point allocation per mint

• Choose between:

• Points-only

• Price-only

• Hybrid (SOL \+ points)

• Mint currency dropdown (defaults to SOL)

• Launch date/time picker

• Whitelist importer:

• Upload email or wallet CSV

• Optional: add condition (e.g., must hold X NFT)

**Backend**

• Store config under collections table

• Parse and attach allowlist rules to mint\_access\_rules

• Prepare config to inform launchpad logic and mint validator

---

**5\. Preview & Testing**

**Frontend**

• Real-time visual preview panel

• “Randomize” button for randomized mint simulation

• “Customize” mode to test trait selection UI

• Highlight broken/incompatible combos (based on config)

• Layer order preview (using PNG layering engine or static previews)

**Backend**

• Serve preview trait images from temporary cache

• Reuse preview system from mint flow if possible

• Validate selected combos against configured incompatibility logic

---

**6\. Finalization & Generation**

**Frontend**

• Show final summary of:

• Collection info

• Trait types

• Pricing and rarity model

• Trait counts

• Preview panel

• “Confirm & Finalize” button triggers asset upload and DB write

**Backend**

• Resize \+ upload trait images to DO Spaces in three sizes:

• selection (e.g., 80x80px)

• preview (300px+)

• full (mint-size, transparent PNG)

• Generate all DB entries:

• collections

• nft\_traits (trait types)

• trait\_items (stock-based traits)

• pricing\_rules

• rarity\_schema

• Flag collection as ready\_to\_mint \= true

• Assign creator’s wallet to collection

---

**7\. Solana Collection Deployment**

**Backend**

• Use Metaboss or internal CLI

• Create:

• Collection NFT

• Collection metadata

• Update authority

• Set collection as “verified” per Metaplex standards

• Save Solana mint address and collection ID to collections table

• Assign update authority:

• Candy wallet, or

• Through Turnkey wallet delegation

---

**📡 API Endpoints**

| Method | Route | Purpose |
| ----- | ----- | ----- |
| POST | /creator/launcher/parse-traits | Accept and organize uploaded folder |
| POST | /creator/launcher/save-config | Save current trait and collection settings |
| POST | /creator/launcher/finalize | Trigger upload, DB write, and asset deployment |
| GET | /creator/launcher/preview | Return randomized/custom preview mock NFT |
| POST | /creator/launcher/validate-combo | Check a combination of traits for compatibility |
| POST | /creator/launcher/deploy-solana | Final Solana collection creation logic |

---

**🗃️ Supabase Schema Updates**

**collections**

• id, name, symbol, description, creator\_wallet, ready\_to\_mint, mint\_type, etc.

**nft\_traits**

• id, collection\_id, type\_name, rarity\_category, point\_cost, price, is\_static, etc.

**trait\_items**

• id, trait\_id, image\_url, is\_unlimited, stock\_remaining, etc.

**trait\_incompatibilities (new)**

• trait\_id, incompatible\_with\_trait\_id

**mint\_access\_rules**

• collection\_id, wallet\_list, condition\_type, token\_gate\_type, etc.

---

**🧠 Suggested Dev Flow (No Dates)**

**Phase 1: Foundation**

• Trait upload parser

• Folder structure validator

• DB schema setup

**Phase 2: Config Interface**

• Trait config tables

• Point/pricing toggles

• Incompatibility UI

• Preview renderer

**Phase 3: Finalization**

• Image resizing \+ uploader

• DB write integration

• Confirmation/locking logic

**Phase 4: Solana Deployment**

• CLI collection deploy

• Save collection mint to DB

• Assign update authority

---

**🔑 PM Notes**

• Trait folder validation \+ UI is a top priority

• Preview tools should support random \+ manual test modes

• All collection data must be finalizable and immutable once locked

• Backend must be ready to handle all trait generation logic for mint flow after this is complete

• Setup should allow creators to move directly into mint launchpad \+ post-mint dashboard

---

Absolutely—here’s a **top-level Jira Epic breakdown** of the **Candy Creator Launcher Tool**, formatted to give your PM a clean jump-off point for sprint planning. Each epic includes suggested story-level items to break out as user stories or dev tasks.

---

**🧩 JIRA Structure: Creator Launcher Tool**

---

**Epic: Collection Info & Creator Setup**

**Description:**

Capture all basic information about the collection (metadata, naming, creator wallet, banner image, naming rules).

**Stories / Tasks:**

• Create collection metadata form (name, symbol, description)

• Implement image upload (banner, thumbnail)

• Add creator wallet capture logic

• Add toggles for “nameable” and “renameable” settings

• Persist collection draft data in Supabase

---

**Epic: Trait Upload & Organization**

**Description:**

Enable creators to upload a folder of trait assets and automatically organize by trait type.

**Stories / Tasks:**

• Build drag-and-drop upload component

• Parse folder structure into trait types and traits

• Display trait list by type in table UI

• Validate folder structure and filenames

• Send parsed trait data to backend for temporary staging

---

**Epic: Trait Configuration UI**

**Description:**

Allow creators to configure pricing, stock, rarity, and compatibility per trait.

**Stories / Tasks:**

• Build dynamic table interface for trait configuration

• Add toggles for limited/unlimited stock

• Add inputs for point cost and price in SOL

• Add dropdowns for rarity presets (e.g., Common, Legendary)

• Implement incompatibility modal (search and select incompatible traits)

• Connect config changes to backend save endpoint

---

**Epic: Mint Logic Configuration**

**Description:**

Define how the collection will mint: randomized or custom, point-based or SOL, and set mint schedule and access rules.

**Stories / Tasks:**

• Add mint type toggle (random vs custom)

• Input for point allocation per mint (if custom)

• Trait pricing logic UI (point/fixed/hybrid)

• Upload allowlist CSV or select gating logic

• Add mint start date/time selector

---

**Epic: Preview & Testing Tools**

**Description:**

Allow creators to preview trait combinations visually before finalizing.

**Stories / Tasks:**

• Build preview window with layered trait rendering

• Add “Randomize” button for randomized mode

• Add “Customize” preview mode for manual testing

• Highlight missing or broken trait combinations

• Show incompatibility warnings

---

**Epic: Finalization & Confirmation**

**Description:**

Lock the collection, upload assets, populate database, and prepare for launch.

**Stories / Tasks:**

• Build final summary UI

• Connect “Confirm & Finalize” button to backend action

• Resize trait assets into multiple sizes

• Upload all assets to DO Spaces

• Create nft\_traits, trait\_items, and collections entries in DB

• Set ready\_to\_mint \= true

---

**Epic: Solana Collection Deployment**

**Description:**

Deploy the collection on-chain with correct metadata and assign update authority.

**Stories / Tasks:**

• Connect to Metaboss or internal CLI

• Create collection NFT \+ metadata

• Verify collection \+ assign update authority

• Save Solana mint address and metadata URI in Supabase

---

**Epic: Backend Integration & Infrastructure**

**Description:**

Supporting infrastructure and endpoints for the launcher tool to function.

**Stories / Tasks:**

• Create API for trait parsing, config saving, and finalization

• Create collection deploy endpoint

• Set up Supabase tables: collections, nft\_traits, trait\_items, trait\_incompatibilities, mint\_access\_rules

• Handle wallet connection via Turnkey

• Set up upload pipeline to DigitalOcean Spaces

---

**Epic: QA & Validation**

**Description:**

Final testing of trait alignment, logic enforcement, and Solana mint success.

**Stories / Tasks:**

• Run test launches with dummy collection data

• Validate trait stacking/rendering accuracy

• Confirm all trait configs respected in output

• Test wallet connection, metadata generation, and NFT visibility on Solana marketplaces

---

---

**🧩 JIRA Structure: Candy Mint Launchpad**

---

**Epic: Mint Launchpad Interface**

**Description:**

Build the main public-facing mint landing pages for each Candy-powered collection.

**Stories / Tasks:**

• Build collection mint page layout (title, description, banner, supply, countdown)

• Add mint access control logic (based on launch time and allowlist)

• Create “Mint Now” call-to-action and status modals (Not Live, Live, Sold Out)

• Display pricing, mint status (per user), and collection info

---

**Epic: Trait Customization Flow**

**Description:**

Enable users to select traits for their NFT during the mint process (if customization is enabled).

**Stories / Tasks:**

• Create trait picker interface with categories (e.g., head, shirt, background)

• Add stock indicators and disabled state for unavailable traits

• Implement “Randomize” button logic

• Display point allocation tracker (if point-based)

• Show final preview window with composite image

---

**Epic: Image & Metadata Generation (Mint Time)**

**Description:**

Generate artwork and metadata based on user selections at the moment of mint.

**Stories / Tasks:**

• Layer trait images into a composite PNG

• Generate Metaplex-compliant metadata JSON

• Upload image and metadata to IPFS (Pinata or DO Spaces)

• Return metadata URI to trigger mint

---

**Epic: Mint Execution (Solana)**

**Description:**

Handle the actual mint transaction on Solana using the generated metadata.

**Stories / Tasks:**

• Connect Phantom wallet and request mint signature

• Call backend to mint NFT with:

• Metadata URI

• Collection ID

• Verified creator, royalties, etc.

• Confirm success or failure

• Handle edge cases (mint fails, trait out of stock, disconnected wallet)

---

**Epic: Post-Mint Sync**

**Description:**

Ensure minted NFTs are synced with Candy backend and associated with the user account.

**Stories / Tasks:**

• Detect minted NFT in wallet post-transaction

• Link minted NFT to Candy user\_id in Supabase

• Save traits used at mint time to minted\_traits

• Log timestamp, collection, and metadata URI

---

**Epic: Mint Access Control (Whitelist, Gating)**

**Description:**

Support different access models for mint eligibility.

**Stories / Tasks:**

• Upload and validate allowlist CSV (emails, wallet addresses)

• Add rule-based eligibility (e.g. must hold X NFT)

• Support multiple tiers (e.g., early access, public)

• Show appropriate messaging to user depending on tier

---

**Epic: UX Polishing & Testing**

**Description:**

Finalize the flow for production use.

**Stories / Tasks:**

• Add progress/loading animations

• Add error handling for metadata/image upload issues

• Full QA run for random and customized mint types

• Test visibility of minted NFTs on marketplaces (ME, Tensor)

---

---

**🧩 JIRA Structure: Trait Secondary Marketplace**

---

**Epic: Trait Marketplace UI**

**Description:**

Create the public-facing marketplace page to browse, search, and purchase traits.

**Stories / Tasks:**

• Build collection-specific trait marketplace view

• Add trait filters (type, collection, rarity, price)

• Display trait cards with image, price, availability

• Add pagination or infinite scroll

---

**Epic: Trait Listing Flow**

**Description:**

Enable users to list unequipped traits for sale.

**Stories / Tasks:**

• Add “List for Sale” modal from user inventory

• Set price and confirm listing

• Lock listed trait to prevent use in customization

• Save listing to backend with escrow-like structure

---

**Epic: Trait Purchase Flow**

**Description:**

Allow users to buy listed traits from others.

**Stories / Tasks:**

• Show listing detail with trait info, history, price chart

• Implement “Buy Now” button and wallet signature

• Handle payment and transfer of ownership

• Mark listing as sold and update trait ownership in backend

---

**Epic: Trait Detail & History Panel**

**Description:**

Enable deep view into a trait’s stats and historical usage.

**Stories / Tasks:**

• Display trait rarity, total supply, and equip history

• Show marketplace price chart (basic line graph)

• Show bid/offer history (future optional)

• Display which NFTs have worn this trait (equip history)

---

**Epic: Backend Listing Engine**

**Description:**

Handle listing creation, validation, and transactions in the backend.

**Stories / Tasks:**

• Create listings table with reference to trait\_items

• Implement listing state: active, sold, canceled

• Block trait usage during listing

• Handle ownership update and log transfer history

---

**Epic: Inventory Management \+ Sync**

**Description:**

Ensure traits in users’ inventory are always accurate and available for marketplace interaction.

**Stories / Tasks:**

• Create inventory fetcher per user (owned unequipped traits)

• Sync post-purchase changes to inventory

• Handle trait unequip → available for listing

• Handle trait purchase → assign to buyer inventory

---

**Epic: Trait Offer System (Optional/Future)**

**Description:**

Allow users to place offers on traits that are not listed.

**Stories / Tasks:**

• Build offer modal on NFT detail view

• Store open offers in DB

• Notify trait owner of incoming offer

• Enable accept/reject flow (future phase)

---

**Epic: Security \+ Permissions**

**Description:**

Ensure all marketplace actions are validated and secure.

**Stories / Tasks:**

• Prevent unauthorized listing of equipped traits

• Prevent duplicate listing of same trait

• Verify ownership before listing or purchase

• Implement backend permission checks

---

Let me know if you’d like me to export these into a Notion/Jira-ready doc structure or if you want to move on to the **Creator Dashboard** module next.

 

---

**Dev Specification: Candy NFT Marketplace (Partner Collection Trading)**

**Version:** v1

**Feature Owner:** CTO

**Audience:** Backend \+ Frontend Devs, PM

---

**🎯 Module Purpose**

Enable buying, listing, and browsing of full Solana NFTs from Candy partner collections (Nubcats, Retardio, Skellies) within the Candy platform—**subverting third-party aggregators** and capturing marketplace fees. Includes trait-aware filtering, NFT customization history, and deep integration with the trait economy.

---

**🧱 Functional Areas**

---

**1\. Collection Marketplace Page**

**Frontend**

• Build dynamic grid of NFTs for a given collection

• Display key stats (floor price, 24h volume, listings)

• Add filters:

• By trait (e.g., Shirt: “Red Hoodie”)

• Price range

• Equipped trait (custom logic for Candy NFTs)

• Add sorting:

• Floor price (asc/desc)

• Recently listed

• Rarity / Drip Score (future-ready)

**Backend**

• Build /collections/:id/nfts API:

• Accept filters, pagination, and sort options

• Return listing status, metadata, traits, and rarity

• Fetch live collection stats from listing engine

---

**2\. NFT Listing Engine**

**Frontend**

• “List for Sale” modal from NFT card or wallet view

• Input price in SOL

• Wallet signature to authorize listing

• Show confirmation modal after listing success

**Backend**

• Create listings table:

• id, nft\_id, price, seller\_id, active, created\_at, tx\_sig, fee\_percent

• Verify:

• NFT ownership

• NFT is from an allowed partner collection

• Optional: lock or mark NFT as “listed” in Candy DB to avoid trait changes

• Process listing fee logic on sale (e.g., 2–3%)

---

**3\. NFT Purchase Flow**

**Frontend**

• “Buy Now” modal on NFT detail view

• Show final cost (price \+ fees)

• Connect wallet and sign transaction

• Confirmation state on success or fail

**Backend**

• Validate listing is still active and price matches

• Transfer NFT to buyer wallet (via Candy wallet if needed)

• Handle fee transfer and royalty splitting

• Update:

• listings → active \= false

• Log sale in sales table

• Update NFT ownership in nfts or user\_nfts table

---

**4\. NFT Detail Modal / Page**

**Frontend**

• Show:

• Large preview image

• Trait breakdown grid (with hover rarity info)

• Trait card click-through to trait detail

• Price chart (basic line or bar graph)

• Listing \+ purchase UI (if listed)

• NFT evolution timeline for Candy NFTs

• Show trait rarity % \+ total supply \+ equipped %

**Backend**

• Serve /nft/:id API:

• Include metadata, traits, listing info, sale history, customization history

• Image history:

• Retrieve list of historical metadata URIs or rendered image URLs

---

**5\. Customization History Timeline (Candy NFTs only)**

**Frontend**

• Carousel or modal viewer for NFT visual history

• Each item \= visual snapshot of NFT (image) with timestamp

• Expandable gallery for full-screen view

**Backend**

• Add nft\_customization\_log table:

• nft\_id, image\_url, metadata\_uri, timestamp, trait\_ids\[\]

• Trigger this log on every customization / trait change

---

**6\. Trait-Aware UI**

**Frontend**

• Hover over traits shows:

• Supply

• Equipped count

• Rarity %

• Click through to:

• Trait detail popup or full page (from trait marketplace)

• Trait marketplace → buy/sell that individual trait

• Enable filtering “NFTs with this trait” from marketplace grid

**Backend**

• Compute rarity % \= (trait items owned or equipped) / total supply

• Store in trait indexer service or calculate on-demand

• Enable /trait/:id/nfts route to fetch NFTs equipped with specific trait

---

**7\. Optional / Future Features**

• **Offers & Bidding:**

• Store open bids on listed and unlisted NFTs

• Accept / reject functionality

• **Wallet inventory sync:**

• Regularly refresh NFT wallet ownership

• Delist NFTs automatically when sold outside Candy

• **Gamified Trading Layer:**

• Track volume traded per user

• Reward tiers or trading achievements

---

**📡 API Endpoints**

| Method | Endpoint | Purpose |
| ----- | ----- | ----- |
| GET | /collections/:id/nfts | Fetch marketplace NFTs for collection |
| GET | /nft/:id | Get NFT metadata, traits, listings, sale history |
| POST | /listing/create | List NFT for sale |
| POST | /listing/buy | Purchase listed NFT |
| GET | /trait/:id/nfts | Get NFTs that have specific trait |
| GET | /nft/:id/history | Return NFT customization history |
| GET | /listing/:id/price-chart | Return price chart data for given NFT |

---

**🗃️ Supabase Schema Additions**

**listings**

• id, nft\_id, price, seller\_wallet, active, created\_at, fee\_percent, tx\_sig

**sales**

• id, nft\_id, buyer\_wallet, seller\_wallet, price, timestamp, collection\_id

**nft\_customization\_log**

• id, nft\_id, image\_url, metadata\_uri, timestamp, trait\_ids\[\]

---

**🧠 Suggested Dev Flow (No Timelines)**

**Phase 1: Setup \+ Schema**

• Define listings, sales, nft\_customization\_log tables

• Set up collection \+ NFT fetching endpoints

**Phase 2: Frontend Grid \+ Listing**

• Collection marketplace grid with filters

• List NFT flow \+ confirmation

• NFT detail modal: preview, traits, price, and listing

**Phase 3: Trait-Aware Features**

• Hover rarity tooltips

• Trait click-through

• Filter by trait logic

**Phase 4: Purchase Flow \+ History**

• Implement buy logic

• Add ownership sync \+ listing invalidation

• Build customization history timeline gallery

**Phase 5: QA \+ Incentives**

• End-to-end test: list → buy → history update

• PM can define leaderboard/incentive rules

---

**🧱 Suggested Jira Epics (Preview)**

• **NFT Marketplace Grid \+ Filtering**

• **NFT Detail Modal \+ Trait Display**

• **Listing Engine (Backend \+ Frontend)**

• **Purchase Engine**

• **Customization History Timeline**

• **Trait Search \+ Trait Linking**

• **Database \+ Indexer Infrastructure**

• **Wallet Integration \+ Signing**

• **QA \+ Integration Testing**

---

Let me know when you’re ready to dive into the **Creator Dashboard**, or want to start ticketing out any of these specs. You’re building a serious platform here—this is gonna make waves.

