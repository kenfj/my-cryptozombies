# Solidity Dapp Webapp with Web3.js

## Setup Ganache Local Network and MetaMask

* https://qiita.com/kanazwk/items/4b947f170d9ac370c3f3

```bash
# start local network on 127.0.0.1:8545
ganache-cli
# check output and copy paste the first one of Private Keys
# import the account to MetaMask using the private key
```

## Setup Test Contract

```bash
# migrate contract to local network
truffle migrate
# copy paste CryptoZombies contract address to index.html

# create test zombie
truffle console

let cz = await CryptoZombies.deployed()
let res = await cz.createRandomZombie('Foo')
res.receipt.status
true
# see also README_TESTS
```

## Start Web Server

* setup 1.x of Web3.js
* https://cryptozombies.io/en/lesson/6/chapter/1

```bash
cd webapp

# save web3.js
curl -OL https://github.com/ChainSafe/web3.js/raw/1.x/dist/web3.min.js
curl -OL https://github.com/ChainSafe/web3.js/raw/1.x/dist/web3.min.js.map

# save ABI https://ethereum.stackexchange.com/questions/38828
python -c "import json; print('const cryptoZombiesABI = '+json.dumps(json.load(open('../CryptoZombies/build/contracts/CryptoZombies.json'))['abi']))" \
    > cryptozombies_abi.js

# start test server
python -m http.server 8000

# open another terminal or web browser
open http://0.0.0.0:8000/

# you should see test zombie Foo
```

## TODO Quotes from Original Site

So we've left this implementation bare-bones. Here's a checklist of ideas for things we would want to implement in order to make our front-end a full implementation for our zombie game, if you want to run with this and build it on your own:

Implementing functions for attack, changeName, changeDna, and the ERC721 functions transfer, ownerOf, balanceOf, etc. The implementation of these functions would be identical to all the other send transactions we covered.

Implementing an "admin page" where you can execute setKittyContractAddress, setLevelUpFee, and withdraw. Again, there's no special logic on the front-end here — these implementations would be identical to the functions we've already covered. You would just have to make sure you called them from the same Ethereum address that deployed the contract, since they have the onlyOwner modifier.

There are a few different views in the app we would want to implement:

a. An individual zombie page, where you can view info about a specific zombie with a permalink to it. This page would render the zombie's appearance, show its name, its owner (with a link to the user's profile page), its win/loss count, its battle history, etc.

b. A user page, where you could view a user's zombie army with a permalink. You would be able to click on an individual zombie to view its page, and also click on a zombie to attack it if you're logged into MetaMask and have an army.

c. A homepage, which is a variation of the user page that shows the current user's zombie army. (This is the page we started implementing in index.html).

Some method in the UI that allows the user to feed on CryptoKitties. We could have a button by each zombie on the homepage that says "Feed Me", then a text box that prompted the user to enter a kitty's ID (or a URL to that kitty, e.g. https://www.cryptokitties.co/kitty/578397). This would then trigger our function feedOnKitty.

Some method in the UI for the user to attack another user's zombie.

One way to implement this would be when the user was browsing another user's page, there could be a button that said "Attack This Zombie". When the user clicked it, it would pop up a modal that contains the current user's zombie army and prompt them "Which zombie would you like to attack with?"

The user's homepage could also have a button by each of their zombies that said "Attack a Zombie". When they clicked it, it could pop up a modal with a search field where they could type in a zombie's ID to search for it. Or an option that said "Attack Random Zombie", which would search a random number for them.

We would also want to grey out the user's zombies whose cooldown period had not yet passed, so the UI could indicate to the user that they can't yet attack with that zombie, and how long they will have to wait.

The user's homepage would also have options by each zombie to change name, change DNA, and level up (for a fee). Options would be greyed out if the user wasn't yet high enough level.

For new users, we should display a welcome message with a prompt to create the first zombie in their army, which calls createRandomZombie().

We'd probably want to add an Attack event to our smart contract with the user's address as an indexed property, as discussed in the last chapter. This would allow us to build real-time notifications — we could show the user a popup alert when one of their zombies was attacked, so they could view the user/zombie who attacked them and retaliate.

We would probably also want to implement some sort of front-end caching layer so we aren't always slamming Infura with requests for the same data. (Our current implementation of displayZombies calls getZombieDetails for every single zombie every time we refresh the interface — but realistically we only need to call this for the new zombie that's been added to our army).

A real-time chat room so you could trash talk other players as you crush their zombie army? Yes plz.

That's just a start
