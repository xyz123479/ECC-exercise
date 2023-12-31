# Fault model & Rank-Level ECC of DDR4

# Author

**Jungrae Kim** 

If you use MRSim code in your work, please use the following citation:

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
</head>
<body>
    <div class="code-container">
        <pre><code id="codeToCopy">@inproceedings{kim2015bamboo,
  title={Bamboo ECC: Strong, safe, and flexible codes for reliable computer memory},
  author={Kim, Jungrae and Sullivan, Michael and Erez, Mattan},
  booktitle={2015 IEEE 21st International Symposium on High Performance Computer Architecture (HPCA)},
  pages={101--112},
  year={2015},
  organization={IEEE}
}
        </code></pre>
    </div>
</body>
</html>

# Objectives
- Implement **Rank-Level ECC (RL-ECC)** of DDR4 ECC-DIMM
- Understand the method of constructing fault model simulation **[2]**
- The above method has been used for several papers **[3-5]**

# Prerequisite
- You need to read the Fault-sim (TACO'15) paper **[2]**
- This exercise applied the **Fault-masking** method from this paper, and the code is written in a simpler manner

# Overview
![An Overview of the exercise](https://github.com/xyz123479/ECC-exercise/blob/main/02_Application/01_MRSim_SECDED/MRSim-Fault%20model.png)

# Code flows (main.cc, Tester.cc)
- 1. **(Start loop)** Calculate the future time point when the fault is expected to occur by inputting the FIT value [6] into the Poisson function. (Tester.cc -> TesterSystem::advance)
- 2. If the expected time point is after the interval used for reliability measurement, we bypass the fault-masking and return to step '1'.
- 3. For instance, in this exercise, the reliability measurement period is 5 years. If the fault occurs 10 years later, we skip the fault-masking process.
- 4. Scrub the soft error.
- 5. If the time point falls within the reliability measurement interval, we apply fault-masking and record that time. (Tester.cc -> hr += advance(dg->getFaultRate());)
- 6. Generate a fault.
- 7. **(Fill in the code)** Based on the fault, generate an error and proceed with decoding using **Rank-Level ECC**.
- 8. **(End loop)** Record the results of Retire, DUE, and SDC, and go back to step 1 to repeat a certain number of times.
- 9. This exercise is repeated 10,000 times. For actual experiments, it is recommended to do it more than 1,000,000 times to ensure reliability.

# DIMM configuration (main.cc, Config.hh)
- DDR4 ECC-DIMM
- Num of rank: 2
- Beat length: 72 bit
- Burst length: 8
- Num of data chips: 16
- Num of parity chips: 2
- Chip capacity: 8Gb
- Num of DQ: 4 (x4 chip)

# ECC configuration
- RL-ECC: (72,64) Hsiao SEC-DED code **[1]**

# Operational faults model configuration (FaultRateInfo.hh)
- # DDR2 Jaguar system [6]
![DRAM fault types and rates (FIT/device) used in the exercise](https://github.com/xyz123479/ECC-exercise/blob/main/02_Application/01_MRSim_SECDED/MRSim-Fault%20model%20Table.png)
- # Future works (operational faults) DDR3 [7], DDR4 [8]
- # Future works (inherent faults) [9]
- # Future works (DRAM error log (released by Alibaba)) [10]
- # Future works (MICRO'23 DRAM fault model paper) [11]

# To do
- Fill in the **hsiao.cc**
- You just need to fill in 2 parts labeled "Fill your code here!!"
- Generate H-matrix.
- Determine whether there is an error ((H * cT) = 0 judgment).
- If there's an error, decode it.

# Getting Started
- $ make clean
- $ make
- $ python run.py

# Answer (010.4x18.SECDED.0.S)
After 100 runs

Retire

0.00000000000

0.00000000000

0.00000000000

0.00000000000

0.00000000000

DUE

0.01000000000

0.02000000000

0.02000000000

0.04000000000

0.05000000000

SDC

0.00000000000

0.00000000000

0.00000000000

0.00000000000

0.00000000000

After 10000 runs

Retire

0.00000000000

0.00000000000

0.00000000000

0.00000000000

0.00000000000

DUE

0.01230000000

0.02430000000

0.03520000000

0.04880000000

0.06110000000

SDC

0.00010000000

0.00020000000

0.00030000000

0.00050000000

0.00060000000

If the results differ from the above, your code might be wrong.

- Retire, DUE and SDC are the probabilities of Retire, DUE, and SDC occurring when run 100 times and 10000 times, respectively. (0~1)
- The Retire, DUE, SDC probability occurring each year -> **cumulative**.
- Ex)

0.01230000000 -> After 1 year
  
0.02430000000 -> After 2 years

0.03520000000 -> After 3 years

0.04880000000 -> After 4 years

0.06110000000 -> After 5 years

# Hint
- Consider the conditions the H-Matrix must meet for 1-bit error correction and 2-bit error detection **[1]**.

# Additional Information
- NE: no error
- CE: detected and corrected error
- DUE: detected but uncorrected error
- ME: detected but miscorrected error
- UE: undetected error
- SDC (Silent Data Corruption): ME + UE
- Only single chip correction is possible
>> Only SEC (Single Error Correction) within the chip is possible
>> 
>> There are 4 bits per chip
>> 
>> There are no errors if all are 0
>> 
>> Some erroneous values like 2, F, A are deliberately placed in some values
>> 
>> If it's 2 (0010), it's a 1 bit error and correction is possible
>> 
>> If it's A (1010), it's a 2 bit error and correction is not possible

- The index is from the right end, 0, 1, 2 ... (same as Verilog)
- If you output the burst, it's 7, 6, ... 0 from the top, a total of 8 (burst length)


# References
- **[1]** Hsiao, Mu-Yue. "A class of optimal minimum odd-weight-column SEC-DED codes." IBM Journal of Research and Development 14.4 (1970): 395-401.
- **[2]** Nair, Prashant J., David A. Roberts, and Moinuddin K. Qureshi. "Faultsim: A fast, configurable memory-reliability simulator for conventional and 3d-stacked systems." ACM Transactions on Architecture and Code Optimization (TACO) 12.4 (2015): 1-24.
- **[3]** Kim, Jungrae, Michael Sullivan, and Mattan Erez. "Bamboo ECC: Strong, safe, and flexible codes for reliable computer memory." 2015 IEEE 21st International Symposium on High Performance Computer Architecture (HPCA). IEEE, 2015.
- **[4]** Gong, Seong-Lyong, et al. "Duo: Exposing on-chip redundancy to rank-level ecc for high reliability." 2018 IEEE International Symposium on High Performance Computer Architecture (HPCA). IEEE, 2018.
- **[5]** Park, Sangjae, and Jungrae Kim. "On-Die Dynamic Remapping Cache: Strong and Independent Protection Against Intermittent Faults." IEEE Access 10 (2022): 78970-78982.
- **[6]** Sridharan, Vilas, and Dean Liberty. "A study of DRAM failures in the field." SC'12: Proceedings of the International Conference on High Performance Computing, Networking, Storage and Analysis. IEEE, 2012.
- **[7]** V. Sridharan, N. De Bardeleben, S. Blanchard, K. B. Ferreira, J. Stearley, J. Shalf, and S. Gurumurthi, ‘‘Memory errors in modern systems: The good, the bad, and the Ugly,’’ ACM Sigplan Notices, vol. 50, no. 4, pp. 297–310, Mar. 2015.
- **[8]** Beigi, Majed Valad, et al. "A Systematic Study of DDR4 DRAM Faults in the Field." 2023 IEEE International Symposium on High-Performance Computer Architecture (HPCA). IEEE, 2023.
- **[9]** Gong, Seong-Lyong, Jungrae Kim, and Mattan Erez. "DRAM scaling error evaluation model using various retention time." 2017 47th Annual IEEE/IFIP International Conference on Dependable Systems and Networks Workshops (DSN-W). IEEE, 2017.
- **[10]** Cheng, Zhinan, et al. "An in-depth correlative study between DRAM errors and server failures in production data centers." 2022 41st International Symposium on Reliable Distributed Systems (SRDS). IEEE, 2022.
- **[11]** Jung, Jeageun, and Mattan Erez. "Predicting Future-System Reliability with a Component-Level DRAM Fault Model." 2023 56th IEEE/ACM International Symposium on Microarchitecture (MICRO). IEEE, 2023.
