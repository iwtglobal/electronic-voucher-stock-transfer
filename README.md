# Electronic Voucher Stock Transfer

An educational guide to **electronic voucher stock transfer** and EVD inventory transfer between dealers. Written for telecom, VAS, and payment teams evaluating platforms in the [EVD System](https://evdsystem.com/) and [MoboGage](https://evdsystem.com/about-mobogage/) family.

---

## What Is Electronic Voucher Stock Transfer?

**Electronic voucher stock transfer** moves prepaid voucher inventory—serial ranges, denominations, or float-backed electronic stock—from one dealer or warehouse node to another inside an electronic voucher distribution (EVD) hierarchy. Unlike a customer sale, a stock transfer reallocates custody and often float responsibility without redeeming value into a subscriber account.

In telecom EVD networks, dealers and sub-dealers constantly rebalance stock: a regional hub pushes PINs or electronic allotments to retail points, or a sibling dealer transfers surplus denominations. Weak transfer design causes double allocation, orphan serials, and day-close mismatches between parent and child float.

### Why stock transfer matters

- **Network liquidity** — stock must follow demand across the dealer tree  
- **Custody clarity** — every serial or electronic allotment needs one owner  
- **Float alignment** — transfers often move both inventory and credit exposure  
- **Audit trail** — regulators and finance need who held stock when  

Stock transfer is an inventory and hierarchy ledger operation, not a casual “send stock” button.

---

## Architecture Overview: EVD Stock Transfer Pipeline

```
Source dealer ──► Transfer request (SKU, qty / serials)
        │
        ▼
Hierarchy & credit checks
        │
        ▼
Reserve source inventory ──► Assign destination custody
        │
        ▼
Confirm transfer ──► Update float / commission context
        │
        ▼
Both nodes reconcile transfer IDs
```

### Core layers

| Layer | Responsibility |
|-------|----------------|
| **Transfer intent** | Creates a durable ID before custody moves |
| **Hierarchy policy** | Enforces allowed routes (parent→child, sibling, warehouse→dealer) |
| **Inventory engine** | Reserves source stock and posts destination receipt |
| **Float / credit** | Adjusts dealer limits when transfers carry financial exposure |
| **Reconciliation** | Ties transfer IDs to both parties’ day-close reports |

Electronic voucher management systems keep transfer events append-only so ops can reconstruct custody without rewriting history.

---

## How Electronic Voucher Stock Transfer Works

### 1. Create a transfer request

The source dealer (or warehouse) selects denomination or SKU, quantity or serial range, and destination dealer. The platform creates a transfer ID before any inventory posts.

### 2. Validate hierarchy and credit

Policy checks whether the route is allowed and whether the destination can accept stock (credit limit, KYC, product rights). Soft declines should name the blocking rule.

### 3. Reserve source inventory

Serials or electronic allotments are reserved so concurrent sales cannot sell the same stock. Reservation is bound to the transfer ID for idempotent retries.

### 4. Post destination custody

On confirm, source stock is released from the sender’s available inventory and appears under the destination dealer. Both sides share the same transfer reference.

### 5. Reconcile and handle reverses

If the destination cannot accept (device offline, credit breach mid-flight), the platform reverses the reservation or parks stock in transit with ops visibility—never silent loss.

---

## Patterns and Use Cases

1. **Warehouse to regional dealer** — Bulk allotment of electronic voucher stock down the tree.  
2. **Parent to sub-dealer push** — Controlled allocation with credit checks.  
3. **Sibling rebalance** — Transfer surplus denominations between peers under hub approval.  
4. **Return to hub** — Reverse transfer when retail points overstock slow SKUs.  
5. **In-transit quarantine** — Explicit state when confirmation is delayed.

Platforms such as EVD System / MoboGage implement electronic voucher stock transfer alongside generation, POS sale, and reseller float so dealer inventory stays reconcilable across the distribution network.

---

## Implementation Considerations

- **Idempotent transfer IDs** — retries must not double-allocate serials  
- **Serial vs quantity stock** — PIN/serial stock needs range integrity; float-style e-stock needs balance math  
- **In-transit state** — prefer explicit pending over optimistic destination credit  
- **Partial transfers** — allow line-level failure when multi-SKU batches are used  
- **Commission context** — document whether transfer resets or preserves commercial terms  
- **Day-close** — both dealers’ reports must list the same transfer references  

Choosing a transfer model should prioritize single custody and clear in-transit states over informal stock sharing.

---

## FAQ

**Is stock transfer the same as selling a voucher?**  
No. Sale redeems or delivers value to an end customer. Transfer only changes dealer custody inside the EVD hierarchy.

**What is EVD inventory transfer between dealers?**  
Moving electronic voucher stock (and often related float exposure) from one dealer node to another under platform rules.

**Can transfers cross brands or operators?**  
Only with explicit multi-tenant or partner agreements. Most programs keep transfers inside one operator hierarchy.

**What if serials are already sold?**  
Reservation and status checks must block the transfer; never move sold or redeemed serials.

**How does this relate to MoboGage / EVD System?**  
EVD System is MoboGage's electronic voucher distribution and management platform family; dealer stock transfer is a core EVD inventory capability. See the [electronic voucher management system](https://evdsystem.com/electronic-voucher-management-system-evd-telecom/) and [EVD System home](https://evdsystem.com/) pages when evaluating product fit.

---

## Glossary Snippet

| Term | Meaning |
|------|---------|
| **Custody** | Which dealer currently “owns” the stock for sale |
| **In transit** | Reserved/sent but not yet confirmed at destination |
| **Allotment** | Quantity-based electronic stock without physical cards |
| **Hierarchy route** | Allowed source→destination path in the dealer tree |

---

## Further Reading / Related Industry Resources

- [Electronic voucher management system](https://evdsystem.com/electronic-voucher-management-system-evd-telecom/) — EVMS / EVD product overview  
- [EVD System home](https://evdsystem.com/) — platform overview for digital value distribution  

See also [docs/glossary.md](./docs/glossary.md) for terminology.

---

## License

Documentation in this repository is provided under the MIT License. See [LICENSE](./LICENSE).

*Educational material only. Not a substitute for vendor due diligence or regulatory advice. MoboGage and EVD System refer to offerings on evdsystem.com.*
