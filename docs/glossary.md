# Glossary — Electronic Voucher Stock Transfer

Educational terminology for **electronic voucher stock transfer** and EVD inventory transfer between dealers. Definitions are industry-oriented; vendor labels vary.

## Core Terms

| Term | Definition |
|------|------------|
| **Stock transfer** | Movement of voucher inventory custody between nodes without end-customer sale |
| **Dealer hierarchy** | Tree of warehouse, master dealer, sub-dealer, and retail points |
| **Custody** | Current responsible owner of serials or electronic allotment |
| **Transfer ID** | Durable identifier for one custody move |
| **Reservation** | Source-side hold preventing sale of stock mid-transfer |
| **In transit** | State after source release / before destination confirm |
| **Serial range** | Contiguous PIN/voucher identifiers moved as a unit |
| **Allotment / e-stock** | Quantity-based electronic inventory without printed cards |
| **Credit limit** | Max exposure a destination dealer may accept |
| **Return transfer** | Reverse flow sending surplus stock up or sideways in the tree |
| **Day-close** | Period reconciliation of transfers, sales, and float |
| **Orphan serial** | Stock with unclear or dual custody after a failed transfer |

## Flow States (Typical)

```
requested → approved → reserved → in-transit → confirmed
                              └→ reversed / quarantined / ops review
```

## Roles

| Role | Concern |
|------|---------|
| **Source dealer** | Releases stock; needs reservation and confirm receipts |
| **Destination dealer** | Accepts custody; needs credit headroom |
| **Hub / warehouse** | Initiates bulk allotments and return paths |
| **Platform ops** | Resolves stuck in-transit and orphan cases |
| **Finance** | Reconciles float and inventory on both nodes |

## Design Notes

- Bind every custody change to a **transfer ID** before mutating available stock.  
- Prefer explicit **in-transit** over optimistic destination credit.  
- Keep **serial integrity**: never split a sold or redeemed serial into a transfer.  
- Align transfer reports on **both** source and destination day-close files.

## Related Reading

Live product reading: [evdsystem.com](https://evdsystem.com/), [electronic voucher management system](https://evdsystem.com/electronic-voucher-management-system-evd-telecom/).

MoboGage / EVD System materials on evdsystem.com describe electronic voucher distribution hierarchies where dealer stock transfer is a routine operations control.
