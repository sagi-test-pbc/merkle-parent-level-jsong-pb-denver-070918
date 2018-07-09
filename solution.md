
# Merkle Parent Level

Given an ordered list of more than two hashes, we can calculate an entire list of parents, or what we call the Merkle Parent Level. If we have an even number of hashes, this is straightforward, as we can simply pair them up in order. If we have an odd number of hashes, then we need to do something else as we have a lone hash at the end.

The Merkle Tree solution is to simply duplicate the last item. So, for a list like [A, B, C] what we do is add C again to get [A, B, C, C]. At this point, we can calculate the merkle parent of A and B and calculate the merkle parent of C and C to get:

[H(A||B), H(C||C)]

Note that since the Merkle Parent always consists of two hashes, we end up with exactly half the number of hashes before, rounded up. The rounding up is because an odd number of hashes is expanded to be one more.

#### Exercise: Calculate the next Merkle Parent Level given these hashes
```
8b30c5ba100f6f2e5ad1e2a742e5020491240f8eb514fe97c713c31718ad7ecd
7f4e6f9e224e20fda0ae4c44114237f97cd35aca38d83081c9bfd41feb907800
ade48f2bbb57318cc79f3a8678febaa827599c509dce5940602e54c7733332e7
68b3e2ab8182dfd646f13fdf01c335cf32476482d963f5cd94e934e6b3401069
43e7274e77fbe8e5a42a8fb58f7decdb04d521f319f332d88e6b06f8e6c09e27
4f492e893bf854111c36cb5eff4dccbdd51b576e1cfdc1b84b456cd1c0403ccb
```


```python
from helper import merkle_parent

hex_hashes = [
    '8b30c5ba100f6f2e5ad1e2a742e5020491240f8eb514fe97c713c31718ad7ecd',
    '7f4e6f9e224e20fda0ae4c44114237f97cd35aca38d83081c9bfd41feb907800',
    'ade48f2bbb57318cc79f3a8678febaa827599c509dce5940602e54c7733332e7',
    '68b3e2ab8182dfd646f13fdf01c335cf32476482d963f5cd94e934e6b3401069',
    '43e7274e77fbe8e5a42a8fb58f7decdb04d521f319f332d88e6b06f8e6c09e27',
    '4f492e893bf854111c36cb5eff4dccbdd51b576e1cfdc1b84b456cd1c0403ccb',
]

# bytes.fromhex to get all the hashes in binary
hashes = [bytes.fromhex(h) for h in hex_hashes]
# initialize parent level
parent_level = []
# skip by two: use range(0, len(hashes), 2)
for i in range(0, len(hashes), 2):
    # calculate merkle_parent of i and i+1 hashes
    parent = merkle_parent(hashes[i], hashes[i+1])
    # print the hash's hex
    print(parent.hex())
    # add parent to parent level
    parent_level.append(parent)
```

### Test Driven Example


```python
def merkle_parent_level(hashes):
    '''Takes a list of binary hashes and returns a list that's half
    the length'''
    # if the list has exactly 1 element raise an error
    if len(hashes) == 1:
        raise RuntimeError('Cannot take a parent level with only 1 item')
    # initialize next level
    parent_level = []
    # loop over every pair (use: for i in range(0, len(hashes), 2))
    for i in range(0, len(hashes), 2):
        # get the merkle parent of i and i+1 hashes
        parent = merkle_parent(hashes[i], hashes[i+1])
        # append parent to parent level
        parent_level.append(parent)
    # return parent level
    return parent_level
```
