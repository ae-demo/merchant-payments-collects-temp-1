# Merchant Payments Platform — PRD

## Problem Statement

Small and medium merchants need to accept digital payments from customers who pay
with mobile money or a card, but stitching together a payment collection flow,
transaction records, and getting the collected funds into their own bank account
or mobile wallet today means juggling multiple disconnected tools, manual
reconciliation, and no single place to see what has been collected and what is
still owed to them.

## Solution

A merchant payments platform where a merchant creates an account, generates
payment requests that customers pay via mobile money or card, tracks every
transaction in one dashboard, and receives the collected funds through the
platform's own settlement process — with a platform admin overseeing merchants
and transactions across the whole system.

## Actors

- **Merchant** — signs up, manages their business profile and payout
destination, creates payment requests, views their transactions and balance,
issues refunds, and receives settlement payouts.
- **Payer** — the merchant's customer; pays a merchant's payment request via
mobile money or card and receives a receipt.
- **Platform Admin** — oversees all merchants and transactions across the
platform: reviews and approves merchant accounts, monitors transactions and
settlement runs, and can suspend a merchant.

## User Stories

1. As a Merchant, I want to sign up and create a merchant account, so that I can start accepting payments.
2. As a Merchant, I want to set up my business profile and payout destination (bank account or mobile wallet), so that settlements are sent to the right place.
3. As a Platform Admin, I want to view newly registered merchant accounts, so that I can monitor onboarding and flag any that need a closer look.
4. As a Merchant, I want to create a payment request for a specific amount, so that I can send it to a customer to pay.
5. As a Payer, I want to pay a merchant's payment request via mobile money, so that I can complete my purchase.
6. As a Payer, I want to pay a merchant's payment request via card, so that I can complete my purchase.
7. As a Payer, I want to receive a receipt after paying, so that I have proof of payment.
8. As a Merchant, I want to view a dashboard of my transactions, so that I can track what has been collected.
9. As a Merchant, I want to see my current balance and settlement history, so that I know how much I will be paid out and when.
10. As a Merchant, I want to be notified when a payout I requested completes, so that I know my funds have reached my bank account or mobile wallet.
11. As a Merchant, I want to request an on-demand payout, so that I can access my funds sooner when I need to.
12. As a Merchant, I want to issue a refund to a payer, so that I can handle returns or disputes.
13. As a Platform Admin, I want to view all transactions across every merchant, so that I can monitor platform health and investigate issues.
14. As a Platform Admin, I want to monitor settlement runs across merchants, so that I can catch and resolve a payout failure.
15. As a Platform Admin, I want to suspend a merchant account, so that I can stop a merchant from collecting further payments when there is a problem.

## Product Decisions

- **Sign-in**: all actors sign in via SSO through Thunder, the platform IDP (org default).
- **Payment collection channels**: the platform collects mobile-money payments and card payments. No specific mobile-money or card processor has been named yet — the provider(s) will be chosen when the corresponding dependency is defined at design time.
- **Payment scope**: one-time payments only (checkout-style payment requests); recurring/subscription billing is not part of this product.
- **Merchant onboarding gate**: a new merchant account can create payment requests and collect payments immediately after sign-up; there is no admin approval gate. A Platform Admin can still view new merchants and suspend one after the fact.
- **Settlement schedule**: merchants are settled on demand only — a merchant requests a payout and the platform pays it out; there is no automatic recurring settlement run.
- **Currency scope**: the platform operates in a single currency at launch — Kenyan Shilling (KES).
- **Settlement geography**: the platform serves merchants and payers in Kenya, settling payouts to Kenyan bank accounts or mobile-money wallets. No specific payout rail/provider has been named yet — it will be chosen when the corresponding dependency is defined at design time.
- **Refund window**: a merchant can refund a transaction at any time after collection, with no fixed cutoff window. *assumed*
- **Payment receipts**: receipts and payment confirmations are sent to the payer by email. *assumed*

## Out of Scope

- Recurring or subscription billing.
- In-person POS hardware or card-reader integrations.
- Multi-currency support.
- Merchant-side accounting, invoicing, or tax reporting beyond transaction and settlement records.
- Dispute/chargeback workflows beyond a merchant-initiated refund.

## Open Questions

1. Which mobile-money provider(s) and which card processor should the platform integrate with?

## Further Notes

The idea statement that started this project ends mid-sentence ("...collects
mobile-money and card payments,"); this PRD treats the platform's scope as
collection plus merchant settlement, per the actor and journey answers given
during the interview.