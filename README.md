# Opensea Lootbox contract review
## Source code: https://github.com/ProjectOpenSea/opensea-erc1155/blob/master/contracts/MyLootBox.sol
## Reviewed by: Osazee Oghagbon


# Review
MyLootBox - a randomized and openable lootbox of MyCollectibles

The MylootBox contract creates a lootbox component for Mycollectibles where each lootbox can be traded just like regular items — but they can also be "opened" to redeem the actual item.it would create these new items according to some pseudo-random logic. For example, a game might want to issue a pack of items randomly generating an item of a certain ability level, or a set of cards with some probability of getting a "mythic" or "legendary" card.
These items will simply implement the unpack method, which burns the LootBox and mints a new item.


# Imports 
ReentrancyGuard.sol contract is used to ensures that a function should be called only once per transaction.
    
    import "openzeppelin-solidity/contracts/utils/ReentrancyGuard.sol";
   
   
Contract module which allows children to implement an emergency stop mechanism that can be triggered by an authorized account.

    import "openzeppelin-solidity/contracts/lifecycle/Pausable.sol";
    
OpenZeppelin ownable.sol provides Ownable for implementing ownership in your contracts. 
By default, the owner of an Ownable contract is the account that deployed it, which is usually exactly what you want.

Ownable also lets you:
transferOwnership from the owner account to a new one, and

renounceOwnership for the owner to relinquish this administrative privilege, a common pattern after an initial stage with centralized administration is over.

    import "openzeppelin-solidity/contracts/ownership/Ownable.sol";
    
    
## State Variables 

    struct OptionSettings {
        // Number of items to send per open.
        // Set to 0 to disable this Option.
        uint256 maxQuantityPerOpen;
        // Probability in basis points (out of 10,000) of receiving each class (descending)
        uint16[NUM_CLASSES] classProbabilities;
        // Whether to enable `guarantees` below
        bool hasGuaranteedClasses;
        // Number of items you're guaranteed to get, for each class
        uint16[NUM_CLASSES] guarantees;
      }
      mapping (uint256 => OptionSettings) public optionToSettings;
      mapping (uint256 => uint256[]) public classToTokenIds;
      mapping (uint256 => bool) public classIsPreminted;
      uint256 seed;
      uint256 constant INVERSE_BASIS_POINT = 10000;
      
      
      
## Events
    LootBoxOpened (uint256,address,uint256,uint256)
    Parameters:
    uint256 optionId
    address buyer
    uint256 boxesPurchased
    uint256 itemsMinted
    
        
    OwnershipTransferred (address,address)
    Parameters
    address previousOwner
    address newOwner


    Paused (address)
    Parameters
    address account


    PauserAdded (address)
    Parameters
    address account


    PauserRemoved (address)
    Parameters
    address account


    Unpaused (address)
    Parameters
    address account


    Warning (string,address)
    Parameters
    string message
    address account


## Constructor 
_proxyRegistryAddress is The address of the OpenSea/Wyvern proxy registry 

_nftAddress The address of the non-fungible/semi-fungible item contract that you want to mint/transfer with each open

   
    constructor nonpayable (address,address)
    Parameters
    address _proxyRegistryAddress
    address _nftAddress

## INITIALIZATION FUNCTIONS FOR OWNER

setClassForTokenId nonpayable (uint256,uint256)

    If the tokens for some class are pre-minted and owned by the contract owner, they can be used for a given class by setting them here

    Parameters
    uint256 _tokenId
    uint256 _classId
    
setTokenIdsForClasses nonpayable ( Class , uint256[6])

    Alternate way to add token ids to a class

    Parameters
    _class
    _tokenIds
    
resetClass nonpayable (uint256)

    Remove all token ids for a given class, causing it to fall back to creating/minting into the nft address

    Parameters
    uint256 _classId
    
    
    













