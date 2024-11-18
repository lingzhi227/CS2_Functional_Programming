# Functional Programming on Cerebras CS2: Mapping HPC Algorithms

## Overview
This README outlines an approach to map a large group of High-Performance Computing (HPC) algorithms onto the Cerebras CS2 system using a functional programming style. It leverages Cerebras System Language (CSL) as a backend machine language to control and program the vast number of Processing Elements (PEs) in CS2. The goal is to simplify functional dataflow programming, enabling efficient execution on CS2's dataflow architecture with 750,000 PEs.

## Contents
1. Introduction to Functional Dataflow Programming
2. The Cerebras CS2 System Overview
3. Stream Processing and Functional Programming Context
4. Simplified Functional Dataflow Programming on CS2
5. Mapping HPC Algorithms onto CS2
6. Example Workflow
7. Future Directions

## 1. Introduction to Functional Dataflow Programming
Functional programming emphasizes immutability and the composition of functions. In a dataflow architecture like CS2, this paradigm is a natural fit for managing large-scale parallelism without manual orchestration of data movement.

Functional programming paradigms such as:
- **Lucid (1985)**, a dataflow language
- **Feldspar (2010)**, a domain-specific language for dataflow architectures

serve as inspirations to express computational logic that can easily map to PEs without requiring explicit control over the distribution of data or low-level communication.

## 2. The Cerebras CS2 System Overview
The Cerebras CS2 is a wafer-scale engine with 750,000 PEs. It enables:
- **Massive parallelism** with a focus on data movement rather than control flow.
- **CSL (Cerebras System Language)**, which allows low-level programming of the CS2 system, tailored to control PE behavior and facilitate communication between them.

## 3. Stream Processing and Functional Programming Context
To effectively utilize the CS2, we'll build upon existing ideas from **stream processing** and **functional programming**:
- **StreamIt (2010)** and **RaftLib (2015)**: concepts of stream processing help organize continuous data streams processed by different operators. On CS2, such streams could represent data chunks processed by specific PEs.
- **Functional Languages**: Inspired by the theoretical models of functional programming, CS2 lends itself well to simplified functional dataflow programming by allowing functions to be mapped onto PEs, each operating on specific pieces of data.

## 4. Simplified Functional Dataflow Programming on CS2
### Key Concepts
- **Dataflow Graph Representation**: Model HPC algorithms as directed acyclic graphs (DAGs) where nodes represent functions and edges represent data dependencies.
- **PE Mapping**: Assign each functional block (or "node") to one or more PEs, ensuring that data dependencies are fulfilled through CSL.
- **Immutability**: Intermediate results are stored as streams between PEs, following the principles of functional programming where data remains immutable.

### Programming Methodology
1. **Define Functional Blocks**: Break down the HPC algorithm into smaller functional units that operate on discrete data streams.
2. **Map to PE Network**: Use CSL to assign functional units to PEs.
3. **Dataflow Control**: Express data communication requirements between PEs as data streams.

## 5. Mapping HPC Algorithms onto CS2
### Example HPC Algorithms
1. **Matrix Multiplication**: Use a systolic array-like mapping, where each PE performs partial computations and forwards results to neighboring PEs.
2. **FFT (Fast Fourier Transform)**: Represent FFT as a dataflow graph with stages mapped to PEs, utilizing CSL to express the communication patterns across stages.
3. **Convolution**: Each PE handles a different filter position, streaming data between PEs as needed.

### Process
1. **Decompose Algorithm**: Express the algorithm in terms of functional blocks (e.g., multiplication, addition).
2. **Establish Dataflow Relationships**: Define how data flows between blocks (streams) and assign corresponding PEs.
3. **Utilize CSL for Mapping**: Write CSL code to instantiate functional blocks and assign PEs to process the dataflow accordingly.

## 6. Example Workflow
### Step 1: Define Functional Blocks
Using a simplified pseudocode:
```haskell
multiplyBlock :: Data -> Data -> Data
multiplyBlock x y = x * y

addBlock :: Data -> Data -> Data
addBlock x y = x + y
```
### Step 2: Use CSL to Define Mapping
Using CSL, assign these functional blocks to PEs:
```csl
// Define PE assignment
task fmac_task(wlet_data: f16, idx: u16) {
  // Perform multiplication and accumulate results
}

// Assign blocks to PEs in a grid pattern
assign_pe(multiplyBlock, grid_position);
assign_pe(addBlock, grid_position);
```
### Step 3: Dataflow Management
Use CSL to define dataflow between PEs:
```csl
stream_data(source_pe, dest_pe, data);
```

## 7. Future Directions
1. **Higher-Level Abstractions**: Develop a higher-level language or library that can facilitate functional dataflow programming for CS2 without requiring direct CSL coding.
2. **Libraries for Common Patterns**: Implement reusable libraries for common functional patterns like map-reduce or pipeline processing, optimized for CS2.
3. **Performance Optimization**: Investigate optimal data partitioning strategies to maximize PE utilization and minimize data transfer delays.

## Conclusion
By utilizing a functional programming style and CSL as a backend language, we can effectively map large groups of HPC algorithms onto the Cerebras CS2. Functional dataflow programming offers a simplified yet powerful approach to managing the massive parallelism and data movement inherent in the CS2 architecture, enabling more accessible and maintainable HPC applications.