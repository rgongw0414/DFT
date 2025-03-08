# DFT (Design for Testability)

## Why do we need DFT?

```markdown
1. **Verify** the manufatured chips
2. Dimish system **cost**
	*** Cost of repair goes up by an order of magnitude each step away from fab line**
		**a. Fab line (IC test)**:
			* Fab line is where the chips initially manufatured. At this stage, 
			  the defects/faults are cheapest to identify and correct because they are 
			  not packaged yet.
			* Tools like **automatic test equipment (ATE)** and **wafer-level testing (e.g., probe card)** are used here.
		**b. Packaging: 
		  *** Chips are packaged and ready for integration into a system.
		**c. Board-level testing:**
		  * Packaged chips are integrated onto circuit boards. If a defect is discovered, the entire board may have to be discard.
		**d. System-level testing:**  
		  * Boards are integrated into a system or device.
		**e. In the field (End-user stage)**
3. Improve system **reliability** (prevent errors occur)
```

![image.png](imgs/image.png)

- SSF

```markdown
* One signal line (net) in the network of logic gates is fixed to logic 0 or logic 1
	* 2 SSFs (s@1 or s@0) per node
* Short Circuits: A short between a node and power (Vdd) or ground (Gnd)
* Impurities: During fabrication, dust particles can disrupt circuit behavior and result
* Physical Damage:
	* Thermal Stress:
		* Excessive heat can damage transistors or interconnects, causing permanent faults.
	* Mechanical Damage:
		* Handling errors, such as during packaging or mounting on a PCB, can damage the circuit and lead to faults.
```

![image.png](imgs/image%201.png)

![image.png](imgs/image%202.png)

# Logic Simulation

### Hign-impedance & unknown value

[https://www.figma.com/design/kP5fJFrKxJzpsKAo1HYPrU/Untitled?node-id=0-1&t=srQhWGj7N09IJLaM-1](https://www.figma.com/design/kP5fJFrKxJzpsKAo1HYPrU/Untitled?node-id=0-1&t=srQhWGj7N09IJLaM-1)

### Ternary Logic is not accurate

- Whether B is 0 or 1, K will always be 0.
    
    ![image.png](imgs/image%203.png)
    
- Resolution:
    
    ![image.png](imgs/image%204.png)
    

### Input Scannign algorithm

- Using **truth table** takes **huge memory** to determine the output value of logic gates
- Not Efficient for multi-input logic gates

![image.png](imgs/image%205.png)

- By scanning ***controlling values*** and ***unknown values***, efficiently determine the gate output in **logic simulation**
    
    ![image.png](imgs/image%206.png)
    

# Fault Models

- Def: **The methods (models) used to determine whether the target faults exist in the CUT**

![image.png](imgs/image%207.png)

### Defect → Fault → Error → Failure

![image.png](imgs/image%208.png)

![image.png](imgs/image%209.png)

## Why Fault Modeling?

![image.png](imgs/image%2010.png)

## Multiple Stuck-at Faults (MSFs)

- Each node got 3 possible states: sa1, sa0, fault-free
- Single stuck-at fault model can detect most of the MSFs (except for the masked faults)
    - Fault Masking: Fault f2 mask fault f1, if 
    A test for f1 fails to detect f2, in the presense of f2.

![image.png](imgs/image%2011.png)

## Bridging Faults (BFs)

- **Fault extraction**: There’re too many BFs in a circuit, so we need a physical tool to inspect where is more likely to have BFs due to neighbor signals

![image.png](imgs/image%2012.png)

- BF does not consider :
    - Shorts to power or ground
    - Intra-cell (intra-gate) defects, because it is gate-level, not transistor-level fault model

### Wired-OR/-AND Models

![image.png](imgs/image%2013.png)

### SSF Test Sets for BFs

- Not good enough, because **the feedback BFs are not detected**
- We have to apply wire-OR/-AND BF models to detect the feedback BFs

![image.png](imgs/image%2014.png)

![image.png](imgs/image%2015.png)

## **Limitations of Fault Models** (U**nmodeled Faults)**

- Def: While achieving **100% FC for a specific fault model** (e.g., stuck-at faults) ensures all faults of that type are tested, it **does not guarantee the detection of faults or errors outside the scope of that model**.
- **Root Cause**: Faults are actually **ABSTRACTIONs of the defects**, so faults can’t 100% reflect the physical-level defects in digital ciruits
- Real-World Defects Beyond the Model: **Manufacturing defects** or **environmental issues**
    - Process Variation, e.g., timing variation causes timing violations
    - Aging-related issues (electromigration, time-dependent dielectric breakdown).

# Quality of Testing

![image.png](imgs/image%2016.png)

## FC & DL (DPM)

- $Fault\ Coverage=\frac{\#Detected\ Faults} {\#Total\ Faults}$
    - The fraction of faults not detected by the testing process (missed faults by the test set)
- $Defect\ Level = 1 - Y^{1-FC}$ (*Brown & Williams* **Model**)
    - It’s a model for **PREDICTING** the **% of Test Escape**
    - The probability that a defective chip remains undetected and is shipped as a "good" chip

# ATPG

# SCAN CHAIN

## Scan Cells

# BIST

## Compression

## Decompression

## LFST

## MISR

# MBIST

# LBIST

# Path Delay Faults

# Transition Faults

# Low-Power Testing

## Reduce the Number of Transitions

- For the sake of reducing the number of test patterns, the generated test patterns often frequently toggle, e.g., 0000→1111, 0101→1010, etc.
- The patterns generated by ATPG are random, so the correlation b/w them is low; unlike the input of a circuit is often high, .e.g, speech signal.

## Low-Power Test Pattern Generation

- **Description**: Generate test patterns that minimize the simultaneous toggling of signals.
- **Benefits**:
    - Reduces power peaks during testing.
    - Prevents overloading the power delivery network, minimizing IR drops and thermal effects.
- **Techniques**:
    - **Capture Power Reduction**: Modify patterns to reduce simultaneous switching during the capture cycle of scan-based tests.
    - **ATPG Optimization**: Use power-aware Automatic Test Pattern Generation (ATPG) tools to generate patterns that balance fault coverage and power consumption.

## Dynamic Power Management During Testing

- **Description**: Incorporate dynamic power management techniques used during normal operation into the test mode.
- **Implementation**:
    - Enable power gating to selectively turn off unused blocks of the chip during testing.
    - Use clock gating to disable clock signals in inactive portions of the circuit during specific test cycles.
- **Benefits**:
    - Reduces unnecessary power draw in unused parts of the circuit.
    - Controls peak power consumption during testing.

## Partitioned Testing

- **Description**: Test smaller subsections of the chip incrementally, rather than the entire chip at once.
- **Implementation**:
    - Divide the circuit into smaller functional blocks or regions and test them one at a time.
    - Use techniques like hierarchical DFT (Design for Testability) to facilitate partitioned testing.
- **Benefits**:
    - Limits the area under test at any given time, reducing power consumption.
    - Prevents excessive thermal stress and overheating.

## **Scan Chain Design for Power Efficiency**

- **Description**: Optimize the design of scan chains to balance testing efficiency and power usage.
- **Techniques**:
    - **Segmented Scan Chains**: Divide scan chains into smaller segments and test each segment independently, reducing simultaneous toggling.
    - **Clocking Strategies**: Use multiple slower clocks rather than a single high-frequency clock to reduce dynamic power.

## **Test Scheduling Optimization**

- **Description**: Arrange the order of tests to minimize peak power usage while maintaining fault coverage.
- **Approaches**:
    - Alternate between high-power and low-power test patterns to distribute power usage more evenly.
    - Avoid consecutive high-power test patterns that may lead to overheating or excessive IR drop.
- **Tools**:
    - Use power-aware test scheduling algorithms to automate this process.

## **Built-In Self-Test (BIST)**

- **Description**: Use power-efficient on-chip self-testing mechanisms to reduce dependency on external test equipment.
- **Benefits**:
    - Limits power usage by controlling the test environment more effectively.
    - Reduces the complexity of test patterns as the test engine is integrated into the chip.

# References:
* [VLSI Testing by Prof. James Chien-Mo Li, Lab of Dependable Systems, National Taiwan University](https://www.youtube.com/playlist?list=PLvd8d-SyI7hjk_Ci0zpTqImAtpEjdK5JF)