# API Description

| route                                            | method | status | doc | test | description                              |
| :----------------------------------------------- | ------ | ------ | --- | ---- | ---------------------------------------- |
| `/marketplace/[collection]/fetchCollectionStats` | `GET`  |        |     |      |                                          |
| `/traits/:item_id/buy`                           | `POST` |        |     |      | Complete purchase and transfer ownership |
Create GET /traits/market
Return global listings with filtering

Create GET /traits/market/:collection_id
Filter listings by collection

Create GET /traits/:item_id
Return detail for single trait (used in detail modal)

Create POST /traits/:item_id/list
List a trait for sale

Create POST /traits/:item_id/unlist
Remove trait listing

Create POST `/traits/:item_id/buy`
Complete purchase and transfer ownership