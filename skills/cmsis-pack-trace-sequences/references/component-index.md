# Component asset index

Read this file first, then load only the selected component assets.

| Asset | Use when | Required verified facts |
|---|---|---|
| `components/etb.xml` | ETB is the trace sink | ETB base address and AP |
| `components/etf.xml` | ETF is a FIFO or circular-buffer sink | ETF base address and AP |
| `components/etr-catu.xml` | ETR is used, with optional CATU | ETR base address/AP; CATU facts only when CATU is present |
| `components/tpiu.xml` | SWO or synchronous TPIU output is used | TPIU base address, AP, and output mode |
| `components/swo.xml` | standalone SWO output is used | SWO/TPIU base address, AP, clocks |
| `components/funnel.xml` | a trace funnel is on the selected path | base address, AP, input mask |
| `components/replicator.xml` | a replicator is on the selected path | base address, AP, output filters |
| `components/timestamp-generator.xml` | a timestamp generator is present | base address, AP, documented control interface |
| `components/cpu-trace.xml` | CPU trace enablement is required | processor and DEMCR applicability |

Use ETR/CATU as one asset for now. If the target lacks CATU, omit the CATU variables and sequences from that asset's merge. A future split is mechanical because the XML keeps ETR and CATU identifiers distinct.
