# crypto-zombies

Here I have saved every step I have done while creating this project. Done with the help of Loom network

// Step 1 - Declared 2 variables (7,8). We have created dnaModulus so that way we can later use the modulus operator % to shorten an integer to 16 digits.
// Step 2 - Created struct Zombie (10) with 2 properties
// Step 3 - Created an array of struct Zombie
// Step 4 - Create a 'public' function named createZombie. It should take two parameters: _name (a string), and _dna (a uint).
// Step 5 - In createZombie - zombies.push(Zombie(_name, _dna));
// Step 6 - Making createZombie private by adding underscore and declaring it private.
// Step 7 - Created a _generateRandomDna helper function that generates a random DNA number from a string. rand%dnaModulus is now 16 digit
// Step 8 - Created createRandomZombie
// Step 9 - Declared an event to let our front-end know every time a new zombie was created, so the app can display it.
// You're going to need the zombie's id. array.push() returns a uint of the new length of the array - and since the first item in an array has index 0, array.push() - 1 will be the index of the zombie we just added. Store the result of zombies.push() - 1 in a uint called id, so you can use this in the NewZombie event in the next line.
// Step 10 - Declared mapping (19.20)
// Step 11 - Updated _createZombie function so that the address which creates zombies is stored through mapping (24,25)
// Step 12 - Updated createRandomZombie so that a particular address could create only 1 zombie by using 'require' (35)
// zombiefeeding.sol 
// Step 13 - Inheritance => Created ZombieFeeding from ZombieFactory (17)
// Step 14 - Called ZombieFactory.sol in ZombieFeeding.sol by using import function (2)
// Step 15 - Created function feedAndMultiply //function feedAndMultiply(uint _zombieId, uint _targetDna) public { //storage=blockchin , memory=RAM
                                                    //require(msg.sender == zombieToOwner[_zombieId]); //making sure we own this zombie
                                                    //Zombie storage myZombie = zombies[_zombieId];    //getting this zombie's DNA and storing it in new var myZombie
// Step 16 - _targetDna = _targetDna % dnaModulus;             //making sure it is 16 digits
        //   uint newDna = (myZombie.dna + _targetDna) / 2;    //generating newDna
        //   _createZombie("NoName", newDna);                  //creating new hybrid zombie
// Step 17 - Updating _createZombie (private=>internal) so that we can access private functions from zombiefactory.sol to zombiefeeding.sol
// Step 18 - Defining Interface so that we can access other contracts on the blockchain which we don't own (3-16)
// Step 19 - Saving CryptoKitties contract to a var kittyContract (20)
// Step 20 - //function feedOnKitty(uint _zombieId, uint _kittyId) public {   //created feedOnKitty function
               //uint kittyDna;                                               //created kittyDna var which will correspond to genes of CK 
               //(,,,,,,,,,kittyDna) = kittyContract.getKitty(_kittyId);         //which is 10th return val in the CK contract hence 9 commas earlier
               //feedAndMultiply(_zombieId, kittyDna);
// Step 21 - BONUS => Adding feature if a zombie comes from a cat, then set the last two digits of DNA to 99 , for this we add parameter _species in fxn feedAndMultiply (22,27,28,36)
//Advanced Solidity Concepts
// Step 22 - Deleted CK hard-coded address declaration. Created setKittyContractAddress fxn
// Step 23 - Added ownable.sol from OpenZeppelin library. Importing it in zombiefactory.sol and using Inheritance
// Step 24 - Fxn Modifier => "function setKittyContractAddress(address _address) external onlyOwner". Here we have used onlyOwner fxn from owanble.sol so it will get executed first and then setKittyContractAddress will get executed
// Step 25 - Added 2 variables of uint32 in struct Zombie. Using small size uint takes up less storage and hence gas fees will be reduced.
// Step 26 - Defined new var cooldownTime = 1 days and updated _createZombie to add above new var parameters (.push)
// Step 27 - Created 2 new fxns in zombiefeeding.sol => _triggerCooldown & _isReady
// Step 28 - Updated feedAndMultiply fxn to internal to make it more secure and added "require(_isReady(myZombie));" to take ready time into consideration. Called _triggerCooldown(myZombie) so that feeding triggers the zombie's cooldown time.
// Step 29 - Created zombiehelper.sol and inherited it from zombiefeeding.sol. Created a modifier aboveLevel with 2 arguments level and zombieId
// Step 30 - Giving special abilities => able to change name if level is above 2 and change dna if level is above 20. For this we have created 2 fxns changeName and changeDna which will take modifier aboveLevel into consideration.
// Step 31 - View fxns don't cost gas when they're called externally by a user. Created a fxn getZombiesByOwner that will return a user's entire zombie army. It takes only 1 arg address and declaring it 'external view' so that no gas is required to view any user's army.
// Step 32 - Using the memory keyword with arrays to create a new array inside a fxn getZombiesByOwner without needing to write anything to storage. The array will only exist until the end of the function call, and this is a lot cheaper gas-wise than updating an array in storage — free if it's a view function called externally. 
// Step 33 - Dclared a counter and a for loop that will iterate over zombies array and check if zombieToOwner[i] is equal to _owner and incrementing counter . The function will now return all the zombies owned by _owner without spending any gas.
//private means it's only callable from other functions inside the contract; internal is like private but can also be called by contracts that inherit from this one; external can only be called outside the contract; and finally public can be called anywhere, both internally and externally.
//view tells us that by running the function, no data will be saved/changed. pure tells us that not only does the function not save any data to the blockchain, but it also doesn't read any data from the blockchain. Both of these don't cost any gas to call if they're called externally from outside the contract
// Step 34 - Using payable => created var levelUpFee=0.001eth. Created a fxn levelUp which takes _zombieId as a parameter and checks if value sent is equal to levelUpFee and then increases the level of zombie
// Step 35 - Created/Extracted a withdraw fxn from Ownable. Created a setLevelUpFee fxn which takes _fee as parameter and sets levelUpFee = _fee.
// Step 36 - Created zombieattack.sol . Created contract ZombieAttack that inhertits from ZombieHelper
// Step 37 - Creating random number(not at all secure) for our ZombieBattle. Created fxn randMod.
// Step 38 - Created var attackVictoryProbability and setting it to 70. Created fxn attack with 2 parameters _zombieId and _targetId and calling it external
// Step 39 - Making sure we own the zombie which we want to use for attack. For that we created a modifier ownerOf with parameter _zombieId in zombiefeeding.sol . Added ownerOf modifier to feedAndMultiply fxn and removed the require line.
// Step 40 - In zombiehelper.sol make use of ownerOf modifier in fxns changeName and changeDna and removing the require lines from both fxns.
// Step 41 - In zombieattack.sol, add modifier in attack fxn. In fxn definition call myZombie and enemyZombie from Zombie array. Getting a random number with randMod fxn and result = rand.
// Step 42 - In zombiefactory.sol, added new properties winCount and lossCount in our Zombie struct. Added those properties in definition of _createZombie fxn setting those counts to 0.
// Step 43 - In zombieattack.sol, added if statement to compare rand with attackVictoryProbability and incrementing myZombie wincount and level, enemyZombie losscount. Called feedAndMultiply fxn at the end.
// Step 44 - In the else statement we increment myZombie losscount and enemyZombie wincount. Also called _triggerCooldown fxn on myZombie so that it can attack once a day. (_triggerCooldown is already run inside feedAndMultiply. So the zombie's cooldown will be triggered whether he wins or loses.)
// Step 45 - For ERC721 logic created contract ZombieOwnership and imported zombieattack.sol
// Step 46 - Implementing balanceOf and ownerOf from ERC721. Returned no. of zombies in balanceOf using mapping ownerZombieCount. Returned address of owner in ownerOf using mapping zombieToOwner.
// Step 47 - ERROR. We already have a modifier named ownerOf and now ERC721 also has the same. So renamed the modifier to onlyOwnerOf and replaced ownerOf with onlyOwnerOf whereever we have declared modifier.
// Step 48 - Created a _transfer fxn in which zombie count of sender and receiver is decremented and incremented respectively. Changed zombieToOwner mapping for this _tokenId so it now points to _to. Fired the Transfer event from erc721.sol
// Step 49 - Created a mapping zombieApprovals to make sure only the owner or the approved address of a token/zombie can transfer it. Used transferFrom fxn from erc721 in zombieownership.sol. In fxn body, added a require statement to make sure that only the owner or the approved address of a token/zombie can transfer it. Lastly called _transfer fxn.
// Step 50 - Using approve fxn from erc721. It uses modifier onlyOwnerOf with parameter _tokenId. For the body of the function, set zombieApprovals for _tokenId equal to the _approved address. 
// Step 51 - Fired the Approval event where _owner is equal to msg.sender
// Step 52 - Imported OpenZeppelin's SafeMath library safemath.sol for avoiding overflows and underflows. Added declaration "using SafeMath for uint256" in zombiefactory.sol
// Step 53 - Replaced ++,-- with SafeMath operators in zombieownership.sol
// Step 54 - In zombiefactory.sol used SafeMath32 for uint32, used SafeMath16 for uint16
// Step 55 - Using natspec standard for commenting eg. @title, @author, @dev, etc.
// Step 56 - npm install web3. Created index.html and imported web3.min.js
// Step 57 - Added boilerplate code for users in order to require users to have Metamask to use your DApp.
// Step 58 - Web3.js will need 2 things to talk to your contract: its address and its ABI. After deploying the contract we got the ABI that we stored in new file cryptozombies_abi.js, stored in variable called cryptoZombiesABI.
// Step 59 - Imported cryptozombies_abi.js. In index.html, created fxn startApp()
// Step 60 - Web3.js has two methods we will use to call functions on our contract: call(view, pure) and send(functions that aren't view or pure). Created fxns getZombieDetails, zombieToOwner, getZombiesByOwner.