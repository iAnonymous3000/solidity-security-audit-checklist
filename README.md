# Solidity Security Audit Checklist

This repository provides a comprehensive checklist and a set of tools to help you audit the security of Solidity smart contracts. It aims to assist developers, auditors, and security researchers in identifying common vulnerabilities and adhering to best practices when reviewing smart contract code.

The checklist covers various aspects of smart contract security, including:

- **Reentrancy vulnerabilities**
- **Arithmetic issues (Overflow/Underflow)**
- **Unchecked external calls**
- **Access control issues**
- **Input validation and sanitization**
- **Randomness and timestamp dependence**
- **Denial of Service (DoS) attacks**
- **Front-running and Miner Extractable Value (MEV)**
- **Flash loan attacks**
- **Cross-chain and interoperability vulnerabilities**
- **NFT security considerations**
- **Gas optimization**

In addition to the checklist, the repository includes:

- **Code snippets demonstrating vulnerabilities and their fixes**
- **Recommendations for automated security analysis tools**
- **Step-by-step usage instructions**
- **Real-world case studies and lessons learned**
- **Community engagement and discussion platform**
- **Additional resources for smart contract security**

Whether you're a developer looking to ensure the security of your smart contracts or an auditor conducting a thorough security review, this repository provides a structured and comprehensive approach to identifying and mitigating potential vulnerabilities.

Join our community, contribute your own findings and experiences, and together, let's enhance the security of the Solidity smart contract ecosystem!

## Table of Contents

- [Security Audit Checklist](#security-audit-checklist)
  - [Progress Tracker](#progress-tracker)
  - [Reentrancy Vulnerabilities](#reentrancy-vulnerabilities)
  - [Arithmetic Issues](#arithmetic-issues)
  - [Unchecked External Calls](#unchecked-external-calls)
  - [Access Control](#access-control)
  - [Input Validation and Sanitization](#input-validation-and-sanitization)
  - [Randomness](#randomness)
  - [Timestamp Dependence](#timestamp-dependence)
  - [Denial of Service (DoS) Attacks](#denial-of-service-dos-attacks)
  - [Front-running and MEV](#front-running-and-mev)
  - [Flash Loan Attacks](#flash-loan-attacks)
  - [Cross-Chain and Interoperability Vulnerabilities](#cross-chain-and-interoperability-vulnerabilities)
  - [NFT Security Considerations](#nft-security-considerations)
  - [Gas Optimization](#gas-optimization)
- [Vulnerability Code Snippets](#vulnerability-code-snippets)
- [Automated Security Analysis Tools](#automated-security-analysis-tools)
  - [Static Analysis Tools](#static-analysis-tools)
  - [Linters](#linters)
  - [Fuzzing Tools](#fuzzing-tools)
  - [Symbolic Execution Tools](#symbolic-execution-tools)
  - [Formal Verification Tools](#formal-verification-tools)
  - [Tool Comparison](#tool-comparison)
- [Usage Instructions](#usage-instructions)
  - [Setting Up the Environment](#setting-up-the-environment)
  - [Reviewing the Checklist](#reviewing-the-checklist)
  - [Running Automated Tools](#running-automated-tools)
  - [Manual Code Review](#manual-code-review)
  - [Reporting Findings](#reporting-findings)
- [Case Studies](#case-studies)
- [Community Engagement](#community-engagement)
- [Security Practices Beyond Solidity](#security-practices-beyond-solidity)
- [Additional Resources](#additional-resources)
- [Contributing](#contributing)
  - [Contribution Guidelines](#contribution-guidelines)
  - [Submitting Changes](#submitting-changes)
  - [Regular Updates](#regular-updates)

## Security Audit Checklist

This section provides a comprehensive checklist categorized by vulnerability types, explaining the importance of each check and the associated risks.

### Progress Tracker

- [ ] Reentrancy Vulnerabilities
- [ ] Arithmetic Issues
- [ ] Unchecked External Calls
- [ ] Access Control
- [ ] Input Validation and Sanitization
- [ ] Randomness
- [ ] Timestamp Dependence
- [ ] Denial of Service (DoS) Attacks
- [ ] Front-running and MEV
- [ ] Flash Loan Attacks
- [ ] Cross-Chain and Interoperability Vulnerabilities
- [ ] NFT Security Considerations
- [ ] Gas Optimization

### Reentrancy Vulnerabilities

Reentrancy vulnerabilities occur when an external contract can call back into the calling contract before the first invocation is completed, potentially manipulating state variables and causing unexpected behavior.

- [ ] Identify all external calls, especially those transferring Ether
- [ ] Ensure state changes happen before external calls (Checks-Effects-Interactions pattern)
- [ ] Use reentrancy guards (`ReentrancyGuard` from OpenZeppelin)
- [ ] Limit gas forwarded in external calls to prevent reentrancy

### Arithmetic Issues

As of Solidity 0.8.0, integer overflows and underflows throw exceptions by default. However, careful consideration is still necessary when dealing with arithmetic operations.

- [ ] Ensure the compiler version is 0.8.0 or above to benefit from built-in overflow checks
- [ ] For earlier versions, use `SafeMath` or similar libraries
- [ ] Validate input values to prevent unexpected arithmetic issues
- [ ] Be cautious with division and modulo operations to avoid division by zero

### Unchecked External Calls

Unchecked external calls can lead to failures if the external contract reverts or runs out of gas. It's important to handle these situations gracefully.

- [ ] Check the return value of external calls
- [ ] Use `try/catch` blocks for external calls in Solidity 0.6.0 and above
- [ ] Handle failures gracefully and avoid assumptions about external contract behavior

### Access Control

Access control vulnerabilities arise when unauthorized users can access sensitive functions or modify critical state variables.

- [ ] Implement proper access control mechanisms for sensitive functions
- [ ] Use modifiers to restrict access based on roles or permissions (e.g., `onlyOwner`, `onlyRole`)
- [ ] Avoid using `tx.origin` for authorization; use `msg.sender` instead
- [ ] Consider using the OpenZeppelin `AccessControl` library for role-based access control

### Input Validation and Sanitization

Lack of proper input validation and sanitization can allow attackers to manipulate contract behavior or exploit vulnerabilities.

- [ ] Validate and sanitize all user inputs
- [ ] Enforce proper constraints on input values (e.g., ranges, formats)
- [ ] Use safe string manipulation techniques
- [ ] Beware of array overflows and underflows

### Randomness

Generating true randomness is challenging in blockchain environments due to the deterministic nature of smart contracts. Improper use of randomness can lead to predictable outcomes and vulnerabilities.

- [ ] Do not rely on block hash, timestamp, or other blockchain data for randomness
- [ ] Use secure and audited random number generation techniques, such as commit-reveal schemes, VRFs (Verifiable Random Functions), or oracles like Chainlink VRF
- [ ] Be cautious of miner manipulation when using on-chain data for randomness

### Timestamp Dependence

Relying on block timestamps for critical logic can be problematic due to the potential manipulation of timestamps by miners.

- [ ] Avoid using block timestamps for critical logic (e.g., time-based conditions)
- [ ] If timestamps are used, be aware of the limitations and potential vulnerabilities
- [ ] Consider using block numbers instead, combined with average block time estimates

### Denial of Service (DoS) Attacks

DoS vulnerabilities occur when a contract's function can be manipulated to consume excessive gas or prevent others from executing certain functions.

- [ ] Avoid unbounded loops that can consume excessive gas
- [ ] Be cautious with external calls that may fail or revert
- [ ] Ensure that critical functions cannot be blocked by a single actor
- [ ] Use pull over push patterns for Ether transfers to prevent DoS in fallback functions

### Front-running and MEV

Front-running involves exploiting knowledge of pending transactions to gain an advantage, often by reordering or inserting transactions.

- [ ] Be aware of functions that can be front-run, such as those involving auctions or trades
- [ ] Implement mechanisms to mitigate front-running, such as commit-reveal schemes
- [ ] Use nonces or other techniques to make transactions unique
- [ ] Consider using Flashbots or similar solutions to protect transactions from being front-run

### Flash Loan Attacks

Flash loans allow borrowing of assets without collateral as long as the loan is repaid within the same transaction. Attackers can exploit flash loans to manipulate markets or exploit vulnerabilities.

- [ ] Analyze contract logic for susceptibility to flash loan attacks
- [ ] Ensure that price oracles are secure and not manipulable within a single transaction
- [ ] Use time-weighted average prices (TWAP) for price feeds
- [ ] Implement checks to prevent manipulation through flash loans

### Cross-Chain and Interoperability Vulnerabilities

Cross-chain bridges and interoperability protocols introduce new attack vectors.

- [ ] Review cross-chain messaging protocols for security
- [ ] Validate and sanitize data received from other chains
- [ ] Be cautious with trust assumptions in cross-chain interactions
- [ ] Ensure atomicity and consistency across chains

### NFT Security Considerations

Non-Fungible Tokens (NFTs) have unique security considerations.

- [ ] Validate token IDs and metadata
- [ ] Ensure compliance with ERC-721 and ERC-1155 standards
- [ ] Prevent unauthorized minting or transfer of tokens
- [ ] Secure royalty and fee mechanisms
- [ ] Handle metadata storage securely to prevent tampering

### Gas Optimization

Inefficient gas usage can lead to high transaction costs and potential out-of-gas exceptions.

- [ ] Optimize gas usage to prevent out-of-gas exceptions
- [ ] Minimize storage reads and writes; use memory variables when possible
- [ ] Use efficient data structures and algorithms
- [ ] Batch operations when appropriate
- [ ] Be cautious with fallback functions and receive functions to avoid excessive gas consumption

## Vulnerability Code Snippets

This section provides well-commented code snippets demonstrating vulnerabilities and their fixes. Each code snippet includes a description of the vulnerability, its potential impact, and the recommended mitigation technique.

### Reentrancy Vulnerability Example

```solidity
// Vulnerable contract
pragma solidity ^0.8.0;

contract VulnerableContract {
    mapping(address => uint256) public balances;

    function withdraw() public {
        uint256 amount = balances[msg.sender];
        // External call
        (bool success, ) = msg.sender.call{value: amount}("");
        require(success, "Transfer failed.");
        // State update happens after external call
        balances[msg.sender] = 0;
    }

    receive() external payable {
        balances[msg.sender] += msg.value;
    }
}

// Fixed contract
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/security/ReentrancyGuard.sol";

contract FixedContract is ReentrancyGuard {
    mapping(address => uint256) public balances;

    function withdraw() public nonReentrant {
        uint256 amount = balances[msg.sender];
        require(amount > 0, "No balance to withdraw.");
        // State update happens before external call
        balances[msg.sender] = 0;
        // External call
        (bool success, ) = msg.sender.call{value: amount}("");
        require(success, "Transfer failed.");
    }

    receive() external payable {
        balances[msg.sender] += msg.value;
    }
}
```

In the vulnerable contract, the external call is made before updating the balance, allowing for reentrancy. The fixed contract uses the `nonReentrant` modifier from OpenZeppelin's `ReentrancyGuard` and updates the balance before making the external call, preventing reentrancy.

### Arithmetic Issues Example

```solidity
// Vulnerable contract (Solidity <0.8.0)
pragma solidity ^0.7.0;

contract VulnerableContract {
    uint256 public totalSupply;

    function mint(uint256 amount) public {
        // Potential overflow if amount is large
        totalSupply += amount;
    }
}

// Fixed contract (Using SafeMath)
pragma solidity ^0.7.0;

import "@openzeppelin/contracts/math/SafeMath.sol";

contract FixedContract {
    using SafeMath for uint256;
    uint256 public totalSupply;

    function mint(uint256 amount) public {
        totalSupply = totalSupply.add(amount); // SafeMath checks for overflow
    }
}

// Fixed contract (Solidity >=0.8.0)
pragma solidity ^0.8.0;

contract SafeContract {
    uint256 public totalSupply;

    function mint(uint256 amount) public {
        totalSupply += amount; // Overflow checks are automatic in Solidity 0.8.0+
    }
}
```

The vulnerable contract is susceptible to integer overflow if compiled with Solidity versions below 0.8.0. The fixed contracts use `SafeMath` for versions below 0.8.0 and rely on built-in overflow checks in Solidity 0.8.0 and above.

## Automated Security Analysis Tools

This section lists and describes various automated tools that can aid in security analysis, including their setup instructions, expected output, and result interpretation guidelines.

### Static Analysis Tools

- **[Slither](https://github.com/crytic/slither)**: A Solidity static analysis framework that detects vulnerabilities, suggests optimizations, and provides a suite of tools for smart contract analysis.
- **[MythX](https://mythx.io/)**: A security analysis platform that integrates with development tools and CI/CD pipelines to detect vulnerabilities.
- **[Securify](https://securify.chainsecurity.com/)**: A security scanner for Ethereum smart contracts that analyzes contracts for vulnerabilities and provides security insights.

### Linters

- **[Solhint](https://github.com/protofire/solhint)**: A linter for Solidity that provides security, style, and best practice rules for smart contract development.
- **[Solium (Ethlint)](https://github.com/duaraghav8/Ethlint)**: A linter for Solidity that identifies security vulnerabilities, coding style issues, and best practice violations.

### Fuzzing Tools

- **[Echidna](https://github.com/crytic/echidna)**: A smart contract fuzzer that generates random transactions to test smart contracts for vulnerabilities and property violations.
- **[Foundry](https://github.com/foundry-rs/foundry)**: A blazing fast, portable, and modular toolkit for Ethereum application development, written in Rust, which includes fuzz testing capabilities.

### Symbolic Execution Tools

- **[Manticore](https://github.com/trailofbits/manticore)**: A symbolic execution tool for analyzing smart contracts and identifying vulnerabilities.
- **[Mythril](https://github.com/ConsenSys/mythril)**: Combines symbolic execution with taint analysis and control flow checking to detect a wide range of vulnerabilities.

### Formal Verification Tools

- **[Certora Prover](https://www.certora.com/)**: A formal verification tool that checks smart contracts against custom specifications.
- **[KEVM](https://github.com/runtimeverification/kevm)**: A formal semantics of the Ethereum Virtual Machine for formal verification of EVM bytecode.
- **[Solidity SMTChecker](https://docs.soliditylang.org/en/latest/smtchecker.html)**: Built into the Solidity compiler, it can detect assertion violations and other issues using formal methods.

### Tool Comparison

- **Quick and Comprehensive Static Analysis**: Use Slither or Securify.
- **In-depth Analysis and Detecting Complex Vulnerabilities**: Use MythX, Mythril, or Manticore.
- **Linting and Style Checking**: Use Solhint or Solium (Ethlint).
- **Testing Edge Cases and Finding Property Violations**: Use Echidna or Foundry.
- **Formal Verification**: Use Certora Prover or KEVM for critical contracts requiring formal proofs.

It's recommended to use a combination of tools for a thorough security audit.

## Usage Instructions

This section provides a step-by-step guide on how to effectively use the provided resources, from setting up the environment to reporting findings.

### Setting Up the Environment

1. **Install Node.js and npm**: [Node.js](https://nodejs.org/) comes with npm, the package manager.
2. **Clone the Repository**: Run `git clone https://github.com/YourUsername/solidity-security-audit-checklist.git` in your terminal.
3. **Install Dependencies**: Navigate to the repository folder and run `npm install` to install any required dependencies.
4. **Set Up Solidity Compiler**: Install [Solidity](https://docs.soliditylang.org/en/latest/installing-solidity.html) compiler for compiling smart contracts.
5. **Install Python (if required)**: Some tools like Slither and Manticore require Python 3.

### Reviewing the Checklist

1. **Familiarize Yourself**: Read through the [Security Audit Checklist](#security-audit-checklist).
2. **Prepare for Review**: Have the smart contract code ready for analysis.
3. **Use the Progress Tracker**: Check off items as you complete each section.

### Running Automated Tools

1. **Choose Tools**: Based on your needs and the [Tool Comparison](#tool-comparison), select appropriate tools.
2. **Install and Set Up Tools**:
   - **Slither**: Install via `pip3 install slither-analyzer`.
   - **Mythril**: Install via `pip3 install mythril`.
   - **Echidna**: Install via [Echidna Installation Guide](https://github.com/crytic/echidna#installation).
3. **Run the Tools**:
   - **Slither**: Run `slither .` in your contract directory.
   - **Mythril**: Run `myth analyze contract.sol`.
   - **Echidna**: Write property-based tests and run `echidna-test ./contract.sol`.
4. **Analyze Results**: Review the outputs and identify any reported issues.

### Manual Code Review

1. **Line-by-Line Review**: Go through the code carefully, keeping the checklist in mind.
2. **Reference Examples**: Use the [Vulnerability Code Snippets](#vulnerability-code-snippets) as guidance.
3. **Document Findings**: Note any vulnerabilities or concerns.

### Reporting Findings

1. **Compile a Report**:
   - **Executive Summary**: Provide an overview of the audit.
   - **Detailed Findings**: Include each vulnerability with severity, description, and remediation advice.
   - **Code References**: Point to specific lines or functions in the code.
2. **Include Supporting Evidence**: Reference code snippets and tool outputs.
3. **Deliver to Stakeholders**: Share the report with developers, project managers, or clients.

## Case Studies

This section includes real-world examples and case studies from past audits to provide practical insights into smart contract vulnerabilities and their consequences.

### The DAO Hack

The DAO hack in 2016 resulted in the theft of approximately 3.6 million Ether. The attacker exploited a reentrancy vulnerability in The DAO's smart contract, allowing them to repeatedly withdraw funds before the contract could update its balance.

**Lessons Learned**:

- Reentrancy vulnerabilities can have severe consequences.
- Proper use of the Checks-Effects-Interactions pattern is crucial.
- Thorough testing and auditing are essential before deploying contracts.

### Parity Multisig Wallet Hack

In 2017, the Parity Multisig Wallet was hacked, leading to the freezing of over 513,774 Ether. The attacker exploited a vulnerability in the wallet's initialization function, allowing them to take ownership of the library contract and render all dependent wallets unusable.

**Lessons Learned**:

- Proper initialization and access control are critical.
- Library contracts should be carefully audited.
- Simplicity and modularity can reduce the attack surface.

### bZx Flash Loan Attack

In 2020, the bZx protocol suffered from a flash loan attack where the attacker manipulated the price of an asset within a single transaction, resulting in a loss of nearly $1 million.

**Lessons Learned**:

- Flash loan attacks can exploit price oracle manipulation.
- Use secure and tamper-resistant price oracles.
- Implement checks to prevent single-transaction manipulation.

### Poly Network Hack

In 2021, the Poly Network was exploited for over $600 million due to a cross-chain interoperability vulnerability, where the attacker manipulated a function responsible for cross-chain transaction verification.

**Lessons Learned**:

- Cross-chain interactions introduce complex security challenges.
- Proper validation and authentication of cross-chain messages are essential.
- Auditing cross-chain logic is critical for preventing such large-scale exploits.

## Community Engagement

This section encourages users to share their audit experiences, discuss challenges they encountered, and how they resolved them. It provides a forum or discussion platform where users can interact and learn from each other. Users are encouraged to contribute their own case studies and lessons learned.

- **Participate in Discussions**: Join our GitHub Discussions page.
- **Share Your Insights**: Contribute case studies, tools, or best practices.
- **Collaborate**: Work with other community members to improve security practices.
- **Stay Updated**: Follow the repository for the latest updates and discussions.

## Security Practices Beyond Solidity

This section provides a holistic view of smart contract security by including resources and best practices for operational security, multi-signature wallets, and smart contract upgradeability patterns.

- **Operational Security Best Practices**:
  - Protect private keys using hardware wallets or secure key management systems.
  - Enforce multi-factor authentication on critical systems.
  - Limit access to sensitive information and systems.
- **Multi-Signature Wallets**:
  - Use wallets like [Gnosis Safe](https://gnosis-safe.io/) for managing assets securely.
  - Require multiple approvals for critical transactions.
- **Smart Contract Upgradeability**:
  - Use patterns like the [Proxy Pattern](https://docs.openzeppelin.com/upgrades-plugins/1.x/proxies).
  - Be cautious of the risks associated with upgradeable contracts, such as increased attack surface.
  - Implement access controls for upgrade functions.
- **Incident Response Planning**:
  - Have a plan in place for responding to security incidents.
  - Monitor contracts and transactions for unusual activity.
  - Engage with security professionals and the community when incidents occur.
- **Formal Verification and Audits**:
  - Engage third-party auditors for comprehensive security reviews.
  - Consider formal verification for critical contracts.

## Additional Resources

This section provides links to external resources, articles, and guides for further learning about smart contract security.

- **[Ethereum Smart Contract Security Best Practices](https://consensys.github.io/smart-contract-best-practices/)**
- **[Solidity Documentation: Security Considerations](https://docs.soliditylang.org/en/latest/security-considerations.html)**
- **[Smart Contract Weakness Classification (SWC) Registry](https://swcregistry.io/)**
- **[OpenZeppelin Contracts](https://docs.openzeppelin.com/contracts/)**
- **[Trail of Bits Blog](https://blog.trailofbits.com/)**
- **[ConsenSys Diligence](https://consensys.net/diligence/)**
- **[Blockchain Security Database](https://consensys.github.io/blockchain-security-db/)**
- **[Secureum Bootcamp](https://secureum.substack.com/)**: Educational content on smart contract security.
- **[Damn Vulnerable DeFi](https://www.damnvulnerabledefi.xyz/)**: A wargame for learning about DeFi vulnerabilities.

## Contributing

We welcome contributions from the community to help improve and expand this repository. If you have suggestions for additional checklist items, code snippets, or tools, please follow the contribution guidelines below.

### Contribution Guidelines

- **Fork the Repository**: Create a personal copy of the repository.
- **Create a Branch**: Use a descriptive name, e.g., `add-new-tool`.
- **Adhere to Style Guidelines**: Follow existing formatting and conventions.
- **Document Your Changes**: Provide clear explanations and, if applicable, code comments.
- **Test Your Changes**: Ensure any code snippets or instructions work as intended.
- **Submit a Pull Request**: Describe your changes and reference any relevant issues.

### Submitting Changes

1. **Fork the Repository**: Click on the "Fork" button at the top right of the repository page.
2. **Clone Your Fork**: `git clone https://github.com/YourUsername/solidity-security-audit-checklist.git`
3. **Create a New Branch**: `git checkout -b my-feature-branch`
4. **Make Changes**: Edit files and add your contributions.
5. **Commit Changes**: `git commit -m 'Add new feature'`
6. **Push to Fork**: `git push origin my-feature-branch`
7. **Open a Pull Request**: Navigate to your fork on GitHub and click "New pull request".

### Regular Updates

This repository is regularly updated to keep up with the latest security best practices and emerging vulnerabilities. Users are encouraged to watch the repository for updates and check back for the latest versions.

---

We hope this repository serves as a comprehensive guide and toolset for auditing the security of your Solidity smart contracts. Remember to stay vigilant, keep your knowledge up-to-date, and always prioritize security in your smart contract development process. Happy auditing!
