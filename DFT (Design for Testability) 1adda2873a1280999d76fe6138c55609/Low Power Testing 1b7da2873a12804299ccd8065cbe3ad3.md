# Low Power Testing

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