# Linear-Block-Code
# Aim
Write a simple python program to Generate Matrix, Codeword, Hamming weight, Syndrome matrix and find the error on received codeword using Linear block code. 
# Tools required
# Program
```asm
import numpy as np

r = int(input("Enter the number of parity bits (r): "))
k = int(input("Enter the number of message bits (k): "))

print("\nEnter the Parity matrix (P) values row-wise:")
P = np.array([list(map(int,input(f"Row {i+1}: ").split())) for i in range(k)])

G = np.hstack((P, np.eye(k,dtype=int)))
n = G.shape[1]

print("\nGenerator Matrix (G = [P | Iₖ]):")
for i in G: print(*i)

msg = np.array([[ (i>>(k-j-1))&1 for j in range(k)] for i in range(2**k)])
cw = (msg @ G) % 2
w = cw.sum(axis=1)
dmin = w[1:].min()

print("\nMessage Bits\tCodeword\tHamming Weight")
for m,c,wt in zip(msg,cw,w):
    print("".join(map(str,m)),"\t\t","".join(map(str,c)),"\t\t",wt)

print("\nMinimum Hamming Distance (d_min):",dmin)

H = np.hstack((np.eye(n-k,dtype=int),P.T))
Ht = H.T

print("\nParity Check Matrix (H = [I | Pᵗ]):")
for i in H: print(*i)

print("\nTranspose of Parity Check Matrix (Hᵗ):")
for i in Ht: print(*i)

r_code = np.array(list(map(int,input("\nEnter the received (possibly erroneous) codeword: ").split())))

syn = (r_code @ Ht) % 2
print("\nSyndrome:",*syn)

err = np.zeros(n,int)
for i in range(n):
    if (syn == Ht[i]).all():
        err[i] = 1
        print("Error detected at bit position:",i+1)
        break
else:
    print("No error detected.")

print("Corrected Codeword:",*( (r_code+err)%2 ))
```
# Output Waveform

<img width="733" height="810" alt="image" src="https://github.com/user-attachments/assets/bbaa2224-c0ab-4180-b5c6-de8447608985" />
<img width="681" height="127" alt="image" src="https://github.com/user-attachments/assets/669ed4e8-ade0-4cb1-b899-3b8e9e277846" />

# Results

Hence the output for linear block code is verified sucessfully

