# Proven Library Integration Plan

This document outlines how the [proven](https://github.com/hyperpolymath/proven) library's formally verified modules can be integrated into poly-proof-mcp.

**Note:** This repo has special affinity with proven - it's a proof-related MCP server that can directly leverage proven's theorem proving infrastructure.

## Applicable Modules

### Core Integration (High Priority)

| Module | Use Case | Formal Guarantee |
|--------|----------|------------------|
| `SafeThm` | Theorem management | Well-typed theorem statements |
| `SafeProof` | Proof checking | Valid proof terms |
| `SafeStateMachine` | Proof state transitions | Valid tactic application |
| `SafeGraph` | Proof dependency graph | Acyclic lemma dependencies |

### Supporting Modules

| Module | Use Case | Formal Guarantee |
|--------|----------|------------------|
| `SafeSchema` | Proof format validation | Well-formed proof objects |
| `SafeProvenance` | Proof lineage | Auditable proof history |
| `SafeCapability` | Proof access control | Scoped proof permissions |

## Integration Points

### 1. Theorem Management (SafeThm)

```
proof_state → SafeThm.validateStatement → typed Theorem
proof_apply_tactic → SafeThm.checkTactic → valid TacticApplication
```

Theorem types are parameterized by their proof state:
- `Theorem (Unproven)`: Statement without proof
- `Theorem (Partial goals)`: In-progress proof
- `Theorem (Proven)`: Completed proof with QED

### 2. Proof State Machine (SafeStateMachine)

```
:unproven → :goal_set → :tactics_applied → :proven
```

Each tactic application is a verified state transition:
- `intro`: Add hypothesis to context
- `apply`: Apply lemma to goal
- `rewrite`: Substitute equal terms
- `qed`: Complete proof

### 3. Proof Graph (SafeGraph)

```
SafeGraph.acyclic lemmas dependencies → valid theory
SafeGraph.topologicalSort → proof order
```

Ensures no circular dependencies in lemma definitions.

## Prover Integrations

| Prover | Protocol | proven Module |
|--------|----------|---------------|
| Lean 4 | LSP | SafeThm for goal state |
| Coq | SerAPI | SafeProof for proof terms |
| Idris 2 | IDE Protocol | Direct integration |
| Agda | Emacs mode | SafeSchema for holes |

## Direct proven Usage

Since poly-proof-mcp is about formal proofs, it can directly expose proven's Idris modules:

```typescript
// MCP tool: proven_check_proof
{
  name: "proven_check_proof",
  description: "Verify a proof term against proven's SafeProof module",
  inputSchema: { proofTerm: "string", expectedType: "string" }
}
```

## Status

- [ ] Integrate SafeThm for theorem state management
- [ ] Add SafeStateMachine for tactic application
- [ ] Build prover adapters (Lean, Coq, Idris)
- [ ] Expose proven modules as MCP resources
