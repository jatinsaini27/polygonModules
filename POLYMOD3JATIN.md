# Designing a zkSNARK circuit

In this project, we have to create a zkSNARK circuit and deploy the solidity verifier to the Mumbai testnet. After deploying it to the Mumabi testnet we will check it by clicking on https://mumbai.polygonscan.com/address/0xf76b63a1d0a7cefbbca5c91f678ebea3992ec5fd.

## Description
Firstly, we have to import the github files (https://github.com/gmchad/zardkat.git)  into the gitpod. After that we have to run the necessary commands. Then, in circuit.circom we have to write the logic and code for creating the circuit. After that run the command npx hardhat circom and npx hardhat run scripts/deploy.ts to compile the file. After that in hardhat.config.js we have to put the network mumbai so that we can deploy the circuit on it. For the private key we are using the environment variable (env file) for security reasons.
After putting the private key in env file(which can be used by installing dotenv) we can use it in the hardhat.config.js by process.env.MYPRIVATEKEY. After this to deploy on the mumbai network we have to run the commands npx hardhat run scripts/deploy.ts --network mumbai. then it will be deployed to the address and verifier assert that it is true.


## Getting Started

### Executing program

1. Import the Github Files from (https://github.com/gmchad/zardkat.git).
2. In circuit.circom insert the code and logic to design the circuit mentioned in the project. (Mentioned in the code)
3. Run the command npx hardhat circom.
4. Run the command npx hardhat run scripts/deploy.ts
(Output:Downloading compiler 0.6.11
Compiled 1 Solidity file successfully
Verifier deployed to 0x5FbDB2315678afecb367f032d93F642f64180aa3
Verifier result: true)
5. For deploying on the mumbai network put this into the hardhat.config.js
   (networks: {
  mumbai: {
    url: `https://rpc.ankr.com/polygon_mumbai`,
    accounts: [process.env.MYPRIVATEKEY] (For this we have used environment variable env file by installing dotenv)

  }};
  6. After this run the Command npx hardhat run scripts/deploy.ts --network mumbai.
  (Output:
  Verifier deployed to 0xF76B63A1D0A7cefbbca5c91f678EBeA3992EC5fd
Verifier result: true)
   
```
pragma circom 2.0.0;

/*This circuit template checks that c is the multiplication of a and b.*/  

template MyCircuit () { 

    //input signals
    signal input a;
    signal input b;

    //signal from gates
    signal x;
    signal y;

    //output signal
    signal output q;

    //components
    component andGate = AND();
    component orGate = OR();
    component notGate = NOT();

    //logic
    andGate.a <== a;
    andGate.b <== b;
    x <== andGate.out;

    notGate.in <== b;
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
component main = MyCircuit();
```



## Authors

Metacrafter Chris
@metacraftersio


## License

This project is licensed under the MIT License - see the LICENSE.md file for details
