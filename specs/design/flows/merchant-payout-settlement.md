# Merchant Payout Settlement

A Merchant requests an on-demand payout of their collected balance, and is
notified by email once it completes. A Platform Admin monitors payout runs
across all merchants.

```mermaid
sequenceDiagram
    actor Merchant
    actor Admin as Platform Admin
    participant portal as merchant-portal
    participant api as payments-api
    participant payout as payout-service
    participant email as email-service

    Merchant->>portal: request payout
    portal->>api: create payout
    api->>payout: disburse to bank or mobile wallet
    payout-->>api: payout result
    api->>email: notify merchant payout completed
    api-->>portal: payout status

    Admin->>portal: open settlement monitor
    portal->>api: list payouts across merchants
    api-->>portal: payout statuses
```