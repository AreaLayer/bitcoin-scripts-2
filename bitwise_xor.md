# Bitwise XOR


## Bitwise XOR of Byte-sized Words

Bitwise XOR of byte-sized words. Once the 512 elements of the reusable lookup table are on the stack, a bitwise XOR of a byte costs only 26 instructions.


```sh
btcdeb "[ 

# Lookup table for 9-bit inputs f(x) = x & 0b01010101
85 84 85 84 81 80 81 80 85 84 85 84 81 80 81 80 
69 68 69 68 65 64 65 64 69 68 69 68 65 64 65 64 
85 84 85 84 81 80 81 80 85 84 85 84 81 80 81 80 
69 68 69 68 65 64 65 64 69 68 69 68 65 64 65 64 
21 20 21 20 17 16 17 16 21 20 21 20 17 16 17 16 
05 04 05 04 01 00 01 00 05 04 05 04 01 00 01 00 
21 20 21 20 17 16 17 16 21 20 21 20 17 16 17 16 
05 04 05 04 01 00 01 00 05 04 05 04 01 00 01 00 
85 84 85 84 81 80 81 80 85 84 85 84 81 80 81 80 
69 68 69 68 65 64 65 64 69 68 69 68 65 64 65 64 
85 84 85 84 81 80 81 80 85 84 85 84 81 80 81 80 
69 68 69 68 65 64 65 64 69 68 69 68 65 64 65 64 
21 20 21 20 17 16 17 16 21 20 21 20 17 16 17 16 
05 04 05 04 01 00 01 00 05 04 05 04 01 00 01 00 
21 20 21 20 17 16 17 16 21 20 21 20 17 16 17 16 
05 04 05 04 01 00 01 00 05 04 05 04 01 00 01 00 
85 84 85 84 81 80 81 80 85 84 85 84 81 80 81 80 
69 68 69 68 65 64 65 64 69 68 69 68 65 64 65 64 
85 84 85 84 81 80 81 80 85 84 85 84 81 80 81 80 
69 68 69 68 65 64 65 64 69 68 69 68 65 64 65 64 
21 20 21 20 17 16 17 16 21 20 21 20 17 16 17 16 
05 04 05 04 01 00 01 00 05 04 05 04 01 00 01 00 
21 20 21 20 17 16 17 16 21 20 21 20 17 16 17 16 
05 04 05 04 01 00 01 00 05 04 05 04 01 00 01 00 
85 84 85 84 81 80 81 80 85 84 85 84 81 80 81 80 
69 68 69 68 65 64 65 64 69 68 69 68 65 64 65 64 
85 84 85 84 81 80 81 80 85 84 85 84 81 80 81 80 
69 68 69 68 65 64 65 64 69 68 69 68 65 64 65 64 
21 20 21 20 17 16 17 16 21 20 21 20 17 16 17 16 
05 04 05 04 01 00 01 00 05 04 05 04 01 00 01 00 
21 20 21 20 17 16 17 16 21 20 21 20 17 16 17 16 
05 04 05 04 01 00 01 00 05 04 05 04 01 00 01 00




0xAA00    # Input A

DUP
1ADD
PICK
DUP
ROT
SWAP
SUB

0x55      # Input B

DUP
3 ADD
PICK
DUP
ROT
SWAP
SUB

ROT

ADD
DUP
1ADD
PICK
SUB

TOALTSTACK
ADD
PICK

FROMALTSTACK
ADD

# ]"
```

### Python Implementation 

Here's a simplified Python implementation, which demonstrates the idea behind the above implementation:

```python
# Inputs
A = 0b00101010
B = 0b10100100

# Algorithm 
A_even = A & 0b01010101
A_odd = A & 0b10101010
B_even = B & 0b01010101
B_odd = B & 0b10101010
A_andxor_B_even = A_even + B_even
A_andxor_B_odd = A_odd + B_odd
A_xor_B_even = A_andxor_B_even & 0b01010101
A_xor_B_odd = A_andxor_B_odd & 0b10101010
A_xor_B = A_xor_B_odd + A_xor_B_even
print(bin(A_xor_B))
```


### Bitwise XOR with 2 Lookup Tables
We can use two lookup tables, each with 256 elements, one for `f(x) = (x & 0b10101010) >> 1` and one for `g(x) = x & 0b01010101`, to reduce the number of instructions per call down to 24.

```
btcdeb "[ 

# Lookup table for 8-bit inputs f(x) = (x & 0b10101010) >> 1
85 84 85 84 81 80 81 80 85 84 85 84 81 80 81 80 
69 68 69 68 65 64 65 64 69 68 69 68 65 64 65 64 
85 84 85 84 81 80 81 80 85 84 85 84 81 80 81 80 
69 68 69 68 65 64 65 64 69 68 69 68 65 64 65 64 
21 20 21 20 17 16 17 16 21 20 21 20 17 16 17 16 
05 04 05 04 01 00 01 00 05 04 05 04 01 00 01 00 
21 20 21 20 17 16 17 16 21 20 21 20 17 16 17 16
05 04 05 04 01 00 01 00 05 04 05 04 01 00 01 00 
85 84 85 84 81 80 81 80 85 84 85 84 81 80 81 80 
69 68 69 68 65 64 65 64 69 68 69 68 65 64 65 64 
85 84 85 84 81 80 81 80 85 84 85 84 81 80 81 80 
69 68 69 68 65 64 65 64 69 68 69 68 65 64 65 64 
21 20 21 20 17 16 17 16 21 20 21 20 17 16 17 16 
05 04 05 04 01 00 01 00 05 04 05 04 01 00 01 00 
21 20 21 20 17 16 17 16 21 20 21 20 17 16 17 16 
05 04 05 04 01 00 01 00 05 04 05 04 01 00 01 00


# Lookup table for 8-bit inputs g(x) = x & 0b01010101
85 85 84 84 85 85 84 84 81 81 80 80 81 81 80 80 
85 85 84 84 85 85 84 84 81 81 80 80 81 81 80 80 
69 69 68 68 69 69 68 68 65 65 64 64 65 65 64 64 
69 69 68 68 69 69 68 68 65 65 64 64 65 65 64 64 
85 85 84 84 85 85 84 84 81 81 80 80 81 81 80 80 
85 85 84 84 85 85 84 84 81 81 80 80 81 81 80 80 
69 69 68 68 69 69 68 68 65 65 64 64 65 65 64 64 
69 69 68 68 69 69 68 68 65 65 64 64 65 65 64 64 
21 21 20 20 21 21 20 20 17 17 16 16 17 17 16 16 
21 21 20 20 21 21 20 20 17 17 16 16 17 17 16 16 
05 05 04 04 05 05 04 04 01 01 00 00 01 01 00 00 
05 05 04 04 05 05 04 04 01 01 00 00 01 01 00 00 
21 21 20 20 21 21 20 20 17 17 16 16 17 17 16 16 
21 21 20 20 21 21 20 20 17 17 16 16 17 17 16 16 
05 05 04 04 05 05 04 04 01 01 00 00 01 01 00 00 
05 05 04 04 05 05 04 04 01 01 00 00 01 01 00 00



0xAA00    # Input A
0x55      # Input B

2DUP

259 ADD PICK
SWAP
259 ADD PICK

ADD

258 ADD PICK

TOALTSTACK

1ADD PICK
SWAP
1ADD PICK

ADD

PICK

DUP ADD

FROMALTSTACK
ADD

# ]"
```