# Black-Box Testing Documentation

Comprehensive black-box test analysis for an e-commerce coupon code validation system, demonstrating professional software quality assurance practices including Equivalence Partitioning, Boundary Value Analysis, and detailed test case design.

## Overview

This project showcases systematic black-box testing methodology applied to a real-world e-commerce feature: coupon code validation and discount application. The analysis demonstrates professional QA practices used in industry to ensure software quality without access to internal code implementation.

## Project Contents

### Test Analysis Documentation
- **Equivalence Partitioning (EP) Analysis**: Comprehensive categorization of input domains into valid and invalid partitions
- **Boundary Value Analysis (BVA)**: Systematic testing of edge cases and boundary conditions
- **Test Case Suite**: 23 detailed test cases covering positive, negative, and boundary scenarios

### System Under Test

E-commerce shopping cart coupon validation system with the following requirements:

**REQ-01**: Coupon input field visible at checkout
**REQ-02**: Coupon codes must be alphanumeric, 4-8 characters, case-insensitive
**REQ-03**: Two valid coupons:
- `SAVE10` - 10% discount
- `5OFF` - $5.00 fixed discount

**REQ-04**: Minimum purchase requirements:
- `SAVE10` requires ≥ $50.00 subtotal
- `5OFF` has no minimum

**REQ-05**: Invalid codes display "Invalid Coupon Code" error
**REQ-06**: Only one coupon allowed per order; total cannot go below $0.00

## Testing Methodology

### Equivalence Partitioning (EP)

Divides input domains into equivalence classes where all values in a partition should exhibit identical behavior:

**Input Categories Analyzed:**
1. **Coupon Code Format** (8 partitions)
   - Valid: Alphanumeric 4-8 characters, numeric-only
   - Invalid: Too short, too long, special characters, empty/null, whitespace

2. **Coupon Code Type** (3 partitions)
   - Valid percentage coupon (`SAVE10`)
   - Valid fixed amount coupon (`5OFF`)
   - Non-existent codes

3. **Cart Subtotal** (5 partitions)
   - Valid: ≥$50 for SAVE10, any positive for 5OFF
   - Invalid: Below minimum, zero cart
   - Edge: Odd-cent values

4. **Discount Calculation** (3 partitions)
   - Discount leaves positive balance
   - Discount equals cart total
   - Discount exceeds cart total (capped at $0)

### Boundary Value Analysis (BVA)

Tests edge cases at partition boundaries:

**Boundaries Tested:**
- **Code Length**: 3, 4, 8, 9 characters
- **SAVE10 Minimum**: $49.99, $50.00, $50.01
- **5OFF Edge Cases**: $0.00, $0.01, $5.00, $5.01
- **Discount Capping**: Scenarios where discount equals or exceeds subtotal

## Test Case Suite

### Coverage Statistics
- **Total Test Cases**: 23
- **Positive Scenarios**: 13 test cases
- **Negative Scenarios**: 9 test cases
- **Boundary Tests**: 8 test cases
- **Requirements Coverage**: 100% (all REQ-01 through REQ-06)

### Test Categories

**Format Validation** (TC-008 to TC-012)
- Length constraints (3, 4, 8, 9 characters)
- Special character handling
- Empty/null inputs
- Whitespace handling

**Business Rule Validation** (TC-001 to TC-007)
- Minimum purchase enforcement
- Percentage discount calculation
- Fixed amount discount application
- Discount capping at $0.00

**Functional Requirements** (TC-013 to TC-023)
- Case-insensitive validation
- Non-existent code rejection
- Single coupon per order enforcement
- Decimal precision in calculations
- Odd-cent value handling

## Key Features Demonstrated

### QA Best Practices
- Systematic test design using EP/BVA techniques
- Comprehensive requirement traceability
- Realistic test data selection
- Clear expected results definition
- Coverage of positive, negative, and edge cases

### Professional Documentation
- Structured analysis tables
- Detailed test case specifications
- Step-by-step test procedures
- Explicit requirement mapping
- Coverage metrics and summaries

### Testing Techniques
- **Equivalence Partitioning**: Efficient test case reduction
- **Boundary Value Analysis**: Edge case identification
- **Requirement-Based Testing**: Full traceability
- **Negative Testing**: Error handling validation
- **Precision Testing**: Decimal calculation accuracy

## Skills Demonstrated

- Black-box testing methodology
- Test case design and documentation
- Equivalence Partitioning technique
- Boundary Value Analysis technique
- Requirements analysis and decomposition
- Test coverage analysis
- Professional QA documentation
- E-commerce domain knowledge
- Financial calculation validation

## Enhanced Version

The repository includes `part2-black-box-enhanced.md` with additional test scenarios:
- Extended edge case coverage
- Additional boundary conditions
- More comprehensive negative testing
- Enhanced requirement coverage scenarios

## Project Context

Developed for **COMP-2136: Software Quality Assurance** at George Brown College. This assignment demonstrates professional-level test analysis and documentation skills applicable to real-world software testing scenarios.

## Testing Principles Applied

1. **Systematic Coverage**: EP/BVA ensure comprehensive test coverage with minimal redundancy
2. **Requirement Traceability**: Every test case maps to specific requirements
3. **Realistic Scenarios**: Test data reflects real e-commerce usage patterns
4. **Edge Case Focus**: Boundaries and corner cases receive explicit attention
5. **Clear Documentation**: Enables test reproducibility and maintenance
