# Opensea Lootbox contract review
## Source code: https://github.com/ProjectOpenSea/opensea-erc1155/blob/master/contracts/MyLootBox.sol
## Reviewed by: Osazee Oghagbon


# Review

The Mylootbox contract creates a lootbox component where each lootbox can be traded just like regular items — but they can also be "opened" to redeem the actual item.it would create these new items according to some pseudo-random logic. For example, a game might want to issue a pack of items randomly generating an item of a certain ability level, or a set of cards with some probability of getting a "mythic" or "legendary" card.
These items will simply implement the unpack method, which burns the LootBox and mints a new item.









