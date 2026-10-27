---
hidden: true
noIndex: true
---

# Flow Coordination Engine

The Flow Coordination Engine (FCE) is BlockRazor’s core internal engine system for coordinating on-chain transaction flows, matching, auctions, ordering, and dynamic composition.

FCE is not exposed directly to customers as a standalone product. Instead, it serves as foundational infrastructure behind multiple BlockRazor business systems, providing a unified coordination and decision-making layer for scenarios such as block building, order flow auctions, dynamic bundle composition, protocol-internal matching, and execution optimization.

The core of FCE is not limited to matching, auctions, or ordering. Rather, it combines the distinctive business characteristics of on-chain transactions with matching algorithms, auction mechanisms, dynamic composition, and real-time decision-making to form a complex transaction coordination system for orders, transactions, bundles, intents, bids, and protocol-internal signals.

In on-chain trading environments, flow is not simply a generic request queue. Different transaction objects often reflect different business intents, revenue expressions, risk constraints, priorities, competitive relationships, and execution windows. If such flows are handled only through generic queues, static rules, or a single matching algorithm, it is difficult to achieve stable outcomes in complex, high-frequency, and highly competitive environments.

FCE integrates transaction business modeling, matching algorithms, auction mechanisms, dynamic composition, ordering strategies, execution constraints, and real-time feedback into a unified internal engine architecture. This enables BlockRazor’s business systems to organize on-chain transaction flows more efficiently and produce better allocation, composition, and decision outcomes across different execution paths.

### Core Positioning

FCE handles business relationships within on-chain transaction flows, not merely the order of data processing.

In systems such as block builders, order flow auctions, bundle composition, and protocol matching, the value of transaction flow depends on many factors: whether transactions can be combined, whether conflicts exist, whether they are constrained by execution windows, whether they must participate in bidding, whether they affect overall returns, and whether they comply with specific protocol or market-structure constraints.

Accordingly, the key questions FCE focuses on include:

How transaction flows are identified, classified, and modeled\
Whether different orders, transactions, bundles, intents, and bids have combinational or conflicting relationships\
How matching, auctions, ordering, and composition can be completed within limited time windows\
Whether execution paths comply with target protocols, market structures, and business constraints\
How transaction flows should be assigned to more suitable executors or processing paths\
How the system can maintain stable decision-making capability in high-concurrency, highly competitive, and rapidly changing environments\
FCE is not a single-point matching algorithm, but an internal engine system that combines transaction business logic with algorithmic decision-making.

### System Components

FCE consists of four core capability groups:

* Transaction business and flow-object modeling
* Coordinated matching and auction mechanisms
* Dynamic composition and ordering optimization
* Real-time feedback and adaptive decision-making

Together, these capabilities support BlockRazor’s handling of complex on-chain transaction flows, allowing different business systems to reuse a unified framework for transaction understanding, matching, composition, and decision-making.

#### Transaction and Flow-Object Modeling

In FCE, flow is treated as a transaction object with business meaning, rather than as a simple message or request.

Here, flow includes not only order flow in the traditional sense, but also transaction flow, bundle flow, intent flow, bid flow, solver flow, protocol flow, and other signal flows related to on-chain execution. Different flows may correspond to different trading intents, revenue structures, risk exposures, execution constraints, and timing windows.

These objects typically have different data structures, value expressions, execution conditions, lifecycles, and dependency relationships. FCE builds modeling capabilities around these characteristics so that the system can determine the role each type of flow plays in matching, auctions, ordering, and execution paths.

This modeling capability allows FCE to determine how a flow object should enter downstream coordination processes based on its business semantics, timeliness, constraints, composability, and potential value.

#### Coordinated Matching and Auction Mechanisms

FCE must coordinate transactions across multiple participants, multiple opportunities, and multiple constraints.

In on-chain environments, matching is not simply a matter of pairing buyers and sellers. It may involve order flow allocation, bid-signal processing, execution-right selection, protocol-internal matching, solver competition, liquidity-path selection, and multi-party interest coordination.

FCE combines matching logic with auction mechanisms according to the needs of different business systems, so that transaction flows, bundles, intents, and bids can be evaluated, compared, and allocated within a unified framework.

The goal of this process is not merely to choose the highest bid or the fastest path, but to reach better decisions across constraints, execution quality, timing windows, and total value.

#### Dynamic Composition and Ordering Optimization

On-chain transaction flows are often composable.

Multiple transactions, bundles, intents, or bids may complement one another or conflict with one another. Some combinations can create higher execution value, while some ordering choices can affect execution outcomes, risk exposure, or final returns.

FCE performs dynamic composition and ordering optimization around these relationships. Within limited time windows, the system must determine which flows can be combined, which should be excluded, which should be prioritized, and how outcomes may differ across execution paths.

This capability makes FCE more than a passive processor of incoming flows. It allows the system to structurally organize input flows and transform fragmented on-chain opportunities into more executable, higher-quality decision outcomes.

#### Real-Time Feedback and Adaptive Decision-Making

The on-chain trading environment is constantly changing.

Market prices, liquidity conditions, gas conditions, mempool state, bidding intensity, protocol constraints, execution success rates, and external signals all affect the optimal way transaction flows should be handled.

Through real-time feedback mechanisms, FCE observes transaction-flow processing outcomes and uses these signals to optimize subsequent matching, auctions, ordering, and composition. Based on execution results, failure reasons, competitive conditions, and market changes, the system can continuously adjust its decision strategies and reduce reliance on static rules.

In high-frequency, highly competitive, and time-sensitive environments, this adaptive decision-making capability is a critical foundation for maintaining execution quality and systemic advantage.

### Supported Business Systems

FCE is the shared core engine capability behind multiple BlockRazor business systems, including:

* Block builder
* Order flow auction
* Dynamic bundle composition
* Protocol-internal matching
* Transaction flow allocation and ordering
* Execution path optimization

These systems provide different product capabilities for different scenarios, but all of them need to process complex on-chain transaction flows and complete matching, bidding, ordering, composition, or allocation within limited time windows.

FCE provides these business systems with a unified underlying coordination capability, allowing BlockRazor to reuse transaction business modeling, matching mechanisms, dynamic composition, and real-time decision-making across different business scenarios.

### Conclusion

The value of FCE lies in elevating on-chain transaction-flow handling from a “single-point matching problem” to a “business-aware coordination and decision-making problem.”

By continuously modeling transaction business characteristics, matching relationships, execution constraints, competitive structures, combination space, and real-time conditions, FCE provides BlockRazor’s business systems with a reusable, evolvable core engine capability that is difficult to replicate through any single algorithm alone.
