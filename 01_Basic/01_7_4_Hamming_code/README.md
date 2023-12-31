# [7, 4] Hamming code

# Author

**Dongwhee Kim** 

- [```Google Scholar```](https://scholar.google.com/citations?user=8xzqA8YAAAAJ&hl=ko&oi=ao)
- [```ORCiD```](https://orcid.org/0009-0007-1673-1931?fbclid=PAAabkpwNHesKweJ6F2eGZDnFa2sch2211hf6ZY825YKuli5V7lcN7VIfT0CA)
- [```LinkedIn```](https://www.linkedin.com/in/dongwhee-kim-5753a8290)

# Objective
- Construct (7,4) Hamming SEC (Single-Error Correction) code **[1]** 
- Design the H-Matrix (Parity Check Matrix) with the corresponding correction capability

# Overview
![An Overview of the exercise](https://github.com/xyz123479/ECC-exercise/blob/main/01_Basic/01_7_4_Hamming_code/%5B7%2C%204%5D%20Hamming%20code.png)

# To do
- Construct H-Matrix.txt

# Getting Started
- $ python Hamming_SEC.py

# Answer
- CE_cnt : 1000
- UCE_cnt: 0

If the result differs from the above, please modify the H_Matrix.txt accordingly.

# Hint
- Consider the conditions the H-Matrix must meet for 1-bit error correction.

# Additional Information
- CE: Correctable Error
- UCE: Un-Correctable Error
- In the codeword, an error is indicated by 1.
- Example) codeword: 0010000 => error occurred at the 3rd bit
- In this problem, only a 1-bit error occurs (in 1000 iterations).
- Hence, 100% error correction must be achieved. (If UCE_cnt>0, something is off, so try changing the H-Matrix!)
- ECC essentially begins with the design of the H-Matrix.
- Once you construct the H-Matrix, you can derive the corresponding G-Matrix (Generator Matrix)

# References
- **[1]** Hamming, Richard W. "Error detecting and error correcting codes." The Bell system technical journal 29.2 (1950): 147-160.
