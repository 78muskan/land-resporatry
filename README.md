pragma solidity ^0.8.0;

contract LandRegistry {
    // Mapping of owner addresses to their respective properties (land IDs)
    mapping(address => uint256[]) public ownersProperties;
    
    // Struct representing a property's metadata
    struct Property {
        address owner;  // Owner of the land
        string location;  // Geographical coordinates or description
        bool isAvailable;  // Whether the land can be sold/traded
    }
    
    // Array to store all properties (land records)
    mapping(uint256 => Property) public properties;

    event NewProperty(address owner, uint256 propertyId);

    function registerLand(
        address _owner,
        string memory _location,
        bool isAvailable
    ) external {
        require(_owner != address(0), "Owner cannot be zero");

        // Generate a unique ID for the new land record
        uint256 propertyId = properties.length;
        
        // Store the new property in our mapping of all properties
        Property memory newProperty =
            Property({
                owner: _owner,
                location: _location,
                isAvailable: isAvailable
            });
            
        // Add this ID to the list of lands owned by this user (if any)
        ownersProperties[_owner].push(propertyId);
        
        // Store it in our mapping of all properties as well
        properties[propertyId] = newProperty;
        
        emit NewProperty(_owner, propertyId);
    }

    function getOwnerLands(address _address) external view returns (uint256[] memory) {
        return ownersProperties[_address];
    }
    
}
