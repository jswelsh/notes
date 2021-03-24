# Governance Alpha

## Governance

The Compound protocol is governed and upgraded by COMP token-holders, using three distinct components; the COMP token, governance module (Governor Alpha), and Timelock. Together, these contracts allow the community to propose, vote, and implement changes through the administrative functions of a cToken or the Comptroller. Proposals can include changes like adjusting an interest rate model, to adding support for a new asset.

Any address with more than 100,000 COMP delegated to it may propose governance actions, which are executable code. When a proposal is created, the community can submit their votes during a 3 day voting period. If a majority, and at least 400,000 votes are cast for the proposal, it is queued in the Timelock, and can be implemented after 2 days.

<img src="gov_diagram.png
" alt="forked commit history" width="500"/>
Governance Diagram

### COMP
COMP is an ERC-20 token that allows the owner to delegate voting rights to any address, including their own address. Changes to the owner’s token balance automatically adjust the voting rights of the delegate.

### Delegate
Delegate votes from the sender to the delegatee. Users can delegate to 1 address at a time, and the number of votes added to the delegatee’s vote count is equivalent to the balance of COMP in the user’s account. Votes are delegated from the current block and onward, until the sender delegates again, or transfers their COMP.