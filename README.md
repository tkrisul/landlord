// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract LandRegistry is ERC721, Ownable {
    struct LandParcel {
        string parcelId;
        string location;
        uint256 sizeInSquareMeters;
    }

    mapping(uint256 => LandParcel) public landParcels;
    uint256 public nextTokenId = 1;

    constructor() ERC721("TokenizedLand", "LAND") {}

    function mintLand(
        address to,
        string memory parcelId,
        string memory location,
        uint256 sizeInSquareMeters
    ) external onlyOwner {
        uint256 tokenId = nextTokenId;
        landParcels[tokenId] = LandParcel(parcelId, location, sizeInSquareMeters);
        _mint(to, tokenId);
        nextTokenId++;
    }

    function getLandDetails(uint256 tokenId) external view returns (
        string memory parcelId,
        string memory location,
        uint256 size
    ) {
        require(_exists(tokenId), "Token does not exist");
        LandParcel memory land = landParcels[tokenId];
        return (land.parcelId, land.location, land.sizeInSquareMeters);
    }

    function transferLand(address to, uint256 tokenId) external {
        require(ownerOf(tokenId) == msg.sender, "You don't own this land");
        _transfer(msg.sender, to, tokenId);
    }
}# landlord
Tokenized land ownership
