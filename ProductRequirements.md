**Product Requirements Document (PRD)**

**Feature:** Trait Secondary Marketplace

**Version:** v1

**Last Updated:** March 29, 2025

**Owner:** CTO

**Collaborators:** COO, Product Manager, Backend Engineer, Frontend Developer

---

**1\. Overview**

The Trait Secondary Marketplace allows users to buy, sell, and trade individual traits (shirts, hats, backgrounds, consumables, etc.) from Candy-partnered NFT collections. Traits are discrete, trackable digital items stored and managed within Candy’s database. While not NFTs themselves, they are treated as assets with ownership, rarity, and marketplace behaviors.

This marketplace enables dynamic NFT customization and fuels a core differentiator of the Candy platform: composability of partner NFT collections. This PRD outlines the functionality, technical structure, and user experience required to deliver this marketplace as part of the MVP launch.

---

**2\. Goals**

• Enable peer-to-peer trading of unequipped traits via listings and direct purchases.

• Support trait listing, browsing, filtering, and purchasing on a collection-level and global marketplace.

• Maintain a secure escrow model for trait listings and transactions.

• Display trait metadata, ownership history, usage stats, and price history.

• Track real-time rarity and supply changes based on equipped/unequipped state.

• Lay groundwork for gamified functionality (drip scores, leaderboards, analytics).

---

**3\. Success Criteria**

• Users can list unequipped traits from their inventory for sale.

• Other users can view and purchase listed traits using connected wallets.

• Trait ownership is securely transferred upon sale.

• Inventory and equipped states are correctly updated post-transaction.

• Trait listing interfaces and detail pages display real-time data.

• Admins and creators can manage trait data, listings, and airdrops.

• System is performant and scalable across multiple collections.

---

**4\. Users & Personas**

**Collectors**

• Want to customize their NFTs

• Buy and sell traits for collection completion, aesthetics, or profit

**Traders**

• Use the market to speculate on trait rarity and value

• Want price data, filtering, and quick listing flows

**Collection Creators**

• Want users to engage deeply with their NFTs

• Need dashboard tools to manage trait releases and redemptions

**Admins**

• Oversee system integrity

• Create and manage new trait inventory

• Approve/resolve marketplace issues

---

**5\. Features & Functionality**

**5.1 Listing Traits for Sale**

• Users can list any **unequipped** trait item in their inventory for sale at a fixed price.

• Trait becomes escrowed: no changes or customization can occur while listed.

• Users can cancel the listing any time before purchase.

**5.2 Browsing & Purchasing**

• Traits are browsable:

• Globally (all Candy traits)

• By collection

• By trait type (shirt, background, etc.)

• Buyers can filter traits by:

• Type

• Rarity

• Price range

• Equipped history

• Origin (mint/shop/airdrop)

• Buyers can purchase a listed trait using supported tokens (e.g., SOL).

• Once purchased, trait is transferred to buyer’s inventory and removed from seller’s.

**5.3 Trait Detail View**

• Trait image

• Collection name

• Trait type and subtype

• Price history chart

• Ownership history

• NFTs the trait has been equipped to

• Supply and rarity stats (total existing, total equipped, total unequipped)

• “Buy” or “Make Offer” actions

**5.4 Inventory & Customization Integration**

• Traits must be unequipped to be listed.

• Equipping a trait removes it from the user’s inventory (assigns it to an NFT).

• Unequipping moves it back to inventory, where it becomes tradable again.

**5.5 Rarity & Analytics Engine**

• Rarity % calculated dynamically:

• Total number of that trait in existence

• How many are equipped

• How many are listed

• Drip scores and other gamified systems will rely on this data.

**5.6 Creator Distribution Tools**

• Collection creators can:

• Add new traits

• Set pricing and stock

• Issue airdrops to holders

• Generate redeemable codes

• Track sales and inventory

• Traits not purchased remain removable; once sold, items become permanent in the database.

---

**6\. Optional / Future Features (Phase 2+)**

• Bidding system for traits

• Direct trait-to-trait trades with same-type conditions

• Trait offers for equipped items (without listing)

• Cross-collection trait trading

• Burn/re-roll mechanics to combine traits into randomized rare traits

• Consumable traits (e.g., name change potions)

• API endpoints for conditional redemptions (QR codes, third-party apps)

---

**7\. Technical Requirements**

**7.1 Backend**

• Trait listing system with escrow

• Ownership transfer logic

• Price tracking service

• Trait history logging

• Supply \+ rarity calculation logic

• Inventory \+ equipped state management

• Secure API for redemption codes and airdrops

**7.2 Frontend**

• Market listing UI (global \+ per collection)

• Trait detail modal/page

• Listing modal (from inventory)

• Inventory management UI

• Filtering/sorting interface

• Purchase flow with connected wallet

**7.3 Database Schema (Core)**

• trait\_items

• nft\_traits

• trait\_types

• users

• listings

• transactions

• market\_events (optional for analytics)

---

**8\. Dependencies & Integrations**

• Supabase (PostgreSQL) for trait ownership and listings

• WalletConnect/Phantom for user wallet actions

• Candy frontend (React/Next.js \+ Tailwind)

• DigitalOcean Spaces / Pinata for image hosting

• Helius (Solana RPC)

• Internal user auth/session system

• Admin and Creator dashboards (connected via internal API)

---

**9\. Open Questions**

• Will auctions be part of the MVP or come post-launch?

• Should we enable “offers” for equipped traits from day one?

• Will we introduce trading with currency *plus* a trait (hybrid direct trade)?

• What rarity/scoring formulas will power drip scores?

• Should trait analytics be computed in real-time or batch processed?

---

**10\. Risks & Mitigations**

| Risk | Mitigation |
| ----- | ----- |
| Trait ownership mismatch on wallet change | Revalidate ownership at session start and on blockchain sync |
| Trait equipped/unequipped logic bugs | Comprehensive unit tests and equip validation system |
| High database writes (equips, unequips, listings) | Rate limit and index critical DB fields |
| Poor user onboarding to trait system | Step-by-step UI, documentation, and walkthroughs |

---

**11\. Milestones**

| Milestone | Target Date |
| ----- | ----- |
| Finalize schema \+ routes | 4.18.25 |
| Frontend listing UI complete | In parallel w/ FE developer: 4.23.25 |
| Backend transfer \+ escrow complete | 5.2.25 |
| MVP release of trait marketplace | 5.9.25 (QAT/Security testing) |

---

**Product Requirements Document (PRD)**

**Feature:** Candy Mint Launchpad (User-Facing)

**Version:** v1

**Last Updated:** April 4, 2025

**Owner:** CTO

**Collaborators:** Product Manager, COO, Backend & Frontend Devs

---

**1\. Overview**

The Candy Mint Launchpad allows users to mint NFTs from Candy-partnered collections directly from our platform. Unlike traditional launchpads, each NFT minted through Candy is user-customized at the moment of mint. Users will select from a set of traits defined by the collection creator, and the resulting artwork and metadata are generated dynamically and then minted onto the Solana blockchain with the correct collection metadata.

This launchpad differentiates Candy’s offering by integrating deep composability and gamified customization at the mint phase, creating a novel user experience and high customization value.

---

**2\. Goals**

• Provide a standard launchpad interface for users to discover and mint new Candy-powered collections.

• Allow users to fully customize their NFT before mint via a trait picker.

• Dynamically generate the composite NFT image and metadata at mint time.

• Upload the generated image and metadata to IPFS (or internal CDN).

• Mint the customized NFT using the user’s wallet and connect it to the correct Solana collection.

• Ensure the NFT is tradable on secondary marketplaces and linked to the user’s backend Candy account.

---

**3\. Success Criteria**

• Users can customize an NFT during the minting flow with no performance or availability issues.

• Metadata and images are correctly generated and uploaded before mint.

• Minted NFTs display correctly in wallets and marketplaces (Magic Eden, Tensor).

• The NFT is associated with the correct collection and update authority.

• NFTs are linked to users’ Candy accounts post-mint.

• Trait availability is accurately enforced (e.g., limited stock traits cannot be minted once sold out).

• Optional point-based customization system can be configured per collection.

---

**4\. Users & Personas**

**Minters / Collectors**

• Want to personalize their NFT at mint

• Interested in unique customization

• Value visual quality, rarity, and scarcity

**Traders**

• Want to mint quickly and at volume

• Prefer randomization to fast-track bulk mints

**Candy Platform Admins**

• Monitor successful mints

• Ensure backend association between minted NFTs and user accounts

---

**5\. Features & Functionality**

**5.1 Launchpad Overview Page**

• Displays upcoming or live Candy-powered mints

• Shows collection art, descriptions, supply, mint price

• Countdown timer

• “Mint Now” button

**5.2 Customization Interface (Pre-Mint)**

• Trait preview panel

• Trait selectors by category (e.g., shirt, background, eyes)

• Stock indicators for limited traits

• Optional point system (user has X points to spend, traits cost Y points)

• Randomize button

• Final preview viewer

**5.3 Image \+ Metadata Generation**

• On user mint confirmation:

• Stack selected trait PNGs to create composite image

• Generate metadata with:

• Trait attributes

• IPFS image link

• Collection info, symbol, creator

• Upload to IPFS (via Pinata or internal CDN)

• Store generated metadata URI

**5.4 Minting Flow**

• Sign and send transaction from connected wallet

• Mint NFT into correct collection with:

• Collection ID

• Update authority

• Verified creator field

• Store minted NFT in user’s wallet

• Match NFT to Candy account (via wallet ID)

**5.5 Post-Mint Handling**

• Confirm NFT in wallet

• Show success modal \+ link to view NFT

• Update Candy backend:

• NFT ID

• Owner (user\_id)

• Metadata URI

• Trait data association

---

**6\. Optional / Future Features (Phase 2+)**

• Bulk minting / batch randomizer

• Trait “rarity reveal” post-mint (like box-opening experience)

• Trait-level royalties

• Post-mint airdrops based on traits selected

• Redemption of leftover mint points into Candy Shop credits

---

**7\. Technical Requirements**

**7.1 Backend**

• Metadata generation service (based on trait inputs)

• Composite image generation pipeline

• IPFS uploader (Pinata / DigitalOcean Spaces integration)

• Mint handler (either Metaplex-compatible or custom)

• API to associate newly minted NFT to user account

• Trait inventory and stock tracking

**7.2 Frontend**

• Launchpad UI with mint access gating (countdown, whitelist)

• Trait selector interface with category tabs, stock display

• Preview renderer (composited view from selected trait images)

• Mint button with wallet connection

• Status modals (minting, uploading, success)

• Randomizer button logic

---

**8\. Dependencies & Integrations**

• Supabase (User accounts, NFT tracking, trait inventory)

• Pinata or DigitalOcean (Image \+ metadata hosting)

• Metaboss / Custom mint function (Solana on-chain mint)

• WalletConnect / Phantom (wallet signing)

• Cloud functions or Lambdas (Image/metadata generation)

---

**9\. Open Questions**

| Topic | Notes |
| ----- | ----- |
| IPFS vs internal image CDN | Are we ready to host image \+ metadata via Spaces instead of Pinata? |
| Minting architecture | Should we integrate Candy Machine v3 or roll custom mint logic for full control? |
| Trait locking | Do we “lock” trait stock immediately on selection or only after mint confirmation? |
| Points system | How configurable is the per-collection logic for mint points and pricing? |
| Race conditions | How do we prevent two users from minting the same limited trait simultaneously? |

---

**10\. Risks & Mitigations**

| Risk | Mitigation |
| ----- | ----- |
| Minting delays due to image generation | Pre-generate images for common trait combos, use cache system |
| Trait stock race conditions | Lock trait stock on client-side \+ confirm with backend reserve |
| Metadata rejected by marketplaces | Ensure Metaplex schema compliance and test on ME/Tensor |
| Wallet disconnection mid-mint | Implement state recovery and resume logic |
| Abuse of randomizer or point system | Validate trait eligibility \+ enforce mint rules server-side |

---

**11\. Milestones**

| Milestone | Target Date |
| ----- | ----- |
| Finalize trait selection UX | 5.2.25 |
| Image \+ metadata service complete | 5.7.25 |
| Mint transaction handler complete | 5.9.25 |
| Mint-to-wallet \+ backend sync stable | 5.14.25 |
| Launchpad MVP live  | 5.16.25  |

---

**Product Requirements Document (PRD)**

**Feature:** Candy Creator Launcher Tool (Internal Wizard)

**Version:** v1

**Last Updated:** March 29, 2025

**Owner:** CTO

**Collaborators:** Product Manager, Backend/Frontend Devs, Strategy Team

---

**1\. Overview**

The **Candy Creator Launcher Tool** is an internal-facing configuration wizard that enables collection creators (starting with launch partners like *Nubcats*) to fully define and deploy a Candy-powered NFT collection. It collects all necessary collection information, trait data, and minting logic, then generates the required database entries, assets, and metadata, and sets the collection up for deployment on Solana.

Though initially internal, this tool is designed to eventually become a **public-facing product** that allows new creators to easily launch trait-based, composable NFT collections on Candy.

---

**2\. Goals**

• Provide a step-by-step interface to configure a Candy NFT collection from scratch

• Allow for both **randomized mints** and **customizable-at-mint collections**

• Support **point-based pricing**, **fixed-price traits**, or **hybrid models**

• Enable creators to upload and organize trait assets by folder structure

• Automatically generate backend trait records, pricing, stock, rarity, and logic rules

• Prepare the collection for **Solana-based minting** with verified Candy metadata

• Link the collection to a creator wallet for future dashboard access

---

**3\. Success Criteria**

• Creators can define all collection-level metadata and trait configuration in one workflow

• Traits and images are uploaded and stored in DigitalOcean Spaces

• Trait types, items, rarity, supply, and rules are properly reflected in the Supabase DB

• Collection is initialized for Solana mint with correct update authority and metadata config

• Collection is marked as “ready to launch” in the Candy system

• Creators can preview trait combinations and identify misalignments before confirming

• The creator wallet is tied to the collection for dashboard access post-mint

---

**4\. Users & Personas**

**Internal Team (Now)**

• Sets up partner collections (e.g., Nubcats)

• Validates setup before mint

**Collection Creators (Soon)**

• Will use this tool directly via the dashboard to configure future drops

**Backend Systems**

• Generate trait records, stock, metadata schema, upload images

• Prepare collection for minting and trait tracking

---

**5\. Features & Functionality**

**5.1 Collection Info Setup**

• Name, symbol, description

• Banner and base image upload

• Naming structure:

• Format: “Name \#47”, “CustomName (Nub \#47)”

• Options:

• Allow naming on mint?

• Allow renaming post-mint?

**5.2 Mint Configuration**

• Mint price

• Supply

• Launch time

• Whitelist tiers (email list import or logic-based, e.g. “holders of X”)

• Currency accepted (SOL)

• Trait pricing model:

• Randomized

• Customization with trait store (point system, fixed price, or both)

**5.3 Trait Upload & Auto-Organization**

• Drag \+ drop upload of folder structured as:

```
traits/
  head/
  shirt/
  eyes/
  background/
```

• Folder names become **trait types**

• File names become **trait names**

• Interface auto-lists each trait in a table by type

**5.4 Trait Configuration UI**

For each trait and type:

• Rarity mode:

• Random collection? → assign rarity percentages

• Custom collection? → assign stock, point cost, fixed price

• Compatibility:

• UI to mark incompatible traits (search and multi-select)

• Unlimited vs limited stock toggle

• Visual preview of trait \+ naming override (optional)

• Group tagging (e.g. “Rare”, “Mythical” for UI filtering)

**5.5 Trait Pricing Models (per collection)**

• **Randomized Mint:**

• Each NFT minted gets randomized traits from trait pool based on rarity

• Traits become fixed to that NFT

• **Custom Mint:**

• Users build NFT with trait picker

• Points system: user gets X points per mint; traits have cost

• Optional trait-specific fixed price in SOL

• Creators may select mixed pricing model (e.g. point cap \+ premium paid traits)

**5.6 Preview & Validation**

• Trait compatibility preview

• Trait layering visualization

• “Randomize Preview” button for randomized collections

• “Customize Preview” mode for custom collections

**5.7 Confirmation Step**

• Summary of all configured settings

• “Finalize” button runs:

• Asset upload to DigitalOcean Spaces (in sticker, preview, full sizes)

• DB population:

• nft\_traits entries

• trait\_items (for fixed stock)

• Assign creator wallet to collections table

• Mark collection as ready for mint

• Flag collection for the creator dashboard post-launch

**5.8 Solana Deployment**

• Internal deploy of collection metadata:

• Set up verified creator, update authority

• Ensure traits follow Metaplex metadata standards

• Use Metaboss or custom CLI to deploy

• Assign update authority:

• Candy wallet (recommended)

• Or abstracted via Turnkey’s permission engine (if implemented)

---

**6\. Optional / Future Features**

• In-app trait art alignment tool (fine-tune XY position)

• Rarity simulation preview (generate 10,000 examples, plot % distributions)

• Metadata export/preview

• Compressed NFT compatibility (Solana compression support)

• “Save as draft” workflow

• Multi-wallet creator team support

• Collection collaborator roles (artist, developer, etc.)

---

**7\. Technical Requirements**

**Backend**

• Trait folder parser (convert folder structure into DB schema)

• DO Spaces upload pipeline with resizing

• Metadata generator templates for randomized vs customized

• Stock generator (create trait\_items)

• Solana collection init CLI \+ metadata assigner

• Creator wallet connection logic (via Turnkey or Phantom)

**Frontend**

• Multi-step wizard UI (collection info → trait config → preview → confirm)

• Dynamic table view per trait type

• Compatibility/incompatibility assignment modal

• Point \+ pricing toggle logic

• Preview \+ randomizer component

• “Finalize” button triggers backend workflow and locks the config

---

**8\. Dependencies**

• Supabase (DB for traits, users, collections)

• DigitalOcean Spaces (image uploads \+ public access)

• Turnkey (wallet connection, signing abstraction)

• Metaboss or custom CLI (Solana collection minting)

• Candy backend worker to process trait ingestion and finalize setup

---

**9\. Open Questions**

| Topic | Notes |
| ----- | ----- |
| Trait “locking” for unlimited items | Do we want to generate unlimited trait items on mint or define only the type? |
| Trait combinations & rarity enforcement | Will we need preview enforcement for rarity conflicts? |
| Custom metadata fields per collection | Will creators ever want to add custom metadata fields? |
| Update authority assignment | Will we default to Candy wallet \+ Turnkey signing for updates or allow creators to hold it? |

---

**10\. Risks & Mitigations**

| Risk | Mitigation |
| ----- | ----- |
| Trait folders not structured properly | Clear upload instructions \+ validator UI |
| Rarity overlaps or impossible traits | Compatibility/rarity warnings in preview mode |
| Solana deployment fails | Dev tooling \+ retry logic \+ backend flags for retry state |
| Users bypass naming constraints | Enforce naming system on backend at mint-level |
| Creators overwhelmed by config UI | Offer presets \+ “Simple/Advanced” mode toggles |

---

**11\. Milestones**

| Milestone | Target Date |
| ----- | ----- |
| Folder parser \+ trait interface ready | 4.18.25 |
| Preview \+ rarity config complete | 4.23.25 |
| Finalization \+ upload pipeline stable | 4.28.25 |
| Solana collection deployment complete | 5.2.25 |
| Nubcats collection configured with launcher | 6.1.25 |

---

Let me know when you’re ready for the **PSD** for this module, or if you’d like to go straight into the **Dev Spec** so we can break this into Jira-like stories and frontend/backend responsibilities.

---

**Product Requirements Document (PRD)**

**Feature:** Candy NFT Marketplace (Partner Collection Trading)

**Version:** v1

**Last Updated:** April 4, 2025

**Owner:** CTO

**Collaborators:** PM, Backend Devs, Frontend Devs, Product Strategy

---

**1\. Overview**

The **Candy NFT Marketplace** enables users to buy, sell, and trade full NFTs from Candy partner collections (e.g., Nubcats, Skellies, Retardio Cousins) directly on our platform. Unlike traditional Solana marketplaces like Magic Eden or Tensor, this marketplace is **tailored to Candy-powered NFTs**, offering enhanced UX, trait-aware tools, and NFT customization history.

The system will be designed to **subvert traditional aggregators** where possible—either by using a custom lightweight listing engine or on-chain primitives—to allow Candy to **retain marketplace fees** and build new incentives around NFT trading within its own ecosystem.

---

**2\. Goals**

• Provide a **full-featured NFT marketplace experience** for Candy partner collections.

• Bypass external marketplace aggregators and route listings through Candy infrastructure.

• Surface **Candy-specific NFT metadata** like customization history and trait-level analytics.

• Create **cross-links** to the Candy trait marketplace.

• Design a **rewardable fee system** that supports partner collection economies and gamification.

---

**3\. Success Criteria**

• NFTs from Candy partner collections are tradable end-to-end on the platform.

• Buyers can filter, browse, and sort NFTs by trait data.

• Listings and purchases work reliably via Candy’s own listing engine (off-chain or on-chain).

• Candy captures listing fees directly.

• Trait detail overlays and click-through to trait marketplace are functional.

• Candy NFTs show **visual change history** with clear version tracking.

• NFT detail modals include full price history, trait breakdowns, and rarity data.

---

**4\. Users & Personas**

**Collectors / Traders**

• Want a clean UI and trustworthy pricing signals.

• Seek advanced filtering and visual browsing features.

**Candy NFT Holders**

• Want to list NFTs for sale without relying on third-party sites.

• Expect integrated trait marketplace access.

**Partner Collection Creators**

• Want to drive secondary sales within the Candy platform.

• Benefit from increased fee capture, loyalty systems, and trading volume.

---

**5\. Features & Functionality**

**5.1 Collection Marketplace Pages**

• Show all NFTs from a specific partner collection.

• Filters:

• By trait (visual filters by trait image or name)

• By price range

• By custom properties (e.g., equipped with “X” trait)

• Sorting:

• Price (low-high, high-low)

• Newest listings

• Rarity / drip score

• Key stats: floor price, 24h volume, total supply, listings count

**5.2 NFT Detail Modal / Page**

• NFT preview image

• Trait breakdown:

• Visuals for each trait

• Hover: rarity %, total trait supply, \# currently equipped

• Click-through: opens trait marketplace modal

• Price chart and listing history

• Purchase \+ bidding interface

• **Customization history timeline** (Candy NFTs only):

• Shows all visual versions over time

• Pop-out gallery with image previews and dates

**5.3 Listing Engine**

• List NFT for sale via:

• Fixed price (initial MVP)

• \[Optional future: auction, offer, Dutch listing\]

• Backend holds listing state and locks NFT (or uses escrow/authority)

• Listings stored in Candy DB or on-chain if feasible

• Fees and royalties enforced on sell (e.g., 2–3% platform \+ creator split)

**5.4 Purchase Flow**

• Wallet connects via Phantom/Backpack

• Purchase signs and processes transaction

• Transfers NFT and fee

• Backend updates ownership, closes listing, logs history

**5.5 Trait-Aware Features**

• Clickable trait cards in detail view

• Hover-over trait rarity / supply

• Trait “listing” filter (e.g., show all NFTs with “Gold Jacket”)

• Trait evolution history (if customized NFTs)

• Link directly to Trait Marketplace

**5.6 Candy-Only Incentive Layer**

• Configurable listing fee (e.g., 3%)

• Optional rewards/staking system for traders

• Leaderboards by volume traded or drip score delta

• Incentive banners/prompts to use Candy over Magic Eden/Tensor

---

**6\. Optional / Future Features**

• Offers / bids system (on listed or unlisted NFTs)

• Trait-based collection charts and trading insights

• Candy-native escrow / auto-compound yield on NFTs in listing

• Points/reward system for volume traded

• API integration with creators for in-collection sales analytics

---

**7\. Technical Requirements**

**Backend**

• Listing creation engine

• Off-chain DB-based initially (e.g., listings table)

• On-chain auction house or marketplace smart contract (optional later)

• Trait indexer service (to power filters and hover stats)

• NFT customization history tracker (stored as image URIs \+ timestamp logs)

• Wallet signature validation for listing \+ buy

• Listing fee collector (Candy wallet or programmable routing)

**Frontend**

• Collection pages with filters \+ sorting

• NFT cards with trait overlays \+ preview hover

• Trait popups and click-through into secondary trait market

• NFT detail modal:

• Trait display

• Gallery timeline for visual history

• Price chart

• Buy / list / delist buttons

• Connection to wallet and action confirmations

---

**8\. Dependencies**

• Supabase (for listings, traits, users, history, fees)

• Helius RPC or Metaplex indexing (initial state sync)

• Custom trait overlay engine (likely already in use from other modules)

• Candy backend listing controller

• Phantom / WalletConnect integration

---

**9\. Open Questions**

| Topic | Notes |
| ----- | ----- |
| Aggregator bypass | Do we need a custom Solana marketplace contract, or can off-chain listings suffice? |
| Fee structure | What percent is ideal for Candy platform vs. creator? |
| Rarity signals | Should we calculate trait rarity at runtime or precompute? |
| Timeline rendering | Can we reuse the image generation system to render trait history at each point? |
| Trait updates | Should we store snapshots or reconstruct from trait assignments? |

---

**10\. Risks & Mitigations**

| Risk | Mitigation |
| ----- | ----- |
| Users default to Magic Eden listings | Incentivize Candy use with rewards, better UX, and trait-based edge |
| NFT transfers off-platform | Revalidate listings and ownership before purchase |
| Aggregator cannibalization | Use whitelisting and restricted indexing of partner-only listings |
| Trait data overload | Use caching and incremental sync on trait-based search and filtering |

---

