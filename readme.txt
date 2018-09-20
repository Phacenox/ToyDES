This program is a toy D.E.S. (Data Encryption Standard) encryption and decryption algorithm.
Ehren Schindelar
RIN 661677600

This can be run as a python script with the following command line inputs
./hw1.py <encrypt/decrypt> <input filename> <output filename> <10 bit key (e.g. 1010010010)>
please include the file extensions

It reads from data in sets of 8 bits, and performs various operations
on them to create a new set of 8 bits, which it writes to a file.

The 10 bit key provided to the algorithm is converted into two distinct
8 bit keys which are used during separate rounds of the DES. This is done
using permutations and bit shifts. The 10 bit key is split into two
5 bit blocks using a permutation. Both blocks are shifted left by one,
and combined into an 8 bit key using a 10 to 8 bit permutation. To generate
another key, the 5 bit blocks are shifted left again, and put through the
same 8 to 10 bit permutation.

The aforementioned  rounds occur after a permutation splits the input data
into two seperate 4 bit blocks. During a round the "right" block is put
through a series of operations, and then xored with the "left" block. The
result is used as the "right" block in the next round, and the original
"right" block is used as the new "left" block. But, after the final round,
the result is used as the "left" block, and the original "right" block
is used as the new "right" block.

For the series of operations, the "right" block goes through a series of
permutations and table lookups. The 4 bit block is expanded to 8 bits using
a permutation. Then, it is xored with the 8 bit key corresponding to that
round. Those 8 bits are split in two again, and each 4 bit group is used to
look up a new 2 bit replacement using two unique predetermined 4x4 substitution
matrices. To choose an output from the table, we use two bits from the 4 bit
group asthe column, and the other two bits as the row. Since the tables contain
two bit data, we combine the results from both tables into a 4 bit block and
perform a 4 bit permutation on that data. After this, the "right" block has
finished its encryption, and the round is almost complete.

After the final round, the resulting "left" and "right" blocks are combined
and we perform a final permutation that is the inverse of the initial
permutation. When this is finished, we write this 8 bit string to our output
file.

To decrypt an encrypted file, we simply put the cypher text through our encryption
algorithm again using the same 10 bit key, but use the derived 8 bit keys in
reverse order.

In total, this algorithm performs 9 permutations, four left-shifts, four
xors, and four table-lookups. Since it performs a set number of operations
for each 8 bits in a file, this algorithm runs in O(n) time, with n being
the size of the input file.