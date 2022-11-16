# Lightning privacy

## Receiver privacy with [LNproxy](http://lnproxy.org/)
* lnproxy takes a bolt 11 invoice and generates a “wrapped” invoice that can be settled if and only if the original invoice is settled. The “wrapped” invoice has the same payment hash, expiry, and description, as the invoice it wraps but adds a small routing fee to the amount. The “wrapped” invoice can be used anywhere the original invoice would be used to trustlessly hide the destination of a payment.
* use the [API](http://lnproxy.org/doc) in your `.bashrc` (available in the Raspiblitz v1.9.0 and above with a CLI shortcut as well):
  ```
  function lnproxy() {
    echo
    echo "Requesting a wrapped invoice from http://rdq6tvulanl7aqtupmoboyk2z3suzkdwurejwyjyjf4itr3zhxrm2lad.onion ..."
    echo
    torify curl http://rdq6tvulanl7aqtupmoboyk2z3suzkdwurejwyjyjf4itr3zhxrm2lad.onion/api/${1}
  }
  ```
* Code and how to run yourself: <https://github.com/lnproxy>
* Improved alias verifying the payment hash: <https://github.com/lnproxy/lnproxy-cli>
* 
## Unannounced channels - shadow liquidity
* use paralell private channels to hide capacity and UTXOs
* <https://eprint.iacr.org/2021/384.pdf>
  > To hide public channel balances, a node may open
unannounced channels in parallel to announced ones. Depending on the relation
between the balances of announced and unannounced channels, the attacker may
still be able to discover unannounced channel balances (e.g., if the balance of
the unannounced channel exceeds the balances of announced channels). Even in
that case, the standard probing technique needs to be modified

## Open channels with improved privacy
### Manual
* create channels from coinjoin outputs
  * private channels are not announced, use them especially if there is a public channel to the peer already
  * open one channel at a time
  * create no change
  * avoid round amounts
* close cooperatively to an external address (eg a Joinmarket wallet running as a Maker))
* Creating a Core Lightning channel funded by JoinMarket: <https://gist.github.com/BitcoinWukong/0c04d9186251b0a6497fef3737e95ceb>
* syntax for LND with Balance of Satoshis:
  ```
  bos open PUBKEY --amount SATS --coop-close-address EXTERNAL_ADDRESS --type private --external-funding
  ```
### Mutiny wallet
- LDK / Sensei based
- a new node for every channel open
- fund channels with an external wallet and no change
- <https://github.com/BitcoinDevShop/pln>
- <https://mutinywallet.com/>

### LN-vortex
- Coinjoin to channels: <https://github.com/ln-vortex/ln-vortex>
- First mainnet transaction: <https://twitter.com/benthecarman/status/1590886577940889600>

### Nolooking
- Payjoin to channels. The receiver of a paymant can open channels in a payjoin.
  * <https://github.com/chaincase-app/nolooking>
  * <https://chaincase.app/words/lightning-payjoin>

## LN wallets
### Mobile wallets (LN node on the phone)
Tor support and private chain info
* OBW - Tor + Electrum server support
  * optional custodial hosted channels support - payments are private from the provider
* Breez - Tor and neutrino backend
* Blixt - Neutrino block source

### Custodial wallets
Traditional custodians provide no privacy from the maintainer.
* Wallet of Satoshi
* Bitcoin Beach Wallet (phone number required)
* CoinOS (Liquid compatible LN <-> Liquid gateway)

### LN compatible chaumian (blinded) mints
* Cashu in browser - <https://cashu.space> , LNbits extension
* Fedimint with LN gateways - WIP

## LN protocol improvements
- [x] Alias SCIDs <https://github.com/lightning/bolts/pull/910>
- [ ] Route blinding: <https://github.com/lightning/bolts/pull/765>
- [ ] Trampoline routing: <https://github.com/lightning/bolts/pull/836>
- [ ] PTLCs <https://lists.linuxfoundation.org/pipermail/lightning-dev/2021-October/003278.html>

## Attacks
### Channel jamming
* <https://jamming-dev.github.io/book>
* <https://twitter.com/ffstls/status/1559902528808140804>
### LNsploit
* A Lightning Network exploit toolkit: <https://www.nakamoto.codes/BitcoinDevShop/LNsploit>

### Probing
- [hiddenlightningnetwork.com](https://github.com/BitcoinDevShop/hidden-lightning-network) - uses LDK to probe the lightning network to detect private channels 
* More details and conversation: <https://lists.linuxfoundation.org/pipermail/lightning-dev/2022-June/003599.html>
* [CD69: decentralized identifiers (DIDs), “web5,” and lightning privacy with tony](https://citadeldispatch.com/cd69/)

## Reading list
* Lightning privacy: from Zero to Hero <https://github.com/t-bast/lightning-docs/blob/master/lightning-privacy.md>
* Mastering the Lightning Network chapter: <https://github.com/lnbook/lnbook/blob/develop/16_security_privacy_ln.asciidoc>
* Current State of Lightning Network Privacy in 2021 - Tony <https://abytesjourney.com/lightning-privacy>
* BOLT12 proposal <https://bolt12.org>
* Make Lightning Payments Private Again (PLN)
    * <https://bc1984.com/make-lightning-payments-private-again/>
    * <https://bitcoinmagazine.com/technical/pln-makes-bitcoin-lightning-more-private>

### Onion routing
* BOLT #4: Onion Routing Protocol https://github.com/lightning/bolts/blob/master/04-onion-routing.md
https://github.com/ellemouton/onion/blob/master/docs/onionRouting.pdf
* Route Blinding proposal https://github.com/lightning/bolts/blob/route-blinding/proposals/route-blinding.md
https://github.com/ellemouton/onion/blob/master/docs/routeBlinding.pdf
* CLI tool for constructing & peeling onions both with and without route blinding. https://github.com/ellemouton/onion

## Listening and videos
* Privacy on Lightning - Bastien Teinturier - Day 2 DEV Track - AB21 <https://bitcointv.com/w/2pXyaypeMThT5tM3MUWcgN>
* Lightning Privacy - Ficsor, Teinturier, Openoms - Day 2 DEV 2pXyaypeMThT5tM3MUWcgN
<https://bitcointv.com/w/xsj5AEx36Usqts8GuNw9b3>
* [Citadel Dispatch e0.2.1 - the lightning network and bitcoin privacy with @openoms and @cycryptr](https://citadeldispatch.com/cd21/)
* RecklessVR Cryptoanarchy weekend / HCPP20 : how and why to use bitcoin privately <https://www.youtube.com/watch?v=NUlUQlgtWlM>
Slides: <https://keybase.pub/oms/slides/Running_a_Lightning_Node_Privately.pdf>
* [SLP391 BEN CARMAN – BITCOIN PRIVACY, SURVEILLANCE, LN VORTEX, P2P & AUSTIN BITDEVS](https://stephanlivera.com/episode/391/)
* [CD70: using lightning privately with tony and @futurepaul](https://citadeldispatch.com/cd70/)
