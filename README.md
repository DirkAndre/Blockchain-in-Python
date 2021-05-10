# Blockchain-in-Python
Built-up a functional Blockchain with Python code

Main Blockchain functunality built-up inside Python
documentation as good as known ... done by #Dirk Wollschlaeger

use two classes "Block" and "Blockchain"
Variables
following variables used inside the use-case:
1. prev_hash = each block has as result as PoW one hash result, used for next block calculation
2. nonce = used digit counter until hash function fulfilled requested difficulty
3. data = data content inside the requested new block (could be all value as string)
4. current_hash = new calculated hash
5. difficulty = the requested number of "0" at the beginning of the hash result
6. found = used as boaland, fulfilled calculated hash the difficulty (yes/no)
7. block = as array and storages for each block 1,2,3,4, ... 
8. chain = storred Blockchain
9. genesis = start of the blockchain to get the first block initiated
Classes:
i) Block = all about to get a new block mind
ii)Blockchain = put the Blockchain alive and usable as database
Function def:
 a) init = initialisation  
 b) repr = help function for during setup the blockchain to check how the hases Block 0 and Block 1 look like, used space {} brackets to built-up a dynamic return 
 c) block_string = put the data together wish as to be used for hash calc
 d) hash1 = used lib function hashlib and calculation out of block_string the right SHA256 hash
 e) mine = mining process, used difficulty
 f) add = add put the new block to the array
 g) last_block = is check, are there a first block, if yes
 h) build_block = initiate class Block to build a new block
import
hashlib to use Hash function SHA256

import hashlib, datetime

class Block:
    prev_hash = None
    nonce = -1
    data = None
    timestamp = None

    current_hash = None

    def __init__ (self, prev_hash, data, timestamp):
        self.prev_hash = prev_hash
        self.data = data
        self.timestamp = timestamp

    def __repr__ (self):
        return "Block {0} (prev. {1})".format (
        self.current_hash,
        self.prev_hash
        )

    @property
    def block_string(self):
        return "{0}-{1}-{2}-{3}".format(
        self.prev_hash,
        self.data,
        self.timestamp,
        self.nonce
        )
    
    def hash1 (self):
        return hashlib.sha256(bytes(self.block_string, "utf-8")).hexdigest()
    
    def mine (self, difficulty):
        found = False
        while not found:
            self.nonce +=1
            self.current_hash = self.hash1()
            if self.current_hash[0:difficulty]== "0" * difficulty:
                found = True

                
class Blockchain:
    blocks = []
    timestamp_counter = []
    difficulty = 5
    
    def add(self, block):
        block.mine(self.difficulty)
        self.blocks.append(block)
        

    @property
    def last_block(self):
        return self.blocks[-1] if len(self.blocks) else None 
    
    def build_block(self, data, timestamp):
        if self.last_block:
            block = Block(self.last_block.current_hash, data, timestamp)
            self.add(block)
        else:
            raise Exception ("Missing first Block")
            

chain = Blockchain()
i = 0
timestamp = str(datetime.datetime.now())
timestamp_counter = f"Block:{i} " + timestamp
genesis = Block(None, "GENESIS", timestamp)
chain.add(genesis)

amount_blocks = input("How much Blocks you want to add ? = " "")

while i < int(amount_blocks):
    
    chain.build_block(input(f"What are the Block data you want to add for the {i+1} Block? = " ""), timestamp=datetime.datetime.now())
    timestamp = str(datetime.datetime.now())
    timestamp_counter = timestamp_counter + f", Block:{i+1} " + timestamp
    i  += 1
    
for b in chain.blocks:
    print(b)
     
print("Last block nonce is ", chain.last_block.nonce)
print("timestamps for all blocks are ", timestamp_counter)

