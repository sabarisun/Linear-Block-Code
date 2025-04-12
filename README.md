# Linear-Block-Code

## Aim
The aim of linear block coding is to detect and correct errors during digital data transmission by adding redundant bits to the original data in a structured way, using linear algebra over binary fields.
## Apparatus Required 
Python: A versatile programming language used for scientific computing and signal processing. NumPy: A powerful numerical library in Python for performing array-based operations and mathematical computations. Matplotlib: A plotting library for generating high-quality graphs and visualizations of data, essentialfor demonstrating the sampling process.
## Program
```
LBC
import numpy as np

pb = [] # Parity matrix
Ik = [] # I_K Matrix
p = []
m = []
h = []
h_dis = []
r_code = []
err = []
col = int(input("Enter the Parity bits : "))
row = int(input("Enter the Message bits : "))

# Generator matrix
for i in range (row):
    p = list(map(int, input(f"Enter the row values : {i+1} (Separated by space) : ").split()))  
    pb.append(p)
p_mat = np.array(pb, dtype=int)
Ik=np.eye(row, dtype=int) # Diagonal Matrix
g_mat = np.hstack((p_mat,Ik)) # Generator Matris

# Codeword length and parity bit length
n, k = g_mat.T.shape

# Possible Message Bits
m = np.array([[1 if (i >> (k - j - 1)) & 1 else 0 for j in range(k)] for i in range(2**k)])

# Codewords and Hamming weights
c = np.mod(np.dot(m, g_mat), 2)

for i, row in enumerate(c):
    h_dis1 = np.sum(row)  # Count number of 1's in the row
    h_dis.append(h_dis1)
h_mat = np.array(h_dis).reshape(1,-1)
#h_mat = np.hstack(h_mat)
d_min = np.min(np.sum(c[1:], axis=1))

# H matrix (Parity-check matrix)
h = p_mat[:, :3]
hp = np.hstack((np.eye(n-k, dtype=int), h.T))
ht = hp.T
zero_row = np.zeros((1, ht.shape[1]), dtype=int)  # Create a row of zeros
hp1 = np.vstack((zero_row, ht))

print('**********')
print('The Generator Matrix is: ')
#for r in p_mat: 
#    print(" ".join(map(str, r)))
#for r in Ik: 
#    print(" ".join(map(str, r)))
for r in g_mat: 
    print(" ".join(map(str, r)))

print('**********')
print(f'Message Bits  Codeword   Hamming Weight')
code_word = np.hstack((m, c, h_mat.T))
for r in range(code_word.shape[0]):
    format_row = " ".join(map(str, code_word[r, :k])) + '\t' + " ".join(map(str, code_word[r, k:n+k])) + '\t' + str(code_word[r, -1])
    print(format_row)

print('**********')
print(f'Minimum Hamming distance : {d_min}')

# Parity Check matrix
print('**********')
print(f'Parity Check Matrix')
for r in hp:
    print(" ".join(map(str, r)))
print('**********')
print(f'Parity Check Matrix Transpose')
for r in hp1:
    print(" ".join(map(str, r)))
#Receive codeword
rc = list(map(int, input(f"Enter the error codeword : ").split()))  
r_code.append(rc)
r_c = np.array(r_code)
#Syndrome Calculation
e = np.mod(np.dot(r_c, ht), 2)
#print('**********')
#print(f'Received codeword Matrix')
#for r in r_c:
#    print(" ".join(map(str, r)))
# Find the Error position
for i in range(n):
    if np.array_equal(e[0], ht[i, :]):
        err = np.eye(n, dtype=int)[i,:]
print(f"The error postion is : " + " ".join(map(str, err)))
n1 = hp1.shape[0]
print(f"The size is : {n1}")
print('**********')
print(f'Syndrome Matrix')
for i in range(n1):
    combined_row = np.concatenate((hp1[i, :], np.eye(n1, dtype=int)[i,:]))
    formatted_row = " ".join(map(str, combined_row[:3])) + '\t' + " ".join(map(str, combined_row[k:]))
    print(f'{formatted_row}')
print('**********')
print(f"Syndeome of given received codeword is : " + " ".join(map(str, e[0])))

# Correct the error in the received codeword
add = err + rc
add = np.array(add)
add1 = add % 2
print(f"The correct codeword is : " + " " .join(map(str,add1)))
```
## Output
![linear block op1](https://github.com/user-attachments/assets/a3092aa7-ce6c-428c-ab7f-08b236284bcc)
![linear block op2](https://github.com/user-attachments/assets/ea581a95-5241-4ccd-a5fa-5eb7c1f8157b)

## Calculation
![WhatsApp Image 2025-04-12 at 15 53 18_df1561aa](https://github.com/user-attachments/assets/f6c55e52-81ac-4e6e-abed-bf9ccbeaaf4f)
![WhatsApp Image 2025-04-12 at 15 51 24_67b9ab4b](https://github.com/user-attachments/assets/6443dc22-6752-43b6-a9e2-a45b367f517f)
![WhatsApp Image 2025-04-12 at 15 51 24_dfd490c7](https://github.com/user-attachments/assets/a2f36175-81ba-4add-985f-d81aad7ee8ce)

## Result
Using linear block codes, errors in transmitted data can be efficiently detected and corrected, improving the reliability of communication systems without significantly increasing the data size.
