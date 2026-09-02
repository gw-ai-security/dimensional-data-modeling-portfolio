# Final Validation Checklist

> **Status: guided implementation complete; release validation not yet fully closed.**

## Source-controlled model semantics

- [x] every final fact has a grain statement
- [x] every final dimension has a grain statement
- [x] fact/dimension classifications documented
- [x] no direct fact-to-fact relationship in TMDL
- [x] active/inactive Date roles documented
- [x] Ship-To/Bill-To Geo roles documented
- [x] order-process fan-out root cause and fix documented
- [x] core measure definitions documented
- [x] dynamic RLS role source-controlled

## Recorded metrics

- [x] Total Sales reference recorded
- [x] Orders reference recorded
- [x] Customer counts recorded
- [x] Target / attainment references recorded
- [x] Order Process failure/reference counts recorded

## Repository evidence

- [x] source-model before-state committed
- [x] source assessment complete
- [x] grain matrix complete
- [x] dimension/fact documentation current
- [x] relationship inventory current
- [x] architecture decisions current
- [x] debugging/failure evidence documented
- [x] README no longer claims the project is only at 03:45:58

## Runtime release gates — still open

- [ ] current `main` PBIP opens and Refresh succeeds after latest Product changes
- [ ] `dim_product` query loads without error after Unmapped member addition
- [ ] Product key types/relationship are accepted by Power BI after refresh
- [ ] Sales Trend renders values through `dim_date`
- [ ] Sales vs Target renders both measures at a valid common time grain
- [ ] Product Category contains explicit `Unmapped` instead of unexplained `(Blank)`
- [ ] `fact_order_process` confirms 80 rows / 80 distinct Orders
- [ ] representative Dynamic RLS `View As` tests executed (minimum two users)
- [ ] restricted totals reconciled
- [ ] final model screenshot committed under `screenshots/after/`
- [ ] Business Overview screenshot committed under `screenshots/after/`
- [ ] independent no-tutorial audit complete

## Claim gate

Until every runtime/independent gate above is complete, the correct claim is:

> **Guided implementation complete; final runtime validation and independent audit pending.**

Do not describe the portfolio as fully validated production Power BI evidence before this checklist is closed.