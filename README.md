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
    
setTokenIdsForClass nonpayable (uint8,uint256[])

    Alternate way to add token ids to a class

    Parameters
    uint8 _class
    uint256[] _tokenIds
    
resetClass nonpayable (uint256)

    Remove all token ids for a given class, causing it to fall back to creating/minting into the nft address

    Parameters
    uint256 _classId
    
setTokenIdsForClasses nonpayable (uint256[6])

        Set token IDs for each rarity class. Bulk version of `setTokenIdForClass`

        Parameters
        uint256[6] _tokenIds: List of token IDs to set for each class, specified above in order
    
setOptionSettings nonpayable (uint8,uint256,uint16[6],uint16[6])
        Set the settings for a particular lootbox option

        Parameters
        uint8 _option: The Option to set settings for
        uint256 _maxQuantityPerOpen: Maximum number of items to mint per open. Set to 0 to disable this option.
        uint16[6] _classProbabilities: Array of probabilities (basis points, so integers out of 10,000) of receiving each class (the index in the array). Should add up to 10k and be descending in value.
        uint16[6] _guarantees: Array of the number of guaranteed items received for each class (the index in the array).
    

setSeed nonpayable (uint256)
        Improve pseudorandom number generator by letting the owner set the seed manually, making attacks more difficult

        Parameters
        uint256 _newSeed: The new seed to use for the next transaction
        
## MAIN FUNCTIONS

open nonpayable (uint256,address,uint256)
        Open a lootbox manually and send what's inside to _toAddress Convenience method for contract owner.

        Parameters
        uint256 _optionId
        address _toAddress
        uint256 _amount
        
        
_mint nonpayable (uint256,address,uint256,bytes)
        minting logic for lootboxes which is called via safeTransferFrom when MyLootBox extends MyFactory.
        NOTE: prices and fees are determined by the sell order on OpenSea.
        Parameters
        uint256 _optionId
        address _toAddress
        uint256 _amount
        bytes _data
        
withdraw nonpayable ()


## METADATA METHODS

uri view (uint256)
            Parameters
            uint256 _optionId
            Return Values
            string _0
            
name view ()
        
        Return Values
        string _0
        
        
symbol view ()
       
        Return Values
        string _0
        
## HELPER FUNCTIONS


        














