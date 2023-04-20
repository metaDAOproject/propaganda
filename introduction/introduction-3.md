# From Corporations to Nations: How the Meta-DAO is Going to Change Everything (Part 3)

![intro picture](media/MetaDAOGovernance.drawio.png)

## Introduction

In [existing approaches to human organization](https://medium.com/@metaproph3t/from-corporations-to-nations-how-the-meta-dao-is-going-to-change-everything-part-2-8abe5b6814fc), we allow humans to rule. Whether the organizations be corporations or national governments and whether the rulers be CEOs, board members, prime ministers, congressmen, or ayatollahs, the structure is mostly the same: humans appointed to serve the interests of the group.

Although these approaches are better than no organization at all, they suffer from the problems of giving humans control.[^1]

We propose an alternative organization where control is handed to executable computer code, namely eBPF instructions stored on the Solana blockchain. The design of this organization's algorithm will be the subject of this post.

## Members

We define members as single-product profit-seeking entities. Members have their own treasuries, from which they can pay grants, payroll, and suppliers. Each member also has its own token, which functions like common equity in a company.

![member](media/member.png)

## Proposals

As in many existing DAOs, members take actions via improvement proposals.[^2] Each improvement proposal contains a list of commands, where a command contains a member and a Solana [instruction](https://docs.solana.com/terminology#instruction) that the member can execute.[^3] If a propsal passes, instructions are executed by their corresponding members.

![improvement proposal](media/improvement-proposal.png)

## Decision-making apparatus

In [post 1](https://medium.com/@metaproph3t/from-corporations-to-nations-how-the-meta-dao-is-going-to-change-everything-part-1-a8657562b12e), we described an ideal decision-making algorithm as one that furthers the interests of the group. Here, this means executing any proposal that would have a net-positive impact on the financials of the members.

Concretely, this algorithm is as follows:
1. Estimate, in some base currency, the impact of this proposal on each member's valuation.
2. Summate all of these estimated impacts.
3. Execute the proposal if, and only if, the resulting sum is positive.

In this way, the algorithm neutrally adjudicates between the interests of members. For example, if a proposal would have an estimated +$10MM impact on member A but an estimated -$15MM impact on member B, the proposal would not go through because (+10,000,000) + (-15,000,000) is non-positive.

![](media/decision-making.png)

## Estimating impact

To estimate the financial impact of a proposal on member, we use a system based on [Robin Hanson's futarchy](https://mason.gmu.edu/~rhanson/futarchy.html). 

While a proposal is active, investors can lock arbitrary tokens into a *vault* in exchange for two convertible instruments: one that converts back to the token if the proposal passes and one that converts if it fails. We call the former a *convertible-on-pass* token and the latter a *convertible-on-fail* token.

![](media/conditional-token-minting.png)

We may use the market prices of these convertible instruments to estimate the impact of a proposal on a member's valuation. 

Given some base currency (e.g., SOL, USDC), the market price of a convertible-on-pass member token divided by the market market price of a convertible-on-pass base token reflects the market's assessment of what that member token would be worth if the proposal were to pass. 

For example, if investors believe that a member token would be worth 10 SOL if a proposal were to pass, but a convertible-on-pass member token could be had for 9 convertible-on-pass SOL, investors are incentivized to lock up their SOL in the vault and trade the resultant convertible-on-pass SOL for convertible-on-pass member token until the price reaches 10 convertible-on-pass SOL. This way, if the proposal passes, the investor has locked in the cheaper price; if the proposal fails, they can still get their original SOL back.

We call the *pass price* the price of convertible-on-pass member tokens, quoted in convertible-on-pass base tokens, and the *fail price* the price of convertible-on-fail member tokens, quoted in convertible-on-fail base tokens.

To estimate how member's valuation will be impacted by a proposal, we take subtract the fail price from the pass price and multiply the difference by the member token's total supply. This difference in market capitalization is the market's assessment of the proposal's impact on the member's valuation.

![calculating impact](media/CalculatingImpact.drawio.png)

## Questions and objections

Many futarchy-specific objections and their responses can be found [here](https://mason.gmu.edu/~rhanson/futarchy2013.pdf).

Questions and objections that don't relate to futarchy are enumerated and responded to here.

### Why would you take away this power from humans? Doesn't this undermine human sovereignty?

As discussed in part 2, appointing humans as leaders is problematic. Too often in history, those leaders have abused their position to enrich themselves at the expense of the broader group. This isn't just a phenomena to be observed in cases of visible corruption, but a phenomena to be observed in every corporate board of directors meeting.

As to whether this would undermine human sovereignty, it depends which humans we are discussing. Those who advocate for proposals that are based on reason and add value to the organization are more likely to see those proposals implemented than in traditional organizations. On the other hand, the Meta-DAO has no need for 'consensus-builders' who push proposals through in a pork barrel-style, and they are unlikely to fare so well.

### Couldn't this hurt the broader world?

The algorithm only explicitly optimizes for the interests of members and not for the interests of society more broadly. It is reasonable to ask, then, if this could lead to undesirable outcomes.

Truthfully, noone can answer this question with certainty, but we conjecture that long-term greed (that which the algorithm practices) is beneficial to society because it leads to the creation of new products and the reduction of costs.

It's also worth noting that many of our institutions are already dominated by 'greedy' humans, only where their greed is confined to themselves as individuals. In contrast, the algorithm is greedy for the group as a whole.

## Conclusion

We have demonstrated a design for a human organization where the managerial decisions are made by eBPF instructions on the Solana blockchain. Because this system is not human-led, it doesn't suffer many of the problems of prior approaches to human organization. 

[^1]: We discussed these problems in part 2. For further examples and discussion of this, see [principal-agent problems](https://en.wikipedia.org/wiki/Principal%E2%80%93agent_problem), [corporate governance](https://en.wikipedia.org/wiki/Corporate_governance), [political corruption](https://en.wikipedia.org/wiki/Political_corruption), [regulatory capture](https://en.wikipedia.org/wiki/Regulatory_capture), and [gerrymandering](https://en.wikipedia.org/wiki/Gerrymandering).
[^2]: This system is described in [Compound Governance](https://medium.com/compound-finance/compound-governance-5531f524cf68).
[^3]: We say that 'members execute instructions' because it is intuitive and describes the high-level process accurately. Under the hood though, really what's happening is that the Meta-DAO program is calling [`solana_program::program::invoke_signed`](https://docs.rs/solana-program/latest/solana_program/program/fn.invoke_signed.html) with that member's PDA seeds. 

