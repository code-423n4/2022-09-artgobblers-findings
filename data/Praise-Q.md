please this is my first audit ;)  I'm still trying to learn the ropes, i believe practice makes perfect.


ArtGobblers.revealGobblers(uint256) (src/ArtGobblers.sol#576-685) contains an incorrect shift operation: randomSeed = keccak256(uint256,uint256)(0,32) % 1 << 64 (src/ArtGobblers.sol#674)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#shift-parameter-mixup

FixedPointMathLib.rpow(uint256,uint256,uint256) (lib/solmate/src/utils/FixedPointMathLib.sol#74-160) performs a multiplication on the result of a division:
	-x = xxRound_rpow_asm_0 / scalar (lib/solmate/src/utils/FixedPointMathLib.sol#131)
	-zx_rpow_asm_0 = z * x (lib/solmate/src/utils/FixedPointMathLib.sol#136)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#divide-before-multiply

Reentrancy in ArtGobblers.mintFromGoo(uint256,bool) (src/ArtGobblers.sol#368-390):
	External calls:
	- goo.burnForGobblers(msg.sender,currentPrice) (src/ArtGobblers.sol#379-381)
	State variables written after the call(s):
	- ++ numMintedFromGoo (src/ArtGobblers.sol#384)
Reentrancy in Pages.mintFromGoo(uint256,bool) (src/Pages.sol#195-213):
	External calls:
	- artGobblers.burnGooForPages(msg.sender,currentPrice) (src/Pages.sol#204-206)
	- goo.burnForPages(msg.sender,currentPrice) (src/Pages.sol#204-206)
	State variables written after the call(s):
	- PagePurchased(msg.sender,pageId = ++ currentId,currentPrice) (src/Pages.sol#209)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#reentrancy-vulnerabilities-1

ArtGobblers.gobble(uint256,address,uint256,bool).owner (src/ArtGobblers.sol#730) shadows:
	- Owned.owner (lib/solmate/src/auth/Owned.sol#17) (state variable)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#local-variable-shadowing

Owned.setOwner(address).newOwner (lib/solmate/src/auth/Owned.sol#39) lacks a zero-check on :
		- owner = newOwner (lib/solmate/src/auth/Owned.sol#40)
ArtGobblers.constructor(bytes32,uint256,Goo,Pages,address,address,RandProvider,string,string)._team (src/ArtGobblers.sol#294) lacks a zero-check on :
		- team = _team (src/ArtGobblers.sol#316)
ArtGobblers.constructor(bytes32,uint256,Goo,Pages,address,address,RandProvider,string,string)._community (src/ArtGobblers.sol#295) lacks a zero-check on :
		- community = _community (src/ArtGobblers.sol#317)
Goo.constructor(address,address)._artGobblers (src/Goo.sol#82) lacks a zero-check on :
		- artGobblers = _artGobblers (src/Goo.sol#83)
Goo.constructor(address,address)._pages (src/Goo.sol#82) lacks a zero-check on :
		- pages = _pages (src/Goo.sol#84)
Pages.constructor(uint256,Goo,address,ArtGobblers,string)._community (src/Pages.sol#161) lacks a zero-check on :
		- community = _community (src/Pages.sol#181)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#missing-zero-address-validation

Reentrancy in ArtGobblers.addGoo(uint256) (src/ArtGobblers.sol#770-776):
	External calls:
	- goo.burnForGobblers(msg.sender,gooAmount) (src/ArtGobblers.sol#772)
	State variables written after the call(s):
	- updateUserGooBalance(msg.sender,gooAmount,GooBalanceUpdateType.INCREASE) (src/ArtGobblers.sol#775)
		- getUserData[user].lastBalance = uint128(updatedBalance) (src/ArtGobblers.sol#825)
		- getUserData[user].lastTimestamp = uint64(block.timestamp) (src/ArtGobblers.sol#826)
Reentrancy in ArtGobblers.mintFromGoo(uint256,bool) (src/ArtGobblers.sol#368-390):
	External calls:
	- goo.burnForGobblers(msg.sender,currentPrice) (src/ArtGobblers.sol#379-381)
	State variables written after the call(s):
	- GobblerPurchased(msg.sender,gobblerId = ++ currentNonLegendaryId,currentPrice) (src/ArtGobblers.sol#386)
	- _mint(msg.sender,gobblerId) (src/ArtGobblers.sol#389)
		- getGobblerData[id].owner = to (src/utils/token/GobblersERC721.sol#169)
	- _mint(msg.sender,gobblerId) (src/ArtGobblers.sol#389)
		- ++ getUserData[to].gobblersOwned (src/utils/token/GobblersERC721.sol#166)
Reentrancy in Pages.mintFromGoo(uint256,bool) (src/Pages.sol#195-213):
	External calls:
	- artGobblers.burnGooForPages(msg.sender,currentPrice) (src/Pages.sol#204-206)
	- goo.burnForPages(msg.sender,currentPrice) (src/Pages.sol#204-206)
	State variables written after the call(s):
	- _mint(msg.sender,pageId) (src/Pages.sol#211)
		- _balanceOf[to] ++ (src/utils/token/PagesERC721.sol#181)
	- _mint(msg.sender,pageId) (src/Pages.sol#211)
		- _ownerOf[id] = to (src/utils/token/PagesERC721.sol#184)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#reentrancy-vulnerabilities-2

Reentrancy in ArtGobblers.addGoo(uint256) (src/ArtGobblers.sol#770-776):
	External calls:
	- goo.burnForGobblers(msg.sender,gooAmount) (src/ArtGobblers.sol#772)
	Event emitted after the call(s):
	- GooBalanceUpdated(user,updatedBalance) (src/ArtGobblers.sol#828)
		- updateUserGooBalance(msg.sender,gooAmount,GooBalanceUpdateType.INCREASE) (src/ArtGobblers.sol#775)
Reentrancy in ArtGobblers.mintFromGoo(uint256,bool) (src/ArtGobblers.sol#368-390):
	External calls:
	- goo.burnForGobblers(msg.sender,currentPrice) (src/ArtGobblers.sol#379-381)
	Event emitted after the call(s):
	- GobblerPurchased(msg.sender,gobblerId = ++ currentNonLegendaryId,currentPrice) (src/ArtGobblers.sol#386)
	- Transfer(address(0),to,id) (src/utils/token/GobblersERC721.sol#171)
		- _mint(msg.sender,gobblerId) (src/ArtGobblers.sol#389)
Reentrancy in Pages.mintFromGoo(uint256,bool) (src/Pages.sol#195-213):
	External calls:
	- artGobblers.burnGooForPages(msg.sender,currentPrice) (src/Pages.sol#204-206)
	- goo.burnForPages(msg.sender,currentPrice) (src/Pages.sol#204-206)
	Event emitted after the call(s):
	- PagePurchased(msg.sender,pageId = ++ currentId,currentPrice) (src/Pages.sol#209)
	- Transfer(address(0),to,id) (src/utils/token/PagesERC721.sol#186)
		- _mint(msg.sender,pageId) (src/Pages.sol#211)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#reentrancy-vulnerabilities-3

ERC20.permit(address,address,uint256,uint256,uint8,bytes32,bytes32) (lib/solmate/src/tokens/ERC20.sol#116-160) uses timestamp for comparisons
	Dangerous comparisons:
	- require(bool,string)(deadline >= block.timestamp,PERMIT_DEADLINE_EXPIRED) (lib/solmate/src/tokens/ERC20.sol#125)
ArtGobblers.claimGobbler(bytes32[]) (src/ArtGobblers.sol#339-357) uses timestamp for comparisons
	Dangerous comparisons:
	- mintStart > block.timestamp (src/ArtGobblers.sol#341)
ArtGobblers.requestRandomSeed() (src/ArtGobblers.sol#509-542) uses timestamp for comparisons
	Dangerous comparisons:
	- block.timestamp < nextRevealTimestamp (src/ArtGobblers.sol#513)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#block-timestamp

FixedPointMathLib.mulDivDown(uint256,uint256,uint256) (lib/solmate/src/utils/FixedPointMathLib.sol#34-51) uses assembly
	- INLINE ASM (lib/solmate/src/utils/FixedPointMathLib.sol#39-50)
FixedPointMathLib.mulDivUp(uint256,uint256,uint256) (lib/solmate/src/utils/FixedPointMathLib.sol#53-72) uses assembly
	- INLINE ASM (lib/solmate/src/utils/FixedPointMathLib.sol#58-71)
FixedPointMathLib.rpow(uint256,uint256,uint256) (lib/solmate/src/utils/FixedPointMathLib.sol#74-160) uses assembly
	- INLINE ASM (lib/solmate/src/utils/FixedPointMathLib.sol#79-159)
FixedPointMathLib.sqrt(uint256) (lib/solmate/src/utils/FixedPointMathLib.sol#166-228) uses assembly
	- INLINE ASM (lib/solmate/src/utils/FixedPointMathLib.sol#167-227)
FixedPointMathLib.unsafeMod(uint256,uint256) (lib/solmate/src/utils/FixedPointMathLib.sol#230-236) uses assembly
	- INLINE ASM (lib/solmate/src/utils/FixedPointMathLib.sol#231-235)
FixedPointMathLib.unsafeDiv(uint256,uint256) (lib/solmate/src/utils/FixedPointMathLib.sol#238-244) uses assembly
	- INLINE ASM (lib/solmate/src/utils/FixedPointMathLib.sol#239-243)
FixedPointMathLib.unsafeDivUp(uint256,uint256) (lib/solmate/src/utils/FixedPointMathLib.sol#246-252) uses assembly
	- INLINE ASM (lib/solmate/src/utils/FixedPointMathLib.sol#247-251)
LibString.toString(uint256) (lib/solmate/src/utils/LibString.sol#8-54) uses assembly
	- INLINE ASM (lib/solmate/src/utils/LibString.sol#9-53)
MerkleProofLib.verify(bytes32[],bytes32,bytes32) (lib/solmate/src/utils/MerkleProofLib.sol#8-46) uses assembly
	- INLINE ASM (lib/solmate/src/utils/MerkleProofLib.sol#13-45)
ArtGobblers.revealGobblers(uint256) (src/ArtGobblers.sol#576-685) uses assembly
	- INLINE ASM (src/ArtGobblers.sol#638-646)
	- INLINE ASM (src/ArtGobblers.sol#670-675)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#assembly-usage

ERC1155._batchBurn(address,uint256[],uint256[]) (lib/solmate/src/tokens/ERC1155.sol#202-222) is never used and should be removed
ERC1155._batchMint(address,uint256[],uint256[],bytes) (lib/solmate/src/tokens/ERC1155.sol#171-200) is never used and should be removed
ERC1155._burn(address,uint256,uint256) (lib/solmate/src/tokens/ERC1155.sol#224-232) is never used and should be removed
ERC1155._mint(address,uint256,uint256,bytes) (lib/solmate/src/tokens/ERC1155.sol#152-169) is never used and should be removed
ERC721._burn(uint256) (lib/solmate/src/tokens/ERC721.sol#172-187) is never used and should be removed
ERC721._mint(address,uint256) (lib/solmate/src/tokens/ERC721.sol#157-170) is never used and should be removed
ERC721._safeMint(address,uint256) (lib/solmate/src/tokens/ERC721.sol#193-202) is never used and should be removed
ERC721._safeMint(address,uint256,bytes) (lib/solmate/src/tokens/ERC721.sol#204-217) is never used and should be removed
FixedPointMathLib.divWadDown(uint256,uint256) (lib/solmate/src/utils/FixedPointMathLib.sol#22-24) is never used and should be removed
FixedPointMathLib.divWadUp(uint256,uint256) (lib/solmate/src/utils/FixedPointMathLib.sol#26-28) is never used and should be removed
FixedPointMathLib.mulDivUp(uint256,uint256,uint256) (lib/solmate/src/utils/FixedPointMathLib.sol#53-72) is never used and should be removed
FixedPointMathLib.mulWadUp(uint256,uint256) (lib/solmate/src/utils/FixedPointMathLib.sol#18-20) is never used and should be removed
FixedPointMathLib.rpow(uint256,uint256,uint256) (lib/solmate/src/utils/FixedPointMathLib.sol#74-160) is never used and should be removed
FixedPointMathLib.unsafeDiv(uint256,uint256) (lib/solmate/src/utils/FixedPointMathLib.sol#238-244) is never used and should be removed
FixedPointMathLib.unsafeMod(uint256,uint256) (lib/solmate/src/utils/FixedPointMathLib.sol#230-236) is never used and should be removed
fromDaysWadUnsafe(int256) (lib/solmate/src/utils/SignedWadMath.sol#28-33) is never used and should be removed
wadDiv(int256,int256) (lib/solmate/src/utils/SignedWadMath.sol#67-80) is never used and should be removed
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#dead-code

Pragma version>=0.8.0 (lib/VRGDAs/src/LogisticToLinearVRGDA.sol#2) allows old versions
Pragma version>=0.8.0 (lib/VRGDAs/src/LogisticVRGDA.sol#2) allows old versions
Pragma version>=0.8.0 (lib/VRGDAs/src/VRGDA.sol#2) allows old versions
Pragma version>=0.8.0 (lib/goo-issuance/src/LibGOO.sol#2) allows old versions
Pragma version>=0.8.0 (lib/solmate/src/auth/Owned.sol#2) allows old versions
Pragma version>=0.8.0 (lib/solmate/src/tokens/ERC1155.sol#2) allows old versions
Pragma version>=0.8.0 (lib/solmate/src/tokens/ERC20.sol#2) allows old versions
Pragma version>=0.8.0 (lib/solmate/src/tokens/ERC721.sol#2) allows old versions
Pragma version>=0.8.0 (lib/solmate/src/utils/FixedPointMathLib.sol#2) allows old versions
Pragma version>=0.8.0 (lib/solmate/src/utils/LibString.sol#2) allows old versions
Pragma version>=0.8.0 (lib/solmate/src/utils/MerkleProofLib.sol#2) allows old versions
Pragma version>=0.8.0 (lib/solmate/src/utils/SignedWadMath.sol#2) allows old versions
Pragma version>=0.8.0 (src/ArtGobblers.sol#2) allows old versions
Pragma version>=0.8.0 (src/Goo.sol#2) allows old versions
Pragma version>=0.8.0 (src/Pages.sol#2) allows old versions
Pragma version>=0.8.0 (src/utils/rand/RandProvider.sol#2) allows old versions
Pragma version>=0.8.0 (src/utils/token/GobblersERC721.sol#2) allows old versions
Pragma version>=0.8.0 (src/utils/token/PagesERC721.sol#2) allows old versions
solc-0.8.17 is not recommended for deployment
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#incorrect-versions-of-solidity

Function ERC20.DOMAIN_SEPARATOR() (lib/solmate/src/tokens/ERC20.sol#162-164) is not in mixedCase
Variable ERC20.INITIAL_CHAIN_ID (lib/solmate/src/tokens/ERC20.sol#41) is not in mixedCase
Variable ERC20.INITIAL_DOMAIN_SEPARATOR (lib/solmate/src/tokens/ERC20.sol#43) is not in mixedCase
Variable ERC721._ownerOf (lib/solmate/src/tokens/ERC721.sol#31) is not in mixedCase
Variable ERC721._balanceOf (lib/solmate/src/tokens/ERC721.sol#33) is not in mixedCase
Variable ArtGobblers.UNREVEALED_URI (src/ArtGobblers.sol#136) is not in mixedCase
Variable ArtGobblers.BASE_URI (src/ArtGobblers.sol#139) is not in mixedCase
Variable Pages.BASE_URI (src/Pages.sol#96) is not in mixedCase
Variable PagesERC721._ownerOf (src/utils/token/PagesERC721.sol#50) is not in mixedCase
Variable PagesERC721._balanceOf (src/utils/token/PagesERC721.sol#52) is not in mixedCase
Variable PagesERC721._isApprovedForAll (src/utils/token/PagesERC721.sol#70) is not in mixedCase
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#conformance-to-solidity-naming-conventions

Variable ArtGobblers.UNREVEALED_URI (src/ArtGobblers.sol#136) is too similar to ArtGobblers.constructor(bytes32,uint256,Goo,Pages,address,address,RandProvider,string,string)._unrevealedUri (src/ArtGobblers.sol#299)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#variable-names-are-too-similar

FixedPointMathLib.sqrt(uint256) (lib/solmate/src/utils/FixedPointMathLib.sol#166-228) uses literals with too many digits:
	- ! y_sqrt_asm_0 < 0x10000000000000000000000000000000000 (lib/solmate/src/utils/FixedPointMathLib.sol#177-180)
FixedPointMathLib.sqrt(uint256) (lib/solmate/src/utils/FixedPointMathLib.sol#166-228) uses literals with too many digits:
	- ! y_sqrt_asm_0 < 0x1000000000000000000 (lib/solmate/src/utils/FixedPointMathLib.sol#181-184)
FixedPointMathLib.sqrt(uint256) (lib/solmate/src/utils/FixedPointMathLib.sol#166-228) uses literals with too many digits:
	- ! y_sqrt_asm_0 < 0x10000000000 (lib/solmate/src/utils/FixedPointMathLib.sol#185-188)
FixedPointMathLib.sqrt(uint256) (lib/solmate/src/utils/FixedPointMathLib.sol#166-228) uses literals with too many digits:
	- ! y_sqrt_asm_0 < 0x1000000 (lib/solmate/src/utils/FixedPointMathLib.sol#189-192)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#too-many-digits

ERC1155 (lib/solmate/src/tokens/ERC1155.sol#6-233) does not implement functions:
	- ERC1155.uri(uint256) (lib/solmate/src/tokens/ERC1155.sol#43)
ERC721 (lib/solmate/src/tokens/ERC721.sol#6-218) does not implement functions:
	- ERC721.tokenURI(uint256) (lib/solmate/src/tokens/ERC721.sol#25)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#unimplemented-functions

computeGOOBalance(uint256,uint256,uint256) should be declared external:
	- LibGOO.computeGOOBalance(uint256,uint256,uint256) (lib/goo-issuance/src/LibGOO.sol#17-41)
setOwner(address) should be declared external:
	- Owned.setOwner(address) (lib/solmate/src/auth/Owned.sol#39-43)
uri(uint256) should be declared external:
	- ERC1155.uri(uint256) (lib/solmate/src/tokens/ERC1155.sol#43)
setApprovalForAll(address,bool) should be declared external:
	- ERC1155.setApprovalForAll(address,bool) (lib/solmate/src/tokens/ERC1155.sol#49-53)
safeTransferFrom(address,address,uint256,uint256,bytes) should be declared external:
	- ERC1155.safeTransferFrom(address,address,uint256,uint256,bytes) (lib/solmate/src/tokens/ERC1155.sol#55-76)
safeBatchTransferFrom(address,address,uint256[],uint256[],bytes) should be declared external:
	- ERC1155.safeBatchTransferFrom(address,address,uint256[],uint256[],bytes) (lib/solmate/src/tokens/ERC1155.sol#78-116)
balanceOfBatch(address[],uint256[]) should be declared external:
	- ERC1155.balanceOfBatch(address[],uint256[]) (lib/solmate/src/tokens/ERC1155.sol#118-135)
supportsInterface(bytes4) should be declared external:
	- ERC1155.supportsInterface(bytes4) (lib/solmate/src/tokens/ERC1155.sol#141-146)
approve(address,uint256) should be declared external:
	- ERC20.approve(address,uint256) (lib/solmate/src/tokens/ERC20.sol#68-74)
transfer(address,uint256) should be declared external:
	- ERC20.transfer(address,uint256) (lib/solmate/src/tokens/ERC20.sol#76-88)
transferFrom(address,address,uint256) should be declared external:
	- ERC20.transferFrom(address,address,uint256) (lib/solmate/src/tokens/ERC20.sol#90-110)
permit(address,address,uint256,uint256,uint8,bytes32,bytes32) should be declared external:
	- ERC20.permit(address,address,uint256,uint256,uint8,bytes32,bytes32) (lib/solmate/src/tokens/ERC20.sol#116-160)
tokenURI(uint256) should be declared external:
	- ERC721.tokenURI(uint256) (lib/solmate/src/tokens/ERC721.sol#25)
ownerOf(uint256) should be declared external:
	- ERC721.ownerOf(uint256) (lib/solmate/src/tokens/ERC721.sol#35-37)
balanceOf(address) should be declared external:
	- ERC721.balanceOf(address) (lib/solmate/src/tokens/ERC721.sol#39-43)
approve(address,uint256) should be declared external:
	- ERC721.approve(address,uint256) (lib/solmate/src/tokens/ERC721.sol#66-74)
setApprovalForAll(address,bool) should be declared external:
	- ERC721.setApprovalForAll(address,bool) (lib/solmate/src/tokens/ERC721.sol#76-80)
safeTransferFrom(address,address,uint256) should be declared external:
	- ERC721.safeTransferFrom(address,address,uint256) (lib/solmate/src/tokens/ERC721.sol#111-124)
safeTransferFrom(address,address,uint256,bytes) should be declared external:
	- ERC721.safeTransferFrom(address,address,uint256,bytes) (lib/solmate/src/tokens/ERC721.sol#126-140)
supportsInterface(bytes4) should be declared external:
	- ERC721.supportsInterface(bytes4) (lib/solmate/src/tokens/ERC721.sol#146-151)
tokenURI(uint256) should be declared external:
	- ArtGobblers.tokenURI(uint256) (src/ArtGobblers.sol#693-712)
tokenURI(uint256) should be declared external:
	- Pages.tokenURI(uint256) (src/Pages.sol#265-269)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#public-function-that-could-be-declared-external

Goo.constructor(address,address)._artGobblers (src/Goo.sol#82) lacks a zero-check on :
		- artGobblers = _artGobblers (src/Goo.sol#83)
Goo.constructor(address,address)._pages (src/Goo.sol#82) lacks a zero-check on :
		- pages = _pages (src/Goo.sol#84)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#missing-zero-address-validation

ERC20.permit(address,address,uint256,uint256,uint8,bytes32,bytes32) (lib/solmate/src/tokens/ERC20.sol#116-160) uses timestamp for comparisons
	Dangerous comparisons:
	- require(bool,string)(deadline >= block.timestamp,PERMIT_DEADLINE_EXPIRED) (lib/solmate/src/tokens/ERC20.sol#125)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#block-timestamp

Pragma version>=0.8.0 (lib/solmate/src/tokens/ERC20.sol#2) allows old versions
Pragma version>=0.8.0 (src/Goo.sol#2) allows old versions
solc-0.8.17 is not recommended for deployment
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#incorrect-versions-of-solidity

Function ERC20.DOMAIN_SEPARATOR() (lib/solmate/src/tokens/ERC20.sol#162-164) is not in mixedCase
Variable ERC20.INITIAL_CHAIN_ID (lib/solmate/src/tokens/ERC20.sol#41) is not in mixedCase
Variable ERC20.INITIAL_DOMAIN_SEPARATOR (lib/solmate/src/tokens/ERC20.sol#43) is not in mixedCase
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#conformance-to-solidity-naming-conventions

approve(address,uint256) should be declared external:
	- ERC20.approve(address,uint256) (lib/solmate/src/tokens/ERC20.sol#68-74)
transfer(address,uint256) should be declared external:
	- ERC20.transfer(address,uint256) (lib/solmate/src/tokens/ERC20.sol#76-88)
transferFrom(address,address,uint256) should be declared external:
	- ERC20.transferFrom(address,address,uint256) (lib/solmate/src/tokens/ERC20.sol#90-110)
permit(address,address,uint256,uint256,uint8,bytes32,bytes32) should be declared external:
	- ERC20.permit(address,address,uint256,uint256,uint8,bytes32,bytes32) (lib/solmate/src/tokens/ERC20.sol#116-160)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#public-function-that-could-be-declared-external

ArtGobblers.revealGobblers(uint256) (src/ArtGobblers.sol#576-685) contains an incorrect shift operation: randomSeed = keccak256(uint256,uint256)(0,32) % 1 << 64 (src/ArtGobblers.sol#674)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#shift-parameter-mixup

FixedPointMathLib.rpow(uint256,uint256,uint256) (lib/solmate/src/utils/FixedPointMathLib.sol#74-160) performs a multiplication on the result of a division:
	-x = xxRound_rpow_asm_0 / scalar (lib/solmate/src/utils/FixedPointMathLib.sol#131)
	-zx_rpow_asm_0 = z * x (lib/solmate/src/utils/FixedPointMathLib.sol#136)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#divide-before-multiply

Reentrancy in ArtGobblers.mintFromGoo(uint256,bool) (src/ArtGobblers.sol#368-390):
	External calls:
	- goo.burnForGobblers(msg.sender,currentPrice) (src/ArtGobblers.sol#379-381)
	State variables written after the call(s):
	- ++ numMintedFromGoo (src/ArtGobblers.sol#384)
Reentrancy in Pages.mintFromGoo(uint256,bool) (src/Pages.sol#195-213):
	External calls:
	- artGobblers.burnGooForPages(msg.sender,currentPrice) (src/Pages.sol#204-206)
	- goo.burnForPages(msg.sender,currentPrice) (src/Pages.sol#204-206)
	State variables written after the call(s):
	- PagePurchased(msg.sender,pageId = ++ currentId,currentPrice) (src/Pages.sol#209)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#reentrancy-vulnerabilities-1

ArtGobblers.gobble(uint256,address,uint256,bool).owner (src/ArtGobblers.sol#730) shadows:
	- Owned.owner (lib/solmate/src/auth/Owned.sol#17) (state variable)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#local-variable-shadowing

Owned.setOwner(address).newOwner (lib/solmate/src/auth/Owned.sol#39) lacks a zero-check on :
		- owner = newOwner (lib/solmate/src/auth/Owned.sol#40)
ArtGobblers.constructor(bytes32,uint256,Goo,Pages,address,address,RandProvider,string,string)._team (src/ArtGobblers.sol#294) lacks a zero-check on :
		- team = _team (src/ArtGobblers.sol#316)
ArtGobblers.constructor(bytes32,uint256,Goo,Pages,address,address,RandProvider,string,string)._community (src/ArtGobblers.sol#295) lacks a zero-check on :
		- community = _community (src/ArtGobblers.sol#317)
Goo.constructor(address,address)._artGobblers (src/Goo.sol#82) lacks a zero-check on :
		- artGobblers = _artGobblers (src/Goo.sol#83)
Goo.constructor(address,address)._pages (src/Goo.sol#82) lacks a zero-check on :
		- pages = _pages (src/Goo.sol#84)
Pages.constructor(uint256,Goo,address,ArtGobblers,string)._community (src/Pages.sol#161) lacks a zero-check on :
		- community = _community (src/Pages.sol#181)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#missing-zero-address-validation

Reentrancy in ArtGobblers.addGoo(uint256) (src/ArtGobblers.sol#770-776):
	External calls:
	- goo.burnForGobblers(msg.sender,gooAmount) (src/ArtGobblers.sol#772)
	State variables written after the call(s):
	- updateUserGooBalance(msg.sender,gooAmount,GooBalanceUpdateType.INCREASE) (src/ArtGobblers.sol#775)
		- getUserData[user].lastBalance = uint128(updatedBalance) (src/ArtGobblers.sol#825)
		- getUserData[user].lastTimestamp = uint64(block.timestamp) (src/ArtGobblers.sol#826)
Reentrancy in ArtGobblers.mintFromGoo(uint256,bool) (src/ArtGobblers.sol#368-390):
	External calls:
	- goo.burnForGobblers(msg.sender,currentPrice) (src/ArtGobblers.sol#379-381)
	State variables written after the call(s):
	- GobblerPurchased(msg.sender,gobblerId = ++ currentNonLegendaryId,currentPrice) (src/ArtGobblers.sol#386)
	- _mint(msg.sender,gobblerId) (src/ArtGobblers.sol#389)
		- getGobblerData[id].owner = to (src/utils/token/GobblersERC721.sol#169)
	- _mint(msg.sender,gobblerId) (src/ArtGobblers.sol#389)
		- ++ getUserData[to].gobblersOwned (src/utils/token/GobblersERC721.sol#166)
Reentrancy in Pages.mintFromGoo(uint256,bool) (src/Pages.sol#195-213):
	External calls:
	- artGobblers.burnGooForPages(msg.sender,currentPrice) (src/Pages.sol#204-206)
	- goo.burnForPages(msg.sender,currentPrice) (src/Pages.sol#204-206)
	State variables written after the call(s):
	- _mint(msg.sender,pageId) (src/Pages.sol#211)
		- _balanceOf[to] ++ (src/utils/token/PagesERC721.sol#181)
	- _mint(msg.sender,pageId) (src/Pages.sol#211)
		- _ownerOf[id] = to (src/utils/token/PagesERC721.sol#184)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#reentrancy-vulnerabilities-2

Reentrancy in ArtGobblers.addGoo(uint256) (src/ArtGobblers.sol#770-776):
	External calls:
	- goo.burnForGobblers(msg.sender,gooAmount) (src/ArtGobblers.sol#772)
	Event emitted after the call(s):
	- GooBalanceUpdated(user,updatedBalance) (src/ArtGobblers.sol#828)
		- updateUserGooBalance(msg.sender,gooAmount,GooBalanceUpdateType.INCREASE) (src/ArtGobblers.sol#775)
Reentrancy in ArtGobblers.mintFromGoo(uint256,bool) (src/ArtGobblers.sol#368-390):
	External calls:
	- goo.burnForGobblers(msg.sender,currentPrice) (src/ArtGobblers.sol#379-381)
	Event emitted after the call(s):
	- GobblerPurchased(msg.sender,gobblerId = ++ currentNonLegendaryId,currentPrice) (src/ArtGobblers.sol#386)
	- Transfer(address(0),to,id) (src/utils/token/GobblersERC721.sol#171)
		- _mint(msg.sender,gobblerId) (src/ArtGobblers.sol#389)
Reentrancy in Pages.mintFromGoo(uint256,bool) (src/Pages.sol#195-213):
	External calls:
	- artGobblers.burnGooForPages(msg.sender,currentPrice) (src/Pages.sol#204-206)
	- goo.burnForPages(msg.sender,currentPrice) (src/Pages.sol#204-206)
	Event emitted after the call(s):
	- PagePurchased(msg.sender,pageId = ++ currentId,currentPrice) (src/Pages.sol#209)
	- Transfer(address(0),to,id) (src/utils/token/PagesERC721.sol#186)
		- _mint(msg.sender,pageId) (src/Pages.sol#211)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#reentrancy-vulnerabilities-3

ERC20.permit(address,address,uint256,uint256,uint8,bytes32,bytes32) (lib/solmate/src/tokens/ERC20.sol#116-160) uses timestamp for comparisons
	Dangerous comparisons:
	- require(bool,string)(deadline >= block.timestamp,PERMIT_DEADLINE_EXPIRED) (lib/solmate/src/tokens/ERC20.sol#125)
ArtGobblers.claimGobbler(bytes32[]) (src/ArtGobblers.sol#339-357) uses timestamp for comparisons
	Dangerous comparisons:
	- mintStart > block.timestamp (src/ArtGobblers.sol#341)
ArtGobblers.requestRandomSeed() (src/ArtGobblers.sol#509-542) uses timestamp for comparisons
	Dangerous comparisons:
	- block.timestamp < nextRevealTimestamp (src/ArtGobblers.sol#513)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#block-timestamp

FixedPointMathLib.mulDivDown(uint256,uint256,uint256) (lib/solmate/src/utils/FixedPointMathLib.sol#34-51) uses assembly
	- INLINE ASM (lib/solmate/src/utils/FixedPointMathLib.sol#39-50)
FixedPointMathLib.mulDivUp(uint256,uint256,uint256) (lib/solmate/src/utils/FixedPointMathLib.sol#53-72) uses assembly
	- INLINE ASM (lib/solmate/src/utils/FixedPointMathLib.sol#58-71)
FixedPointMathLib.rpow(uint256,uint256,uint256) (lib/solmate/src/utils/FixedPointMathLib.sol#74-160) uses assembly
	- INLINE ASM (lib/solmate/src/utils/FixedPointMathLib.sol#79-159)
FixedPointMathLib.sqrt(uint256) (lib/solmate/src/utils/FixedPointMathLib.sol#166-228) uses assembly
	- INLINE ASM (lib/solmate/src/utils/FixedPointMathLib.sol#167-227)
FixedPointMathLib.unsafeMod(uint256,uint256) (lib/solmate/src/utils/FixedPointMathLib.sol#230-236) uses assembly
	- INLINE ASM (lib/solmate/src/utils/FixedPointMathLib.sol#231-235)
FixedPointMathLib.unsafeDiv(uint256,uint256) (lib/solmate/src/utils/FixedPointMathLib.sol#238-244) uses assembly
	- INLINE ASM (lib/solmate/src/utils/FixedPointMathLib.sol#239-243)
FixedPointMathLib.unsafeDivUp(uint256,uint256) (lib/solmate/src/utils/FixedPointMathLib.sol#246-252) uses assembly
	- INLINE ASM (lib/solmate/src/utils/FixedPointMathLib.sol#247-251)
LibString.toString(uint256) (lib/solmate/src/utils/LibString.sol#8-54) uses assembly
	- INLINE ASM (lib/solmate/src/utils/LibString.sol#9-53)
MerkleProofLib.verify(bytes32[],bytes32,bytes32) (lib/solmate/src/utils/MerkleProofLib.sol#8-46) uses assembly
	- INLINE ASM (lib/solmate/src/utils/MerkleProofLib.sol#13-45)
ArtGobblers.revealGobblers(uint256) (src/ArtGobblers.sol#576-685) uses assembly
	- INLINE ASM (src/ArtGobblers.sol#638-646)
	- INLINE ASM (src/ArtGobblers.sol#670-675)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#assembly-usage

ERC1155._batchBurn(address,uint256[],uint256[]) (lib/solmate/src/tokens/ERC1155.sol#202-222) is never used and should be removed
ERC1155._batchMint(address,uint256[],uint256[],bytes) (lib/solmate/src/tokens/ERC1155.sol#171-200) is never used and should be removed
ERC1155._burn(address,uint256,uint256) (lib/solmate/src/tokens/ERC1155.sol#224-232) is never used and should be removed
ERC1155._mint(address,uint256,uint256,bytes) (lib/solmate/src/tokens/ERC1155.sol#152-169) is never used and should be removed
ERC721._burn(uint256) (lib/solmate/src/tokens/ERC721.sol#172-187) is never used and should be removed
ERC721._mint(address,uint256) (lib/solmate/src/tokens/ERC721.sol#157-170) is never used and should be removed
ERC721._safeMint(address,uint256) (lib/solmate/src/tokens/ERC721.sol#193-202) is never used and should be removed
ERC721._safeMint(address,uint256,bytes) (lib/solmate/src/tokens/ERC721.sol#204-217) is never used and should be removed
FixedPointMathLib.divWadDown(uint256,uint256) (lib/solmate/src/utils/FixedPointMathLib.sol#22-24) is never used and should be removed
FixedPointMathLib.divWadUp(uint256,uint256) (lib/solmate/src/utils/FixedPointMathLib.sol#26-28) is never used and should be removed
FixedPointMathLib.mulDivUp(uint256,uint256,uint256) (lib/solmate/src/utils/FixedPointMathLib.sol#53-72) is never used and should be removed
FixedPointMathLib.mulWadUp(uint256,uint256) (lib/solmate/src/utils/FixedPointMathLib.sol#18-20) is never used and should be removed
FixedPointMathLib.rpow(uint256,uint256,uint256) (lib/solmate/src/utils/FixedPointMathLib.sol#74-160) is never used and should be removed
FixedPointMathLib.unsafeDiv(uint256,uint256) (lib/solmate/src/utils/FixedPointMathLib.sol#238-244) is never used and should be removed
FixedPointMathLib.unsafeMod(uint256,uint256) (lib/solmate/src/utils/FixedPointMathLib.sol#230-236) is never used and should be removed
fromDaysWadUnsafe(int256) (lib/solmate/src/utils/SignedWadMath.sol#28-33) is never used and should be removed
wadDiv(int256,int256) (lib/solmate/src/utils/SignedWadMath.sol#67-80) is never used and should be removed
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#dead-code

Pragma version>=0.8.0 (lib/VRGDAs/src/LogisticToLinearVRGDA.sol#2) allows old versions
Pragma version>=0.8.0 (lib/VRGDAs/src/LogisticVRGDA.sol#2) allows old versions
Pragma version>=0.8.0 (lib/VRGDAs/src/VRGDA.sol#2) allows old versions
Pragma version>=0.8.0 (lib/goo-issuance/src/LibGOO.sol#2) allows old versions
Pragma version>=0.8.0 (lib/solmate/src/auth/Owned.sol#2) allows old versions
Pragma version>=0.8.0 (lib/solmate/src/tokens/ERC1155.sol#2) allows old versions
Pragma version>=0.8.0 (lib/solmate/src/tokens/ERC20.sol#2) allows old versions
Pragma version>=0.8.0 (lib/solmate/src/tokens/ERC721.sol#2) allows old versions
Pragma version>=0.8.0 (lib/solmate/src/utils/FixedPointMathLib.sol#2) allows old versions
Pragma version>=0.8.0 (lib/solmate/src/utils/LibString.sol#2) allows old versions
Pragma version>=0.8.0 (lib/solmate/src/utils/MerkleProofLib.sol#2) allows old versions
Pragma version>=0.8.0 (lib/solmate/src/utils/SignedWadMath.sol#2) allows old versions
Pragma version>=0.8.0 (src/ArtGobblers.sol#2) allows old versions
Pragma version>=0.8.0 (src/Goo.sol#2) allows old versions
Pragma version>=0.8.0 (src/Pages.sol#2) allows old versions
Pragma version>=0.8.0 (src/utils/rand/RandProvider.sol#2) allows old versions
Pragma version>=0.8.0 (src/utils/token/GobblersERC721.sol#2) allows old versions
Pragma version>=0.8.0 (src/utils/token/PagesERC721.sol#2) allows old versions
solc-0.8.17 is not recommended for deployment
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#incorrect-versions-of-solidity

Function ERC20.DOMAIN_SEPARATOR() (lib/solmate/src/tokens/ERC20.sol#162-164) is not in mixedCase
Variable ERC20.INITIAL_CHAIN_ID (lib/solmate/src/tokens/ERC20.sol#41) is not in mixedCase
Variable ERC20.INITIAL_DOMAIN_SEPARATOR (lib/solmate/src/tokens/ERC20.sol#43) is not in mixedCase
Variable ERC721._ownerOf (lib/solmate/src/tokens/ERC721.sol#31) is not in mixedCase
Variable ERC721._balanceOf (lib/solmate/src/tokens/ERC721.sol#33) is not in mixedCase
Variable ArtGobblers.UNREVEALED_URI (src/ArtGobblers.sol#136) is not in mixedCase
Variable ArtGobblers.BASE_URI (src/ArtGobblers.sol#139) is not in mixedCase
Variable Pages.BASE_URI (src/Pages.sol#96) is not in mixedCase
Variable PagesERC721._ownerOf (src/utils/token/PagesERC721.sol#50) is not in mixedCase
Variable PagesERC721._balanceOf (src/utils/token/PagesERC721.sol#52) is not in mixedCase
Variable PagesERC721._isApprovedForAll (src/utils/token/PagesERC721.sol#70) is not in mixedCase
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#conformance-to-solidity-naming-conventions

Variable ArtGobblers.UNREVEALED_URI (src/ArtGobblers.sol#136) is too similar to ArtGobblers.constructor(bytes32,uint256,Goo,Pages,address,address,RandProvider,string,string)._unrevealedUri (src/ArtGobblers.sol#299)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#variable-names-are-too-similar

FixedPointMathLib.sqrt(uint256) (lib/solmate/src/utils/FixedPointMathLib.sol#166-228) uses literals with too many digits:
	- ! y_sqrt_asm_0 < 0x10000000000000000000000000000000000 (lib/solmate/src/utils/FixedPointMathLib.sol#177-180)
FixedPointMathLib.sqrt(uint256) (lib/solmate/src/utils/FixedPointMathLib.sol#166-228) uses literals with too many digits:
	- ! y_sqrt_asm_0 < 0x1000000000000000000 (lib/solmate/src/utils/FixedPointMathLib.sol#181-184)
FixedPointMathLib.sqrt(uint256) (lib/solmate/src/utils/FixedPointMathLib.sol#166-228) uses literals with too many digits:
	- ! y_sqrt_asm_0 < 0x10000000000 (lib/solmate/src/utils/FixedPointMathLib.sol#185-188)
FixedPointMathLib.sqrt(uint256) (lib/solmate/src/utils/FixedPointMathLib.sol#166-228) uses literals with too many digits:
	- ! y_sqrt_asm_0 < 0x1000000 (lib/solmate/src/utils/FixedPointMathLib.sol#189-192)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#too-many-digits

ERC1155 (lib/solmate/src/tokens/ERC1155.sol#6-233) does not implement functions:
	- ERC1155.uri(uint256) (lib/solmate/src/tokens/ERC1155.sol#43)
ERC721 (lib/solmate/src/tokens/ERC721.sol#6-218) does not implement functions:
	- ERC721.tokenURI(uint256) (lib/solmate/src/tokens/ERC721.sol#25)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#unimplemented-functions

computeGOOBalance(uint256,uint256,uint256) should be declared external:
	- LibGOO.computeGOOBalance(uint256,uint256,uint256) (lib/goo-issuance/src/LibGOO.sol#17-41)
setOwner(address) should be declared external:
	- Owned.setOwner(address) (lib/solmate/src/auth/Owned.sol#39-43)
uri(uint256) should be declared external:
	- ERC1155.uri(uint256) (lib/solmate/src/tokens/ERC1155.sol#43)
setApprovalForAll(address,bool) should be declared external:
	- ERC1155.setApprovalForAll(address,bool) (lib/solmate/src/tokens/ERC1155.sol#49-53)
safeTransferFrom(address,address,uint256,uint256,bytes) should be declared external:
	- ERC1155.safeTransferFrom(address,address,uint256,uint256,bytes) (lib/solmate/src/tokens/ERC1155.sol#55-76)
safeBatchTransferFrom(address,address,uint256[],uint256[],bytes) should be declared external:
	- ERC1155.safeBatchTransferFrom(address,address,uint256[],uint256[],bytes) (lib/solmate/src/tokens/ERC1155.sol#78-116)
balanceOfBatch(address[],uint256[]) should be declared external:
	- ERC1155.balanceOfBatch(address[],uint256[]) (lib/solmate/src/tokens/ERC1155.sol#118-135)
supportsInterface(bytes4) should be declared external:
	- ERC1155.supportsInterface(bytes4) (lib/solmate/src/tokens/ERC1155.sol#141-146)
approve(address,uint256) should be declared external:
	- ERC20.approve(address,uint256) (lib/solmate/src/tokens/ERC20.sol#68-74)
transfer(address,uint256) should be declared external:
	- ERC20.transfer(address,uint256) (lib/solmate/src/tokens/ERC20.sol#76-88)
transferFrom(address,address,uint256) should be declared external:
	- ERC20.transferFrom(address,address,uint256) (lib/solmate/src/tokens/ERC20.sol#90-110)
permit(address,address,uint256,uint256,uint8,bytes32,bytes32) should be declared external:
	- ERC20.permit(address,address,uint256,uint256,uint8,bytes32,bytes32) (lib/solmate/src/tokens/ERC20.sol#116-160)
tokenURI(uint256) should be declared external:
	- ERC721.tokenURI(uint256) (lib/solmate/src/tokens/ERC721.sol#25)
ownerOf(uint256) should be declared external:
	- ERC721.ownerOf(uint256) (lib/solmate/src/tokens/ERC721.sol#35-37)
balanceOf(address) should be declared external:
	- ERC721.balanceOf(address) (lib/solmate/src/tokens/ERC721.sol#39-43)
approve(address,uint256) should be declared external:
	- ERC721.approve(address,uint256) (lib/solmate/src/tokens/ERC721.sol#66-74)
setApprovalForAll(address,bool) should be declared external:
	- ERC721.setApprovalForAll(address,bool) (lib/solmate/src/tokens/ERC721.sol#76-80)
safeTransferFrom(address,address,uint256) should be declared external:
	- ERC721.safeTransferFrom(address,address,uint256) (lib/solmate/src/tokens/ERC721.sol#111-124)
safeTransferFrom(address,address,uint256,bytes) should be declared external:
	- ERC721.safeTransferFrom(address,address,uint256,bytes) (lib/solmate/src/tokens/ERC721.sol#126-140)
supportsInterface(bytes4) should be declared external:
	- ERC721.supportsInterface(bytes4) (lib/solmate/src/tokens/ERC721.sol#146-151)
tokenURI(uint256) should be declared external:
	- ArtGobblers.tokenURI(uint256) (src/ArtGobblers.sol#693-712)
tokenURI(uint256) should be declared external:
	- Pages.tokenURI(uint256) (src/Pages.sol#265-269)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#public-function-that-could-be-declared-external
