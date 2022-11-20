# poolingColateral

poLateral is an open-source liquidity protocol to provide FIL collaterals as a loan for storage providers (SPs). The main object of the protocol is a lending pool in which liquidity providers can supply FIL assets and storage providers can borrow a designated amount for the collateral needs in providing computer storage. The smart contract will lock the future income until the SPs have repaid their loan. 

Motivation: To provide storage space on-chain in Filecoin, Storage providers have to post collateral in FIL to onboard storage capacity to the network and to accept storage deals. This collateral incentivizes the storage provider to behave correctly, by presenting timely proof of the health of the data (PoRep, PoSt) or they are. getting slashed. 

The friction of pledge collateral exists such as insufficient initial capital or high transaction costs throttles SP participation and growth. On the other hand, notice that the Filecoin network has a large base of long-term token holders that would see the network grow, and are willing to lend their FIL to reputable and growth-oriented SPs. 

Design: 
  Objects: SP, Beneficiary, LP, Borrower. note: we set SP as the borrower by default. 
  Contract: Pool, LoanAgent, MinerAPI
  Token: sFIL, WFIL
  Action:
    - deposit (supply): A LP deposits a certain amount of token (FIL) to the pool. For now, we support token WFIL. In return, the LP would receive 1-to-1 amount of sFIL (staked FIL) token. Depositor also earn interests as the tokens are supplied into the pool. 
	  - underwrite: A borrower can submit a request to borrow FIL token as a collateral. The borrower need to specific amount to be borrowed. The protocol will automatically calculate interest rates based on on-chain information from the borrower such as slash rate, length of operation, storage capacity. The borrower needs to provide collateral (could be any other tokens) in order to borrow FIL token. If borrowed successfully, the Pool contract generates a LoanAgent contract which serves as the owner and beneficiary of the miner actor
	  - activate: start the LoanAgent and begin to provide storage services. 
	  - repay:
      1. miner will accumulate block rewards as long as storage providers function.
      2. SP can initialize repay schedule (amount, time)
      3. LoanAgent can pull allowed fund from the actor according to the repayment schedule. 
      4. after repayment, interests should add to each LP should get. 
      5. after all repayments are completed
	
   
   User experience: 
       SP:
    	1. Loan request sent to Pool contract.
	    	1.1. Pool create (CREATE2) a LoanAgent contract 
	    	1.2. Pool contract switches miner actor's owner to LoanAgent contract address. 
    	2. SP can activate LoanAgent loan when she sees fits.
	    	2.1. LoanAgent contract will check owner address as LoanAgent itself.
	    	2.2. LoanAgent contract will change the beneficiary address of the miner actor to be the LoanAgent
	   LP:
		1. deposit FIL
		2. withdraw
		3. WFIL swap


Reference: https://fvm-forum.filecoin.io/t/lending-pool-cookbook/114
https://fvm-forum.filecoin.io/t/building-fvm-use-cases/45

https://github.com/filecoin-project/FIPs/discussions/513

https://github.com/filecoin-project/FIPs/discussions/514

https://github.com/filecoin-project/FIPs/discussions/508
