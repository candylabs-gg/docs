**Product Specification Document (PSD)**

**Feature:** Trait Secondary Marketplace

**Prepared For:** Executive, Product, Design, Development

**Version:** v1

**Last Updated:** March 29, 2025

---

**Summary**

The Trait Secondary Marketplace is a major feature of the Candy platform that allows users to trade individual NFT traits (like shirts, hats, accessories, backgrounds, or even consumable items). These traits are not standalone NFTs, but behave like individual, tradable assets within our ecosystem.

Users will be able to **list**, **buy**, and **sell** traits that are **not currently equipped to their NFTs**, allowing for collection, customization, and speculation. The marketplace will show trait listings by collection and globally, with full filtering, rarity stats, and price history.

This feature powers the dynamic nature of Candy NFTs and sets us apart from traditional static NFTs.

---

**Why It Matters**

• Enables a fluid, composable NFT experience: NFTs are not fixed—they evolve through community-driven customization.

• Creates a novel economic layer around traits that collection creators can use to build games, stories, and brand ecosystems.

• Drives ongoing user engagement and marketplace activity beyond mint.

• Helps collection creators earn post-mint revenue from secondary trait sales and interactions.

---

**Key Concepts**

• **Trait**: A shirt, background, accessory, or consumable item that can be attached to or detached from a user’s NFT.

• **Inventory**: Where a user’s unequipped traits live.

• **Equipped Trait**: A trait currently being used on a specific NFT and therefore not available for trade.

• **Trait Item**: A unique, individual instance of a trait (like a single “Red Hoodie \#003”).

• **Listing**: A trait item placed for sale in the marketplace by a user.

• **Marketplace**: The interface and system where trait listings are browsed, filtered, and bought/sold.

---

**Primary User Flows**

**1\. Listing a Trait for Sale**

• Users open their inventory.

• Select an unequipped trait item.

• Click “List for Sale.”

• Set price and confirm transaction.

• Trait is placed in escrow and becomes unavailable for customization.

**2\. Buying a Trait**

• Users browse traits by collection or globally via Candylabs’ marketplace/API.

• In the marketplace, users can optionally filter by price, type, rarity, etc.

• Users can view specific details about a trait by clicking on the listing.

• Once the user has selected a trait to be purchased in the marketplace, they can click on "Add to Cart” 

When the user is ready to finalize their purchase, they can Check Out and pay with either $SOL or natively supported token.

• Upon finalization of purchase, the trait is added to the user's inventory.

**3\. Equipping a Trait**

• Users navigate to their inventory to view and select an owned NFT.

• Once an NFT is selected, a customization menu appears that lists all modifiable traits in the selected NFT.

• When the user wants to swap a trait from their inventory into their selected NFT, they can select the desired trait to be swapped and it will be applied directly into the selected NFT.

• When the user is finished with their customizations, the "Finalize" button is pressed to apply all changes to the selected NFT. Any swapped traits will be returned to the user's inventory.

**4\. Redeeming or Receiving Traits**

• Users receive airdropped traits, rewards, or redemptions (QR codes, links, etc.).

• Trait appears in inventory, available for customization or listing.

---

**Feature Summary**

| Feature | Description |
| ----- | ----- |
| Trait Listings | Users can list unequipped traits for a fixed price. (Auction feature TBD for Phase 1\) |
| Purchase Flow | Buyers can purchase traits and receive them in their inventory |
| Trait Browsing | Trait marketplace by collection and globally |
| Filtering & Sorting | Filter by trait type, rarity, origin, equipped history, price |
| Trait Details View | Shows rarity stats, ownership history, price charts |
| Trait History | Tracks where traits came from, who owned them, which NFTs wore them |
| Dynamic Rarity | Updates rarity data as traits are equipped, unequipped, sold, or burned |
| Inventory System | Trait management UI with equipped/unequipped state |
| Creator Tools | Collection owners can create traits, stock shops, issue airdrops, and reward users |
| Escrow Listing System | Traits in the NFT are locked in when listed to prevent issues with customization  |

---

**Sample UI (To Be Designed)**

• Trait card components for listing view

• Trait detail modal/page with:

• Image

• Collection \+ Type

• Price history graph

• Equip history (NFTs that used it)

• Buttons: Buy Now / Make Offer / Report

• Inventory panel with tabbed filtering and “List for Sale” button

• Filter sidebar on marketplace page

• Creator dashboard trait management (Add stock, View sales, Airdrop tools)

---

**Business Rules**

• Traits must be **unequipped/swapped** to be listed (with rare exceptions for future direct trades).

• Listings are removed from escrow immediately upon purchase.

• Trait ownership and usage history are permanently logged in DB.

• Users cannot delete sold traits; only creators can remove unsold stock.

• Trait rarity is calculated dynamically based on how many are in the ecosystem and currently equipped.

---

**Integration Notes**

• **Solana**: Wallet authentication, marketplace transactions

• **Supabase**: Trait ownership, listing management, rarity calculation

• **DigitalOcean / Pinata**: Image hosting, metadata handling

• **Frontend**: Built with Next.js and Tailwind CSS, ShadCN

• **Creator Dashboard**: Will use internal APIs to manage traits, airdrops, and drops

• **Auth & Wallet Connect**: Needed for any buying, selling, listing, or equipping

---

**Roadmap Position**

This is a **core MVP feature** and launch blocker. The trait marketplace is foundational to the user experience and will significantly differentiate Candy from traditional NFT marketplaces.

Target release for MVP: **6.25.2025**

---

**Product Specification Document (PSD)**

**Feature:** Candy Mint Launchpad (User-Facing)

**Version:** v1

**Prepared For:** Executive, Product, Design, Development

**Last Updated:** April 4, 2025

---

**Summary**

The **Candy Mint Launchpad** enables users to mint fully customized NFTs on the Solana blockchain directly from our platform. Unlike traditional NFT mints, each Candy-powered NFT is **built in real-time by the user**, with a **custom trait selection interface**. The platform then dynamically generates the final NFT artwork and metadata, and mints the NFT into a verified Solana collection tied to the user’s wallet and Candy account.

This creates a more interactive and personalized minting experience, helping new collections stand out while increasing user engagement and long-term value of minted NFTs.

---

**Key Features**

**1\. Launchpad Interface**

• Live or upcoming Candy-powered collections are displayed on the home page.

Users can select the promoted collection by clicking on the “Mint Now” button.

• Collection artwork, name, description, mint price, supply, and countdown is listed in the Launchpad page for the specified collection.

• Access gates (e.g., whitelist, wallet-based gating). If you can’t mint, the button will be greyed out with an optional “open for public” countdown attached.

• “Mint Now” button opens the customization flow under the specified collection. 

**2\. Trait Customization**

• Trait categories: shirt, hat, background, etc.

• Each purchasable trait in the specified collection is visually displayed with quantity remaining (if limited).

• Users select from available traits to build their NFT.

• Some traits may cost more “points” than others (optional gamified system).

• Users can hit “Randomize” to get an auto-generated combination.

**3\. Live Preview \+ Mint**

• Users see their completed NFT in a live preview window.

• When confirmed:

• Image is generated from trait layers.

• Metadata is built (collection info, selected traits, etc.).

• Image and metadata are uploaded to IPFS.

• NFT is minted into the collection via Solana.

**4\. Post-Mint Sync**

• NFT is sent to the user’s wallet.

• NFT is linked to their Candy user account on the backend.

• Success modal and wallet confirmation.

• Traits selected are tracked for rarity analytics and drip scoring.

---

**Optional Features (Phase 2+)**

• Point-based customization system for limiting trait access at mint.

• Trait rarity revealed post-mint (“box-opening” style).

• Leftover points converted to Candy trait credits.

• Airdrops or bonuses tied to selected traits.

---

**Why It Matters**

• **Personalized NFTs** increase emotional and financial investment from users.

• **Collection creators can offer more curated, creative drops**, with traits managed like inventory.

• **Marketplaces like Magic Eden or Tensor** will instantly recognize these NFTs due to standard metadata formats.

• **Gamified systems (points, rarity mechanics)** build long-term loyalty.

• **Candy account linkage** ensures ownership and metadata stay synced for customization or resale.

---

**Basic Flow**

1\. User connects wallet and views live collection.

2\. Enters customization interface and selects traits.

3\. Preview is updated in real-time.

4\. User confirms → metadata \+ image are generated and uploaded.

5\. NFT is minted using their wallet.

6\. Success screen appears; NFT is visible in wallet and Candy account.

---

**Dependencies**

• Solana wallet (e.g. Phantom)

• IPFS storage (Pinata or DigitalOcean Spaces)

• Backend metadata/image generator

• Solana minting handler (via Metaboss or custom)

• Candy backend (Supabase) for trait tracking and user/NFT relationship

---

**Team Notes**

• PM and Design should finalize the customization UX with trait limits and real-time preview.

• Backend must support image stacking, metadata construction, and upload before initiating mint.

• Frontend must handle mint status states (loading, success, fail).

• QA testing should include randomized mints, bulk mints, and trait stock edge cases.

---

**Product Specification Document (PSD)**

**Feature:** Candy Creator Launcher Tool

**Version:** v1

**Prepared For:** Executive, Product, Design, Development

**Last Updated:** March 29, 2025

---

**Summary**

The **Candy Creator Launcher Tool** is an internal-use configuration wizard designed to let partner creators (e.g., Nubcats, Skellies) fully set up their Candy-powered NFT collection. The tool walks creators through a step-by-step process for defining their collection’s core metadata, trait system, rarity logic, customization settings, and mint behavior.

Unlike traditional NFT launchers, Candy’s launcher does not require creators to pre-generate metadata or images. Instead, the system is designed for **on-mint dynamic generation**, meaning each NFT’s final artwork and metadata is created by the user—either through randomization or customization—at the moment of minting.

This tool will later evolve into a **self-serve public launcher** for any creator to deploy Candy-native NFT projects.

---

**Key Features**

**1\. Collection Metadata Configuration**

• Collection name, symbol, description

• Banner and thumbnail images

• Naming format (e.g., “Nub \#47” or “Bob (Nub \#47)”)

• Toggle: Allow naming at mint?

• Toggle: Allow renaming post-mint?

**2\. Mint Settings**

• Price and currency (e.g. SOL)

• Total supply

• Launch start time

• Whitelist import or conditional allowlist logic (e.g. token holders)

• Option: Randomized mint or customization-based mint

**3\. Trait Asset Upload**

• Creator drops a folder of trait assets organized like:

```
traits/
  head/
  shirt/
  face/
  background/
```

• The tool detects folder names as trait types

• Each file becomes a unique trait in the collection

**4\. Trait Configuration Panel**

• For each trait:

• Set rarity (for randomized collections)

• Set stock \+ point cost (for customizable collections)

• Set SOL price (optional)

• Mark traits as unlimited or limited supply

• Trait incompatibility UI (search and block combinations)

• Presets for rarity values (e.g., “Rare”, “Common”, etc.)

**5\. Preview Tools**

• Customize preview mode: build test NFTs visually

• Randomizer preview: simulate random mints

• Layering validation

• Compatibility tester

**6\. Finalization Step**

• “Confirm & Finalize” locks the configuration and:

• Uploads all trait assets to cloud storage

• Populates Candy’s internal database with trait structure

• Prepares metadata system for minting

• Marks the collection as “ready for launch”

**7\. Solana Deployment**

• Collection is deployed to Solana using internal tooling

• Sets update authority to Candy or uses abstracted signature (via Turnkey)

• Minted NFTs are tradable on Magic Eden, Tensor, etc. automatically.

**8\. Creator Wallet Binding**

• Creator connects wallet during setup

• Candy links wallet to the collection in backend

• Grants dashboard access post-mint for management, airdrops, and analytics

---

**Why It Matters**

• Gives creators **complete control** over collection structure without coding

• Simplifies trait management, rarity logic, and mint setup

• Enables **customized NFTs** as a core part of the mint experience

• Sets the stage for a **self-serve launch platform**

• Ensures smooth handoff to the Creator Dashboard for long-term trait economy management

---

**Flow Summary**

1\. Creator sets basic collection details.

2\. Uploads organized trait assets.

3\. Configures trait behavior and mint system.

4\. Tests combinations in preview mode.

5\. Confirms and finalizes.

6\. Tool handles upload, DB setup, and collection deployment.

7\. Collection becomes visible on Candy’s launchpad and ready for mint.

---

**Dependencies**

• DigitalOcean Spaces for image upload and delivery

• Supabase for trait structure and creator ownership tracking

• Metaboss or internal CLI for Solana deployment

• Turnkey for wallet connection \+ permission abstraction

• Candy backend workers for trait ingestion and DB linking

---

---

**Product Specification Document (PSD)**

**Feature:** Candy NFT Marketplace (Full NFT Trading)

**Version:** v1

**Prepared For:** Product, PM, Design, Development, Executive

**Last Updated:** March 29, 2025

---

**Summary**

The **Candy NFT Marketplace** is a curated trading interface focused entirely on NFTs from **Candy partner collections**, such as Nubcats, Retardio Cousins, and Skellies. It allows users to **buy, list, and explore NFTs** with advanced trait-aware tools, enhanced visual UX, and tight integration with Candy’s trait economy and metadata systems.

Unlike generic marketplaces (e.g., Magic Eden, Tensor), the Candy Marketplace is built **specifically for dynamic, composable NFTs**—offering visual previews of traits, customization history, and cross-links to individual trait trading.

It also gives Candy control over **listing and trading fees**, allowing for monetization, rewards systems, and creator-specific incentives.

---

**Key Features**

**1\. Collection Marketplace Pages**

• Grid display of NFTs within a Candy partner collection

• Trait filters (e.g., search by equipped trait or type)

• Sorting options: floor price, newest, rarity

• Live collection stats: volume, supply, listings, floor

**2\. NFT Detail View**

• NFT preview image

• Trait breakdown (with visuals \+ hoverable stats)

• Click-through to the trait marketplace for individual items

• Buy button with wallet connection

• Price chart and sale history

**3\. Customization History (Candy NFTs only)**

• Shows every version of the NFT over time

• Visual timeline of trait combinations and date of change

• Full-screen gallery view available

**4\. Listing & Purchase**

• Users can list NFTs directly on Candy

• Listings tracked in Candy’s backend (or on-chain in future)

• Buyers can purchase with SOL via wallet signature

• Candy captures fees at point of sale (e.g., 2–3%)

**5\. Trait-Aware Functionality**

• Clickable traits that lead to the trait marketplace

• Hover-over reveals trait rarity %, supply, and equip count

• Advanced filters on collection page: “Show NFTs wearing \[X\]”

**6\. Fee and Rewards System (Optional/Future: point system)**

• Candy charges marketplace fee (e.g., 3%)

• Portion can be redistributed via drip score, staking, or rewards

• Incentivizes trading on Candy instead of third-party platforms

---

**Why It Matters**

• Captures volume and revenue currently going to third-party aggregators

• Deepens Candy’s product loop between **full NFTs** and **traits**

• Gives creators a way to track and grow secondary market activity **within Candy’s ecosystem**

• Offers a **superior user experience** tailored for visual NFT customization

• Unlocks future opportunities for trading rewards, loyalty systems, and gamification

---

**Flow Summary**

1\. User visits a collection page (e.g., Nubcats Marketplace).

2\. Browses NFTs with advanced filters and live stats.

3\. Clicks into an NFT to see visual traits, rarity, and history.

4\. Can purchase immediately via wallet or click through to a trait’s page.

5\. Listing fees are captured and routed to Candy and creators.

---

**Key Differentiators**

• Focused on **Candy collections only** for high signal-to-noise

• Deep integration with the **trait layer system**

• Timeline/history of **visual customization**

• **Controlled listings and fee capture** (no external aggregators)

• Tailored UX to each collection’s aesthetic and community

---

**Dependencies**

• Supabase (for listings, user ownership, traits, and history)

• Solana wallet connection (Phantom/Backpack)

• Helius RPC or on-chain indexer

• Trait image overlay system (already used in customization)

• Candy listing engine (initially off-chain, future on-chain optional)
