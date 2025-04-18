Core Principles

* Fast time-to-market (lean, modern frameworks, batteries-included tools)

* Horizontal scale, real-time UX, safety on holds/escrow

* Web3-native (Solana/Metaplex), smooth wallet flows, IPFS integration

* Future proof: supports modular web2/web3 patterns (traits "off-chain", NFTs on-chain, connects to APIs/blockchain)

* Cloud-native, portable (avoid lock-in, infra as code, clear migration to on-chain where needed)

---

## 1\. Frontend

* Framework: [Next.js 15](https://nextjs.org/) (hybrid SSR/SSG, best-in-class DX, React 19, page/server components for performance, incremental static)

* UI Kit: [Tailwind CSS](https://tailwindcss.com/), [Shadcn/ui](https://ui.shadcn.com/) or Radix UI for accessibility

* State Management: Zustand or React Context/Query

* Wallet Connect: [Solana WALLET ADAPTER](https://github.com/solana-labs/wallet-adapter), supports Phantom, Backpack, Solflare, Ledger

* Realtime: [Supabase Realtime](https://supabase.com/docs/guides/realtime) (for live inventory, listings, transactions) or [Pusher Channels](https://pusher.com/channels) if cross-DB needed

* Image Handling: [Next.js Image](https://nextjs.org/docs/app/api-reference/components/image), [imgix](https://www.imgix.com/) (CDN optimizations)

## 2\. Backend / API Layer

Overall: Light but resilient, scale to high concurrency, focus on core trait logic.

* Primary API: [Node.js](https://nodejs.org/) (v20+) \+ [tRPC](https://trpc.io/) or NestJS (if you prefer opinionated architecture)  
  * tRPC for type-safe, end-to-end workflows (frontend+backend)  
  * Express (microservices/tasks where needed, e.g. image pipeline)  
  * Next.js server routes for fast prototyping/internal APIs

* Worker/Queue System: [BullMQ](https://docs.bullmq.io/), [Bee-Queue](https://github.com/bee-queue/bee-queue) for jobs (image generation, IPFS uploads, reconcile, trait airdrops)

* Cloud Functions: [Supabase Edge Functions](https://supabase.com/docs/guides/functions), \[Vercel Serverless\], or \[AWS Lambda\] for event-driven or image/metadata generation

* Solana Integration: [Helius SDK](https://www.helius.dev/docs/), [Solana Web3.js](https://solana-labs.github.io/solana-web3.js/), \[Metaplex JS\] (for minting/metadata)

* Auth: Supabase Auth (password/login, email magic links, can federate wallets), or custom JWT with session service

* Wallet Sessions: Solana wallet signature for login \+ secondary session token for DB operations

## 3\. Database

* Primary: [Supabase/PostgreSQL](https://supabase.com/) (fully managed, triggers, policies, audit logs, row-level security)  
  * Realtime: Supabase Realtime engine/webhooks to frontend  
  * Schema-first: trait\_items, nft\_traits, trait\_types, users, listings, transactions, market\_events etc.  
  * Materialized views or CRON for analytics (rarity, floor price, drip scores)

## 4\. Storage/Media

* Trait Images / NFT composites: [DigitalOcean Spaces](https://www.digitalocean.com/products/spaces) (S3-compatible) with CDN enabled

* NFT Media/Metadata: [Pinata](https://www.pinata.cloud/) (IPFS gateway), fallback to Spaces for web2 fallback \+ faster image loading

* On-the-fly Image Generation: Use [Sharp](https://sharp.pixelplumbing.com/) in backend worker or [Cloudflare Images](https://developers.cloudflare.com/images/) for previews/composite

## 5\. Blockchain/Web3

* Core chain: Solana (latest)

* Minting / Metadata: [Metaplex Candy Machine v3](https://docs.metaplex.com/programs/candy-machine/) or custom mint handler if trait customization on mint is complex; use [Metaboss](https://metaboss.rs/) for CLI admin ops

* RPC: [Helius](https://www.helius.dev/) (fast, dev-friendly, reliable, NFT/webhook support)

* Marketplace escrow (future): Custom PDA contracts or use Metaplex’s Auction House module, or off-chain with admin-held escrow wallets (v1 off-chain, v2 can move on-chain as needed)

## 6\. Admin/Creator Tools

* Internal UI: Extend Next.js app or [Retool](https://retool.com/) for MVP admin workflows if speed critical

* Trait Uploader/Parser: Node backend or Edge Function parses folders, writes to DB, pushes images to DO Spaces

* Image alignment & preview: [Fabric.js](http://fabricjs.com/) or [React Konva](https://konvajs.org/docs/react/) for drag/edit layers in admin UI

## 7\. Analytics/Monitoring

* User actions: Supabase event DB, [PostHog](https://posthog.com/) or [Amplitude](https://amplitude.com/)

* Error monitoring: [Sentry](https://sentry.io/) (frontend & backend)

* Security: 2FA via Supabase or email magic links, RLS on DB, wallet sig verification on sensitive ops

## 8\. CI/CD/Infra

* Deployment: [Vercel](https://vercel.com/) (best for Next.js), \[AWS ECS/fargate\] or \[Fly.io\] for Node.js backend if split

* Infra as Code: [Terraform](https://www.terraform.io/) or Pulumi for managing infrastructure and environments

* Testing: [Jest](https://jestjs.io/), [Playwright](https://playwright.dev/) (integration), [Vitest](https://vitest.dev/) for tRPC

---

## Stack Summary Table

| Layer | Tech/Service | Why |
| :---- | :---- | :---- |
| Frontend | Next.js 15, Tailwind, Radix UI | Modern, scalable, best SSR, flexible UI, rapid dev |
| State Mgmt | Zustand, React Query | Fast, predictable, type-safe |
| Wallet | Solana Wallet Adapter | Phantom, Backpack, Solflare, Ledger support, reliable |
| Backend API | Node.js, tRPC, Express | Fast, type-safe, merges nicely with Next.js if needed |
| DB | Supabase (Postgres) | Managed, secure, RLS, realtime, easy to scale |
| Blockchain | Solana, Metaplex SDK, Helius, Web3 | Fast mints, good NFT infra, scaleable, dev support |
| Storage | DigitalOcean Spaces, Pinata (IPFS) | Fast, S3-API, cost-effective, IPFS fallback for web3 needs |
| Workers | BullMQ, Supabase Functions, Lambda | Async image gen, airdrops, sync |
| Admin UIs | Next.js internal or Retool | Fast for MVP, extensible |
| Monitoring | Sentry, Amplitude/PostHog | Errors, usage, iterates UX |
| DevOps | Vercel, Terraform/Pulumi, GitHub | Modern CD/CI, preview deployments, auditable infra |

---

## Why this stack?

* Fits composability: You can compose traits, NFTs, and trading logic without monolithic on-chain contracts.

* Scale on demand: Supabase+Next.js CDNs \+ Node queue workers lets you scale during high mints/trading or trait airdrops.

* Enables Realtime UX: Traits/listings update in user UIs without reloads.

* Web3 future-proof: Ready for more on-chain logic (marketplace upgrades, trait on-chain NFTs) with low friction.

* Developer velocity: Modern JS/TS all the way, massive hiring pool, rapid onboarding, great production tooling.

---

## Alternatives / Non-Core Notes

* If you want a more serverless/infra-modern approach: consider [Cloudflare D1](https://developers.cloudflare.com/d1/) for DB (early but growing), or [PlanetScale](https://planetscale.com/) (if moving away from Postgres).

* For highly on-chain marketplace v2: Consider moving trait marketplace to Solana Smart Contracts using Anchor, but this fits a phase 2/v3 for most teams.

# 2025 Tech Stack for Candy Ecosystem

(Trait Secondary Marketplace, Mint Launchpad, Creator Launcher Tool, Candy NFT Marketplace)

## 1\. Modern Frontend

| Tool | Purpose | Notes/PSD Coverage |
| :---- | :---- | :---- |
| Next.js 15 | React app, SSR, SSG, API routes, multi-app ready | Used for all user/dashboard/creator UIs. Next API-handlers can stitch image/metadata if needed. |
| Tailwind CSS \+ shadcn-ui or Radix UI | Design system | Rapid, accessible, customizable UI matrices. Adaptable per feature/module. |
| React State: Zustand/React Query, Context | Composable state | Trait-inventory, listing, and live preview performant—integrates with Supabase hooks. |

PSD Matching:

* Inventory panels, trait details, instant filters, timeline UIs.

* "Live preview" (used in Mint & Creator flows) with React-canvas component (see below).

---

## 2\. Realtime Backend API

| Tool | Purpose | Notes/PSD Coverage |
| :---- | :---- | :---- |
| Node.js 20+ | Core API logic, scalable, modern async/TS support | tRPC/NestJS both fine (tRPC for easier end-to-end typing, faster MVP) |
| tRPC/NestJS/Express | API layer | API for trait ops, listing, marketplace, preview, auth etc. |
| Supabase Edge Functions | Fast serverless/cron (mint image gen, airdrop, escrow) | Use for async tasks: image stacking, rarity batch processing, reconciliation, trait airdrops |
| Supabase Realtime (WS subscriptions) | Push trait/inventory/market changes. | Needed to show live trait market, rare trait “just listed” notifications, etc. |

PSD Matching:

* Escrow/lock logic is in API (trait state, listing state, equip/de-equip).

* Server-side trait rarity/analytics (on trait equip/unequip/list/sale).

* Creator endpoints for upload, validate, finalize.

---

## 3\. Database & Storage

| Tool | Purpose | Notes/PSD Coverage |
| :---- | :---- | :---- |
| Supabase (PostgreSQL) | Relational DB for all core data | All trait, user, NFT, listing info. Leverage RLS for security (escrow/ownership, admin, creator roles). |
| Materialized/Postgres Views & CRON | Realtime and batch analytics | Powers rarity calculations, trait usage, leaderboards, supply metrics. |
| DigitalOcean Spaces (S3 compatible, CDN) | Trait image & NFT layer storage | For user/creator trait uploads in the launcher, generated mints, preview caching, etc. |
| Pinata (IPFS) | Public NFT image/metadata hosting | For mints; fallback to Spaces for app runtime performance. |

PSD Matching:

* "Trait item," inventory state, trait history (ownership chain), NFT-to-trait mapping.

* Trait folder upload → asset → table entries in Creator Launcher.

* Batch/cron for analytics-driven features: drip score, rarity stats, etc.

---

## 4\. Blockchain/Solana Layer

| Tool | Purpose | PSD Coverage |
| :---- | :---- | :---- |
| Helius RPC | Fast, Webhook-enabled Solana access | Mint, listing transfer, airdrop monitoring, instant traits. |
| Solana/web3.js \+ Metaplex | Mint, manage, verify NFTs and collections | Mint Launchpad, NFT Marketplace lists/sells. |
| Metaboss | CLI/worker for quick admin/creator ops | Initial collection deployment, update authority, etc. |
| Custom Escrow (future) | PDA smart contract for on-chain escrows | (For v2: handle trait trades fully on-chain.) |

PSD Matching:

* Trait/marketplace flows authenticate via wallet.

* Minting, airdrops, listings interact on-chain and are reflected in backend DB.

---

## 5\. Wallet/Auth/Session

| Tool/Lib | Purpose | PSD Coverage |
| :---- | :---- | :---- |
| Solana Wallet Adapter | Multi-wallet support (Phantom, Backpack, Ledger, etc.) | For all auth—buy, sell, mint, equip—across flows. |
| Supabase Auth/JWT | Non-wallet auth (email, dashboard) | Internal staff, admin, creator login. |

---

## 6\. Workers/Queues

| Tool | Purpose | PSD Coverage |
| :---- | :---- | :---- |
| BullMQ (Redis) or Supabase Functions | Offload async/photo gen, send airdrops, manage trait stock | Mint image stacking, trait airdrop flows, delayed actions, etc. |
| Node "Sharp" or "canvas" | Image compositing for NFT artwork | Preview and "finalize" in both Launcher Tool and Mint Launchpad. |

---

## 7\. Internal Admin/Creator UI

* Extend your Next.js app with protected admin/creator routes, custom dashboard UI.

* Use generic table/lib (e.g. React Table, Shadcn/ui DataTable) for trait/stock/sales.

* Optionally use Retool for rapid MVP if high urgency.

---

## 8\. Monitoring & Analytics

| Tool | Purpose | PSD Coverage |
| :---- | :---- | :---- |
| Sentry | Frontend \+ backend error logs | Ensure mint/trade flows don’t break. |
| PostHog | Product analytics | Track trait, mint, trade engagement KPIs. |
| DB logs | For all state/value changes | Legal/accounting and community trust. |

---

# Implementation Tips Per PSD

### Trait Secondary Marketplace

* Trait/Inventory Tables:  
  * trait\_items (unique trait instance, status: equipped/listed/owned),  
  * listings (trait\_id, price, seller, escrow status, timestamps)

* Escrow Model:  
  * When listing, trait’s state set to "escrowed", removed from user’s available inventory, prevent equips

* History/Audit:  
  * transaction\_log table (who-owned, when, price, which NFT equipped)

* Live Filtering:  
  * Supabase full-text with index on trait type, rarity, origin; combine with server-side price/range filter

### Mint Launchpad

* Live Preview:  
  * React canvas component using trait assets (layered PNGs from Spaces), updated per user selection

* Image/Metadata Generation:  
  * On commit, serverless function generates final image, sends to Pinata/Spaces, returns metadata hash/URI

* Mint Handler:  
  * Node.js lambdas or Express route kicks off mint instruction (web3.js), links NFT to Candy account

* Trait Point System:  
  * Expose per-mint settings from DB; validate stock/purchase cap server-side via API before mint trigger

### Creator Launcher Tool

* Folder Upload Parser:  
  * Node.js backend reads folder, creates “trait\_type”, “trait\_item” records, uploads images to Spaces

* Trait Compatibility:  
  * Many-to-many relation: "incompatible\_traits" lookup table

* Preview:  
  * Use same React canvas as Mint for admin test previews; randomized trait generator for stress testing

* Finalization:  
  * Locks records, triggers Space upload, creates supabase batch rows per trait/stock

### Candy NFT Marketplace

* NFT Display:  
  * Query nfts \+ trait\_equip tables for overlays, use prebuilt overlay on backend for fast loading

* Customization Timeline:  
  * Store each equip/unequip as event; generate snapshot preview server-side or via client canvas for gallery

* Listings:  
  * listings table with NFT id, price, seller; escrow NFT in program or in admin wallet for v1 off-chain

* Buy Flow:  
  * Signature-based transfer with backend swap/settle logic; update DB, unlock NFT, distribute fee/royalty

---

## 1\. Example Next.js Folder/API Structure

Assume Next.js 15 App Router (src/app), API handlers, modern conventions, and good separation.  
code  
/src  
  /app  
    /marketplace  
      page.tsx              \# Marketplace home (global trait listing)  
      layout.tsx  
      /\[collection\]  
        page.tsx            \# Traits by collection  
        /\[traitId\]  
          page.tsx          \# Trait detail view  
    /inventory  
      page.tsx              \# User inventory: view, list, equip  
      /list/\[traitId\]  
        page.tsx            \# Listing creation modal/page  
    /creator  
      /dashboard  
        page.tsx            \# Trait owner tools  
        /traits  
          page.tsx  
        /airdrops  
          page.tsx  
    /api  
      /traits  
        route.ts            \# GET: all traits, POST: new trait (admin)  
      /traits/\[id\]  
        route.ts            \# GET/PUT/DELETE: trait by id, metadata, history  
      /listings  
        route.ts            \# GET: all, POST: create  
      /listings/\[id\]  
        route.ts            \# GET: detail, DELETE: delist  
      /inventory  
        route.ts            \# GET: my inventory, POST: equip/unequip/list  
      /analytics/rarity  
        route.ts            \# GET: rarity stats  
      /airdrop/redeem  
        route.ts            \# POST: redeem code/airdrop  
      /auth/wallet  
        route.ts            \# POST: wallet login, nonce signing

  /components  
    TraitCard.tsx  
    TraitDetailModal.tsx  
    InventoryPanel.tsx  
    ListingCreateModal.tsx  
    FilterSidebar.tsx  
    CreatorDashboard/index.tsx

  /hooks  
    useTraits.ts  
    useInventory.ts  
    useLiveListings.ts  
    useWalletAuth.ts

  /lib  
    supabaseClient.ts  
    solanaClient.ts  
    traitHelpers.ts

  /utils  
    filterTraits.ts  
    priceFormat.tsx

  /types  
    trait.ts  
    listing.ts  
    user.ts  
    inventory.ts

Notes:

* Most market actions are SSR or ISR-enabled for freshness.

* useLiveListings.ts can use Supabase subscriptions for real-time trait/trade updates.

* Clean split between app (pages/UI), api (backend logic), hooks (state/data logic).

---

## 2\. Sample ERD (Entity Relationship Diagram) for Traits/NFTs

plaintext  
![][image1]

Key Notes:

* trait\_items \= unique, tradable asset (think: "Red Hoodie \#003 owned by Alice")

* Inventory \= trait\_items where equipped\_to \= null AND listed\_bool \= false AND owner\_id \= user

* A trait can be owned, equipped, listed, airdropped, moved; all transitions logged in a transactions table.

* nft\_traits \= tracks which unique trait is currently equipped on which NFT

---

## 3\. Annotated Sample Epics/Jira Stories

### Trait Marketplace MVP (Phase 1 only)

Epic 1: Trait Listing Core

* Story: User can list unequipped trait from their inventory for fixed price  
  * BE: API to POST /api/listings (escrow trait, create listing, lock trait from equip)  
  * FE: Add “List for Sale” in InventoryPanel; modal with price input and confirmation  
  * BE: Validate trait is unequipped, not already listed, and belongs to user

Epic 2: Browsing & Filtering

* Story: User can browse traits globally and by collection  
  * BE: GET /api/listings?collection=x\&type=y  
  * FE: Implement Marketplace page with TraitCard grid, filters (collection, type, rarity, price)

* Story: Filter and sort trait listings by type, price, rarity  
  * FE: FilterSidebar with select/multiselect/slider components  
  * BE: Query params for efficient DB filtering/indexing

Epic 3: Trait Purchase Flow

* Story: User can purchase a listed trait  
  * BE: API to POST /api/listings/\[id\]/purchase (verify payment, transfer ownership, update listing status, move trait to buyer inventory)  
  * FE: Buy button on TraitDetailModal, show wallet connect \+ confirmation loading

* Story: Inventory updates after purchase  
  * FE: Auto-refresh or WS subscription to update InventoryPanel with new trait

Epic 4: Trait Detail & History

* Story: User can view details and usage history for any trait listing  
  * BE: GET /api/traits/\[id\] with current, price history, previous NFTs, all owners  
  * FE: TraitDetailModal with image, history, rarity stats

Epic 5: Realtime/UX

* Story: Live update trait market as new listings/purchases occur  
  * BE: Supabase Realtime (or listen for new/changed rows on listings)  
  * FE: useLiveListings hook to auto-refresh grid/cards

Epic 6: Admin Tools

* Story: Creators/admins can add new trait stock, airdrop  
  * BE: API for /api/traits (POST add, PATCH/PUT modify properties)  
  * FE: CreatorDashboard page for adding trait, managing stock, viewing activity

---

## 4\. Component Hierarchy & Key React Hooks

**Hierarchy for Marketplace Page**  
code  
MarketplacePage  
  ├── FilterSidebar  
  ├── TraitGrid  
      ├── TraitCard (xN)  
          └── \[onClick\] \=\> TraitDetailModal  
  ├── InventoryPanel (drawer/modal)  
      ├── ListForSaleModal  
  └── CreatorDashboard (if admin/creator)

**Key React Hooks Sketches**  
typescript  
// hooks/useLiveListings.ts  
import { useEffect, useState } from "react";  
import { supabase } from "@/lib/supabaseClient";

export function useLiveListings(collectionId?: string) {  
  const \[listings, setListings\] \= useState(\[\]);

  useEffect(() \=\> {  
    let query \= supabase  
      .from('listings')  
      .select('\*')  
      .eq('status', 'listed'); // filter only active listings

    if (collectionId) query \= query.eq('collection', collectionId);

    const fetchListings \= async () \=\> {  
      const { data } \= await query;  
      setListings(data || \[\]);  
    };

    fetchListings();

    const subscription \= supabase  
      .channel('public:listings')  
      .on(  
        'postgres\_changes',  
        { event: 'INSERT', schema: 'public', table: 'listings' },  
        (payload) \=\> {  
          setListings((curr) \=\> \[payload.new, ...curr\]);  
        }  
      )  
      .on(  
        'postgres\_changes',  
        { event: 'UPDATE', schema: 'public', table: 'listings' },  
        (payload) \=\> {  
          setListings((curr) \=\>  
            curr.map((l) \=\> (l.id \=== payload.new.id ? payload.new : l))  
          );  
        }  
      )  
      .subscribe();

    return () \=\> subscription.unsubscribe();  
  }, \[collectionId\]);

  return listings;  
}

typescript  
// hooks/useInventory.ts  
import { useEffect, useState } from "react";  
import { supabase } from "@/lib/supabaseClient";  
import { useWallet } from "@solana/wallet-adapter-react";

export function useInventory() {  
  const { publicKey } \= useWallet();  
  const \[inventory, setInventory\] \= useState(\[\]);

  useEffect(() \=\> {  
    if (\!publicKey) return;  
    const fetchInventory \= async () \=\> {  
      const { data } \= await supabase  
        .from('trait\_items')  
        .select('\*')  
        .eq('owner\_id', publicKey.toBase58())  
        .eq('listed\_bool', false)  
        .is('equipped\_to', null);  
      setInventory(data || \[\]);  
    };  
    fetchInventory();

    // TODO: Subscribe for real-time inventory changes

  }, \[publicKey\]);

  return inventory;  
}

---

## Use These As Starting Templates

* Folder structure is scalable for multi-products, multi-roles (users/creators/admin).


* Component/hooks approach lets you have truly real-time, personalized web apps—one of Candy’s differentiators\!  
  

[image1]: <data:image/png;base64,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>