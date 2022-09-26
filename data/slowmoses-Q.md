## (1) Consider Uniform SPDX License

Not all source files specify the same license.

***

// SPDX-License-Identifier: MIT
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/ArtGobblers.sol#L1

// SPDX-License-Identifier: AGPL-3.0-only
https://github.com/code-423n4/2022-09-artgobblers/tree/main/src/utils/token/GobblersERC721.sol#L1

## (2) Solidity Version

All source files in the project specify “solidity >=0.8.0”.

Consider using the most recent version of solidity.
0.8.17 is already released.

Also: a fixed version of solidity should be specified in all non-library files.

## (3) Avoid Magic Numbers

Magic numbers should be avoided.
Consider using properly named constants instead.

Use _ for better number readability. (This will not help in the examples cited below.)

***

int256 k = ((x << 96) / 54916777467707473351141471128 + 2**95) >> 96;
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L100

y = ((y * x) >> 96) + 57155421227552351082224309758442;
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L108

p = ((p * y) >> 96) + 28719021644029726153956944680412240;
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L110

p = p * x + (4385272521454847904659076985693276 << 96);
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L111

q = ((q * x) >> 96) + 50020603652535783019961831881945;
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L115

q = ((q * x) >> 96) - 533845033583426703283633433725380;
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L116

q = ((q * x) >> 96) + 3604857256930695427073651918091429;
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L117

q = ((q * x) >> 96) - 14423608567350463180887372962807573;
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L118

q = ((q * x) >> 96) + 26449188498355588339934803723976023;
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L119

x = int256(uint256(x) >> 159);
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L164

p = ((p * x) >> 96) + 24828157081833163892658089445524;
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L169

p = ((p * x) >> 96) + 43456485725739037958740375743393;
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L170

p = ((p * x) >> 96) - 11111509109440967052023855526967;
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L171

p = ((p * x) >> 96) - 45023709667254063763336534515857;
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L172

p = ((p * x) >> 96) - 14706773417378608786704636184526;
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L173

p = p * x - (795164235651350426258249787498 << 96);
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L174

q = ((q * x) >> 96) + 71694874799317883764090561454958;
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L179

q = ((q * x) >> 96) + 283447036172924575727196451306956;
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L180

q = ((q * x) >> 96) + 401686690394027663651624208769553;
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L181

q = ((q * x) >> 96) + 204048457590392012362485061816622;
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L182

q = ((q * x) >> 96) + 31853899698501571402653359427138;
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L183

q = ((q * x) >> 96) + 909429971244387300277376558375;
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L184

r *= 1677202110996718588342820967067443963516166;
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L201

r += 16597577552685614221487285958193947469193820559219878177908093499208371 * k;
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L203

r += 600920179829731861736702779321621459595472258049074101567377883020018308;
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L205

r >>= 174;
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L207

## (4) Revert and Require Should Have Descriptive Reason Strings

revert(0, 0)
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/FixedPointMathLib.sol#L45

revert(0, 0)
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/FixedPointMathLib.sol#L64

revert(0, 0)
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/FixedPointMathLib.sol#L116

revert(0, 0)
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/FixedPointMathLib.sol#L127

revert(0, 0)
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/FixedPointMathLib.sol#L142

revert(0, 0)
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/FixedPointMathLib.sol#L151

revert(0, 0)
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L59

revert(0, 0)
https://github.com/transmissions11/solmate/blob/bff24e835192470ed38bf15dbed6084c2d723ace/src/utils/SignedWadMath.sol#L74


