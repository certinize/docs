# E-Certificate

A digital representation of an official document attesting the legitimacy of the holder’s achievement.

## Background

This definitions of the terms used in this document are available at:

- [https://docs.solana.com/terminology](https://docs.solana.com/terminology)

## E-Certificates and Non-Fungible Tokens

An e-Certificate is represented by a [non-fungible token (NFT)](https://en.wikipedia.org/wiki/Non-fungible_token). This token is then transferred to the recipient’s [associated token account](https://spl.solana.com/associated-token-account). The token has an associated metadata which contains a link to the original copy of the e-Certificate which is stored in a [distributed storage system](https://docs.metaplex.com/guides/storage-overview).

![figure-1](figure-1.svg)

*Figure 1. Common implementation for Non-Fungible tokens.*

## 1. Minting Account

The system will use Solana’s SPL Token tooling to mint a new token and set the supply to 1. Then it will freeze all minting of that token. This is what will make tokens unique. There can only be one, programmatically. NFTs are just tokens with some associated metadata. The minting account is the first user who mints or publishes the token for the first time on the blockchain. Following that, any accounts involved simply have the NFT transferred to them.

## 2. Program

The system and the program which the user will use to mint an NFT. It is an on-chain program that will
administer the transactions for handling the NFT.

## 3. NFT

The minting account owns the token, which is associated with some metadata. The metadata is stored
in a separate account and is referenced by the token.

## *3.1 Account Owner*

[The program's address that owns the account.](https://docs.solana.com/terminology#account-owner)

## *3.2 Token*

The digital asset that proves digital ownership of the e-Certificate, i.e., the NFT itself.

## *3.3 Metadata*

A set of data containing information about the NFT, including the link to the original copy of the e-Certificate

Once minted, the NFT belongs to the account that minted it. The owner can then transfer it to any Solana account using a simple *transfer transaction*. The sending of the NFT is controlled by the on-chain program(s) included with the system. When the owner decides to transfer the NFT, the token that represents the e-Certificate, the program(s) will transfer the NFT from the owner to the recipient.

To learn more, please refer to Solana’s token program: [https://spl.solana.com/token](https://spl.solana.com/token)
