# zkSNARK Circom Circuit Verifier [Polygon Advanced Final Project]

This project guides you through coding various circuits in the Circom programming language and deploying a verifier to a testnet like [Mumbai Polygon](https://mumbai.polygonscan.com/). You will also learn how to generate proofs of specific inputs and verify them using the deployed verifier.

## Description

This project includes all the necessary resources to get started with Circom programming for zkSNARK circuits. You'll need a basic understanding of circuit gates, such as AND, OR, and NOT gates, to begin coding circuits. Below is a brief explanation of these gates:

### Basic Logic Gates

1. **AND Gate (andGate)**:  
   An AND gate takes two inputs (A, B) and returns an output (X) based on the following logic:  
   
   | A  | B  | X  |
   |----|----|----|
   | 1  | 0  | 0  |
   | 1  | 1  | 1  |
   | 0  | 0  | 0  |
   | 0  | 1  | 0  |

2. **NOT Gate (notGate)**:  
   A NOT gate takes one input (A) and returns an output (X) based on the following logic:

   | A  | X  |
   |----|----|
   | 1  | 0  |
   | 0  | 1  |

3. **OR Gate (orGate)**:  
   An OR gate takes two inputs (A, B) and returns an output (X) based on the following logic:

   | A  | B  | X  |
   |----|----|----|
   | 1  | 0  | 1  |
   | 1  | 1  | 1  |
   | 0  | 0  | 0  |
   | 0  | 1  | 1  |

These gates form the basis for this project. Here's the corresponding circuit diagram:  
![Circuit Diagram](https://authoring.metacrafters.io/assets/cms/Assessment_b05f6ed658.png?updated_at=2023-02-24T00:00:37.278Z)

### Circuit Templates

We will use pre-made templates from [Circomlib](https://github.com/iden3/circomlib) to create each gate for this circuit.

### Circuit Code Example

```circom
pragma circom 2.0.0;

template Multiplier2 () {  
   // Declare Inputs 
   signal input a;
   signal input b;

   // Declare Signals
   signal x;
   signal y;

   // Declare Output
   signal output q;

   // Gates
   component andGate = AND();
   component notGate = NOT();
   component orGate = OR();

   // Circuit Logic
   andGate.a <== a;
   andGate.b <== b;
   notGate.in <== b;

   x <== andGate.out;
   y <== notGate.out;

   orGate.a <== x;
   orGate.b <== y;

   q <== orGate.out;
}

template AND() {
   signal input a;
   signal input b;
   signal output out;

   out <== a*b;
}

template OR() {
   signal input a;
   signal input b;
   signal output out;

   out <== a + b - a*b;
}

template NOT() {
   signal input in;
   signal output out;

   out <== 1 + in - 2*in;
}

component main = Multiplier2();
```

## Getting Started

### Prerequisites

To start working on this project, you will need:

- **IDE**: Use an offline IDE like [VSCode](https://code.visualstudio.com/download), or an online one like [Remix](https://remix.ethereum.org/) or [Gitpod](https://gitpod.io/).
- **Dependencies**:  
  - Install [Hardhat](https://hardhat.org/)  
  - Install [Node.js](https://nodejs.org/en/download/current)

### Installing

- Clone this repository or use a template like [Zardkat](https://github.com/gmchad/zardkat).
- Install the [dotenv](https://www.npmjs.com/package/dotenv) package to securely manage private keys.
- Set up your testnet details in the `circuits.config.json` file following the [Hardhat Network tutorial](https://hardhat.org/tutorial/deploying-to-a-live-network).
- Obtain test MATIC for free from [Alchemy Faucet](https://www.alchemy.com/faucets/polygon-mumbai).
- Add your `PRIVATE_KEY` in the `.env.example` file and rename it to `.env`.

### Compilation and Deployment

1. Install all NPM dependencies:
   ```bash
   npm i
   ```

2. Compile the circuit:
   ```bash
   npx hardhat circom
   ```
   This generates the circuit intermediaries in the `out` folder and the `MultiplierVerifier.sol` contract.

3. Set input values for AND gate in `input.json`:
   ```json
   {
     "a": "0",
     "b": "1"
   }
   ```

4. Deploy the verifier to the Polygon Mumbai Testnet:
   ```bash
   npx hardhat run scripts/deploy.ts --network mumbai
   ```

### Execution

This script performs the following:

1. Deploys the `MultiplierVerifier.sol` contract to Polygon Mumbai Testnet.
2. Generates a proof with `generateProof()` using input `(0,1)`.
3. Generates calldata with `generateCallData()`.
4. Calls `verifyProof()` on the verifier contract with the generated calldata.

You can check the contract on [Mumbai Polygon Scan](https://mumbai.polygonscan.com/) by pasting your contract ID.

## Directory Structure

### Circuits

```
circuits
├── multiplier
│   ├── circuit.circom
│   ├── input.json
│   └── out
│       ├── circuit.wasm
│       ├── multiplier.r1cs
│       ├── multiplier.vkey
│       └── multiplier.zkey
└── powersOfTau28_hez_final_12.ptau
```

### Contracts

```
contracts
└── MultiplierVerifier.sol
```

## Author

@Helix51

## License

This project is not licensed.
