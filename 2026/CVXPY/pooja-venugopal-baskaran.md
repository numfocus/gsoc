# [CVXPY] Post-Solver Feasibility and Optimality Checks

**Applicant:** Pooja Venugopal Baskaran
**Email:** pooja.vb2000@gmail.com
**GitHub:** https://github.com/Pooja2420
**Discord:** pooja07347
**Location:** Chicago, IL, USA (UTC-5)

## Abstract

CVXPY transforms user-defined optimization problems into standard solver forms, which can introduce discrepancies between solver-reported tolerances and the actual feasibility of the original constraints. This project delivers two post-solver verification features: (1) feasibility checks that evaluate all original constraints against returned solutions and emit structured warnings when violations exceed user-specified tolerances, and (2) optimality conditions checks that extend the GSoC 2023 work by Aryaman Jeendgar to support DNLP with dual variable recovery via IPOPT.

## Mentors
- William Zhang
- Daniel Cederberg
- Parth Nobel

## Project Size
350 hours (Large)

## Community Contributions
- Joined CVXPY Discord (pooja07347)
- Fixed issue cvxpy/cvxpy#3227
- Submitted PR marimo-team/learn#141

## AI Disclosure
Portions of this proposal were drafted with assistance from Claude (Anthropic) as a writing aid. All technical content, project understanding, code contributions, and ideas are my own.
