# Domain Model

The platform tracks merchants, the payment requests they raise, the transactions
payers complete against them, and the payouts merchants draw from their balance.

```mermaid
erDiagram
    MERCHANT ||--o{ PAYMENT_REQUEST : creates
    MERCHANT ||--o{ PAYOUT : requests
    PAYMENT_REQUEST ||--o| TRANSACTION : "is paid by"
    TRANSACTION ||--o| REFUND : "may have"

    MERCHANT {
        string id
        string businessName
        string email
        string phone
        string payoutDestinationType
        string payoutDestinationRef
        string status
        datetime createdAt
    }
    PAYMENT_REQUEST {
        string id
        string merchantId
        decimal amount
        string currency
        string description
        string status
        datetime createdAt
        datetime expiresAt
    }
    TRANSACTION {
        string id
        string paymentRequestId
        string channel
        decimal amount
        string currency
        string payerContact
        string status
        string providerReference
        datetime paidAt
    }
    REFUND {
        string id
        string transactionId
        decimal amount
        string reason
        string status
        datetime createdAt
    }
    PAYOUT {
        string id
        string merchantId
        decimal amount
        string currency
        string destinationType
        string status
        string providerReference
        datetime requestedAt
        datetime completedAt
    }
```

- **Merchant** — a registered business; `status` tracks active/suspended.
`payoutDestinationType` is `bank` or `mobile-wallet`.
- **PaymentRequest** — an amount a merchant asks a payer to pay; `status`
moves from `pending` to `paid` or `expired`.
- **Transaction** — the payer's actual payment against a request, via
`channel` `mobile-money` or `card`; `status` is `pending`, `succeeded` or
`failed`.
- **Refund** — a merchant-initiated reversal of a succeeded transaction.
- **Payout** — an on-demand withdrawal of a merchant's balance to their payout
destination; `status` moves from `pending` to `completed` or `failed`.

