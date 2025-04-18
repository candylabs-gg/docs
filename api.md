# API Description

Symbols:
- 🟪 : not started (current tasks)
- 🟨 : in progress
- ✅ : completed (implementation + docs + testing)
- ❌ : deprecated


Conventions
- Each feature can be decoupled in its frontend or backend tasks
- A backend task can be an `api route`, `server action` or other
- `jest` for unit testing
- use Pascal case, fix inconsistent naming (mixed snake, kebabe and Pascal)
- add #feature to Purpose of each task

### Current file structure
- [ ] auth
    - [ ] createSignInData
    - [ ] createSignInDataEth
    - [ ] verifySIWE
    - [ ] verifySIWS
- [x] fetchCollectionData `GET`
- [x] fetchNftMetadata  `GET`
- [x] getCollections `GET`
- [ ] heliusHook
- [ ] inventoryTraits
- [x] marketplace
    - [collection]
        - [x] fetchCollectionStats `GET`
        - [x] fetchContract `GET`
        - [x] fetchNfts `GET`
- [x] nft
    - [x] getNFTId `GET`
- [x] ownedCollections `GET`
- [x] ownedNFTs `GET`
- [x] traits
    - [traitId]
        - [x] availability `GET`
- [ ] updateMetadata
- [ ] user
    - [ ] getInventory
    - [ ] getUserId

## General API Listing

### **Others**
| Status | Method | Route                | Purpose                                    |
| :----: | :----: | -------------------- | ------------------------------------------ |
|        |  GET   | /trait/:id/nfts      | Get NFTs that have specific trait          |
|        |  GET   | /users/:id/inventory | User’s unequipped traits                   |
|   🟪   |  POST  | /users/:id/inventory | Return unequipped trait inventory for user |

### **/traits**
| Status | Method | Route                  | Purpose                                               |
| :----: | ------ | ---------------------- | ----------------------------------------------------- |
|   🟪   | GET    | /market                | Return global listings with filtering                 |
|   🟪   | GET    | /market/:collection_id | Filter listings by collection                         |
|   🟪   | GET    | /:item_id              | Return detail for single trait (used in detail modal) |
|   🟪   | POST   | /:item_id/list         | List a trait for sale                                 |
|   🟪   | POST   | /:item_id/unlist       | Remove trait listing                                  |
|   🟪   | POST   | /:item_id/buy          | Complete purchase and transfer ownership              |
|   🟪   | POST   | /:item_id/list         | List a trait for sale                                 |
|        | POST   | /:item_id/equip        | Equip to NFT                                          |
|        | POST   | /:item_id/unequip      | Move to inventory                                     |
|        | GET    | /:item_id/history      | Trait ownership, usage log                            |


### **/collections**
| Status | Method | Endpoint    | Purpose                                       |
| :----: | ------ | ----------- | --------------------------------------------- |
|        | GET    | /:id/traits | Fetch available traits for mint customization |
|        | GET    | /:id/nfts   | Fetch marketplace NFTs for collection         |

### **/mint**
| Status | Method | Endpoint        | Purpose                                                   |
| :----: | ------ | --------------- | --------------------------------------------------------- |
|        | POST   |                 | Accept user selections, generate metadata/image, mint NFT |
|        | GET    | /:tx_id/status  | Poll mint status                                          |
|        | POST   | /reserve-traits | Optional: reserve trait stock pre-mint                    |
|        | POST   | /finalize       | Sync minted NFT with backend and user account             |

### **/creator**
| Status | Method | Endpoint                 | Purpose                                         |
| :----: | ------ | ------------------------ | ----------------------------------------------- |
|        | POST   | /launcher/parse-traits   | Accept and organize uploaded folder             |
|        | POST   | /launcher/save-config    | Save current trait and collection settings      |
|        | POST   | /launcher/finalize       | Trigger upload, DB write, and asset deployment  |
|        | GET    | /launcher/preview        | Return randomized/custom preview mock NFT       |
|        | POST   | /launcher/validate-combo | Check a combination of traits for compatibility |
|        | POST   | /launcher/deploy-solana  | Final Solana collection creation logic          |

### **/listing**
| Status | Method | Endpoint         | Purpose                               |
| :----: | ------ | ---------------- | ------------------------------------- |
|        | POST   | /create          | List NFT for sale                     |
|        | POST   | /buy             | Purchase listed NFT                   |
|        | GET    | /:id/price-chart | Return price chart data for given NFT |

### **/nft**
| Status | Method | Endpoint     | Purpose                                          |
| :----: | ------ | ------------ | ------------------------------------------------ |
|        | GET    | /:id         | Get NFT metadata, traits, listings, sale history |
|        | GET    | /:id/history | Return NFT customization history                 |


## Type Response

```json
{
  "success": true|false,              // Boolean status indicator
  "data": { ... },                    // Primary response data when success is true
  "error": {                          // Present only when success is false
    "code": "ERROR_CODE",             // Machine-readable error code
    "message": "Human readable error" // User-friendly message
  },
  "meta": {                           // Metadata about the request/response
    "requestId": "uuid-v4",           // Unique identifier for request (for logging/tracking)
    "timestamp": "ISO-8601",          // When the response was generated
    "pagination": {                   // Optional, present when response is paginated
      "page": 1,                      
      "limit": 20,
      "totalItems": 240,
      "totalPages": 12,
      "hasMore": true
    },
    "filters": { ... }                // Echo back any filters applied to the request
  }
}
```
