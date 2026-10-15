# UNE Maritime MCP Gateway

[![JSON-RPC 2.0](https://img.shields.io/badge/JSON--RPC-2.0-blue.svg)](https://www.jsonrpc.org/)
[![Model Context Protocol](https://img.shields.io/badge/MCP-Standardized-green.svg)](https://modelcontextprotocol.io/)
[![Regulatory Basis](https://img.shields.io/badge/Legal--Basis-EU%202023%2F1805%20%7C%20EU%202023%2F959-purple.svg)](https://eur-lex.europa.eu/)
[![Execution](https://img.shields.io/badge/Performance-Sub--millisecond-emerald.svg)]()

Deterministic regulatory compliance and economic optimization gateway for the European Union maritime decarbonization framework (FuelEU Maritime & EU ETS Maritime 2026).

Operated as a native, sub-millisecond MCP node within the **Une.education** (Unified Norms Engine) cross-border architecture.

## Primary Capabilities

1. **Fuel Factor Reference (`lookup_fuel_wtw_emission_factor`)**: Statutory Well-to-Wake (WtW), Well-to-Tank (WtT), Tank-to-Wake (TtW), and LCV default parameters directly sourced from Regulation (EU) 2023/1805 Annex II.
2. **Voyage Aggregation (`calculate_voyage_ghg_intensity`)**: Aggregates multi-leg voyages, enforcing port-to-scope consistency and applying the Article 2 geographical scope (50% on extra-EU inbound/outbound routes, 100% intra-EU) against statutory limits (89.3368 gCO₂eq/MJ).
3. **Article 20 Solver (`calculate_fueleu_banking_borrowing`)**: Solves compliance banking and statutory borrowing constraints (capped at 2% of target, mandatory 1.1x repayment factor, 2-year consecutive borrowing limit) with economic arbitrage vs. market pooling.
4. **Article 21 Pool Optimizer (`optimize_fueleu_pool_allocation`)**: Pure mathematical solver to optimize fleet and cross-company pooling under Regulation (EU) 2023/1805 Art. 21. Eliminates state penalties via internal transfer mechanisms.
5. **Statutory Penalties (`calculate_fueleu_maritime_penalty`)**: Auditable calculation of FuelEU financial cash penalties under Annex IV formulas: `(deficit_gCO2eq / (actual_intensity * 41000)) * 2400 EUR`.
6. **EU ETS Maritime Surrender (`calculate_maritime_ets_surrender`)**: Determines EU Allowance (EUA) surrender obligations under Directive (EU) 2023/959 with multi-year phase-in support (40% in 2024, 70% in 2025, 100% in 2026+).
7. **Autonomous In-band Provisioning (`provision_free_api_key`)**: Grants instant sandbox access credentials (50 credits) for qualified agents and developers.

## Strict Mathematical Determinism Benchmark

Unlike probabilistic LLMs, all calculations are sealed with a canonical SHA-256 state proof. Two identical calls produce the exact same `result_hash`, independently of timestamp or execution nonce:

```bash
# Call 1
curl -sk -X POST [https://maritime.une.education/mcp.php](https://maritime.une.education/mcp.php) \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"lookup_fuel_wtw_emission_factor","arguments":{"fuel_type":"VLSFO"}}}' \
  | jq .result._meta.state_proof.result_hash

# Call 2 (Executed later)
curl -sk -X POST [https://maritime.une.education/mcp.php](https://maritime.une.education/mcp.php) \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"lookup_fuel_wtw_emission_factor","arguments":{"fuel_type":"VLSFO"}}}' \
  | jq .result._meta.state_proof.result_hash

# Both outputs match:
# "38be6e35593c66fbf448cf6097e3a61f364e0375a36371cf0e6b5278772a8c32"
Maintainer
Identity: Une-education

Contact: ops@une.education

Infrastructure: Unified Norms Engine (UNE)
