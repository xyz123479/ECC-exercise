# [10, 8] RS (Reed-Solomon) code

# Author

**Dongwhee Kim** 
- [```Google Scholar```](https://scholar.google.com/citations?user=8xzqA8YAAAAJ&hl=ko&oi=ao)
- [```ORCiD```](https://orcid.org/0009-0007-1673-1931?fbclid=PAAabkpwNHesKweJ6F2eGZDnFa2sch2211hf6ZY825YKuli5V7lcN7VIfT0CA)
- [```LinkedIn```](https://www.linkedin.com/in/dongwhee-kim-5753a8290)

# Objectives
- Implement a [10, 8] RS code **[1]** over GF(256).
- Understand the decoding method of RS code.

# Overview
![An Overview of the exercise](https://github.com/xyz123479/ECC-exercise/blob/main/01_Basic/07_RS_code_Single_Symbol_Correction/RS%20code%20-%20SSC.png)

# Code flows
- 1. Codeword setting: all zero (no error)
- 2. Error injection (1 symbol error)
- 3. Correct all 1 symbol errors in the codeword (based on 8-bit symbol)
- 4. Update result_type_rs_code (classified into the following 3 types)
>> 1. **NE**: If there is no error when checking the codeword
>> 2. **CE**: If there is an error but it is correctable and the error has been corrected
>> 3. **DUE**: If there is an error but it is not correctable
- 5. I recommend updating the **error_symbol_position** and **Syndrome** variables as well. These could be useful depending on the ECC (Error Correction Code) scheme being used **[2-4]**.

# To do
- Construct **decoding** function in RS_code.cpp
- decoding function input: codeword - unsigned integer array
- decoding function output: result_type_rs_code (NE, CE, DUE)

# Getting Started
- $ g++ RS_code.cpp
- $ ./a.out

# Answer
- CE_cnt: 1,000,000
- DUE_cnt: 0
- SDC_cnt: 0

If the result differs from these numbers, something is wrong. Please analyze the code!

You've coded incorrectly if you can't correct all one million random 1 symbol errors!

The answer code is in the Solution folder.

# Hint
- Correction capability: SSC (Single Symbol Correction)
- result_tyep_rs_code is a pointer (call-by-reference)
- Section 2 of **[5]**

# Additional Information
- CE: Correctable Error
- DUE: Detectable but Uncorrectable Error
- SDC: Silent-Data Corruption
- In the codeword, an error is indicated by 1.
- Example) codeword: 0010000....00 => error occurred at the 3rd bit.
- This exercise only injects 1 symbol error.
- Code configuration is set to match Chipkill-correct of DDR5 **[4]**. Each beat is configured as (32+8), and 2 beats are combined to form (64+16) using 8-bit symbols for RS-code.

# References
- **[1]** Reed, Irving S., and Gustave Solomon. "Polynomial codes over certain finite fields." Journal of the society for industrial and applied mathematics 8.2 (1960): 300-304.
- **[2]** **(AMD Chipkill)** https://www.amd.com/content/dam/amd/en/documents/archived-tech-docs/programmer-references/42301_15h_Mod_00h-0Fh_BKDG.pdf
- **[3]** Song, Yuseok, et al. "SEC-BADAEC: An Efficient ECC With No Vacancy for Strong Memory Protection." IEEE Access 10 (2022): 89769-89780.
- **[4]** Kim, Dongwhee, et al. "Unity ECC: Unified Memory Protection for Bit and Chip Errors." Proceedings of the International Conference for High Performance Computing, Networking, Storage and Analysis. 2023.
- **[5]** Pontarelli, Salvatore, et al. "Low delay single symbol error correction codes based on reed solomon codes." IEEE transactions on computers 64.5 (2014): 1497-1501.

