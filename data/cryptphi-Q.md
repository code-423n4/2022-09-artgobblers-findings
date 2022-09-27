
1. Missing zero address check
ArtGobblers.constructor() - goo, pages, _team, _community, _randProvider arguments
Pages.constructor() - _goo, _community, _artGobblers arguments
Goo.constructor() - _artGobblers , _pages arguments
GobblerReserve.constructor() - _artGobblers , _owner arguments
Owned.setOwner() - newOwner argument


2. Local variable shadowing
The local variable `owner` in ArtGobblers.gobble() shadows state variable Owner.owner
https://github.com/crytic/slither/wiki/Detector-Documentation#local-variable-shadowing



3.  Floating Pragma is set
The current pragma Solidity directive is "">=0.8.0"". It is recommended to specify a fixed compiler version to ensure that the bytecode produced does not vary between builds. This is
especially important if you rely on bytecode-level verification of the code.

ArtGobblers.sol
GobblerReserve.sol
Pages.sol
Goo.sol
PagesERC721.sol
GobblersERC1155B.sol
GobblersERC721.sol
RandProvider.sol
ChainlinkV1RandProvider.sol
VRGDA.sol
LinearVRGDA.sol
LibGOO.sol
Owned.sol
FixedPointMathLib.sol
ERC721.sol
SignedWadMath.sol
LogisticToLinearVRGDA.sol
MerkleProofLib.sol
LibString.sol
LogisticVRGDA.sol
