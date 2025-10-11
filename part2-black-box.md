## Black Box Part 2 Reasoning

### Why This Section Matters

1. **Foundation for Everything Else**  
  Your EP/BVA analysis and test cases inform the test plan (Part 1) and coverage scenarios (Part 3).
2. **Core of the Assignment**  
  Worth 60/100 points (25 for EP/BVA + 35 for test cases).
3. **Most Time-Consuming**  
  Requires detailed analysis and at least 15 test cases.
4. **Bottom-Up Approach**  
  Understanding the testing details makes writing the strategic overview (Part 1) easier.

---

## Requirements Analysis

Based on requirements (REQ-01 through REQ-06), we need to test:

- **Coupon Code** (REQ-02): Alphanumeric, 4-8 characters, case-insensitive.
- **Cart Subtotal** (REQ-04):  
  - `SAVE10` requires ≥ $50.00  
  - `5OFF` has no minimum

#### Test Conditions

- Valid and invalid coupon formats
- Minimum purchase enforcement
- Discount calculation (percentage vs. fixed)
- Edge cases (cart total can't go below $0)
- Only one coupon per order

---

## Equivalence Partitioning (EP) Analysis

EP divides inputs into groups (_partitions_) where all values in a partition should behave the same way. We test one value from each partition.

### Input 1: Coupon Code Format (REQ-02)

| Partition  | Description                           | Example(s)                    |
| ---------- | ------------------------------------- | ----------------------------- |
| **EP1-V1** | Alphanumeric, 4-8 chars, mixed case   | `SAVE10`, `Save10`, `SaVe10`  |
| **EP1-V2** | Alphanumeric, numeric-only, 4-8 chars | `12345678`, `2024`            |
| **EP1-I1** | Too short (<4 chars)                  | `ABC`, `12`, `X`              |
| **EP1-I2** | Too long (>8 chars)                   | `VERYLONGCODE`, `DISCOUNT123` |
| **EP1-I3** | Contains special characters           | `SAVE@10`, `5-OFF`, `CODE!`   |
| **EP1-I4** | Empty/null                            | `""`, `null`                  |
| **EP1-I5** | Only special chars/spaces             | `!@#$`, `"    "`, `---`       |
| **EP1-I6** | Leading/trailing whitespace           | `" SAVE10"`, `"5OFF "`        |

### Input 2: Coupon Code Type (REQ-03)

| Partition  | Description               | Example(s) |
| ---------- | ------------------------- | ---------- |
| **EP2-V1** | Valid percentage coupon   | `SAVE10`   |
| **EP2-V2** | Valid fixed amount coupon | `5OFF`     |
| **EP2-I1** | Code doesn't exist        | `FAKE99`   |

### Input 3: Cart Subtotal (REQ-04)

| Partition  | Description                      | Example(s)      |
| ---------- | -------------------------------- | --------------- |
| **EP3-V1** | Subtotal ≥ $50.00 (for SAVE10)   | $50.00, $87.42  |
| **EP3-I1** | Subtotal < $50.00 (for SAVE10)   | $49.99, $25.00  |
| **EP3-V2** | Any positive subtotal (for 5OFF) | $0.01, $12.34   |
| **EP3-I2** | Subtotal = $0.00 (for 5OFF)      | $0.00           |
| **EP3-V3** | Odd-cent cart values             | $87.33, $123.67 |

### Input 4: Discount Calculation (REQ-06)

| Partition  | Description                                | Example(s)              |
| ---------- | ------------------------------------------ | ----------------------- |
| **EP4-V1** | Discount leaves positive balance           | $87.42 - $8.74 = $78.68 |
| **EP4-V2** | Discount equals cart total                 | $5.00 - $5.00 = $0      |
| **EP4-I1** | Discount exceeds cart total (capped at $0) | $3.00 - $5.00 = $0      |

---

### EP Analysis Table

| Input                  | Equivalence Class                     | Valid/Invalid | Example(s)              |
| ---------------------- | ------------------------------------- | ------------- | ----------------------- |
| Coupon Code Format     | Alphanumeric, 4-8 chars, any case     | Valid         | `SAVE10`, `5OFF`        |
| Coupon Code Format     | Too short (<4 chars)                  | Invalid       | `ABC`, `12`             |
| Coupon Code Format     | Too long (>8 chars)                   | Invalid       | `VERYLONGCODE`          |
| Coupon Code Format     | Special characters                    | Invalid       | `SAVE@10`, `5-OFF`      |
| Coupon Code Format     | Empty/null                            | Invalid       | `""`, `null`            |
| Coupon Code Format     | Only special chars/spaces             | Invalid       | `!@#$`, `"    "`        |
| Coupon Code Format     | Leading/trailing whitespace           | Invalid       | `" SAVE10"`, `"5OFF "`  |
| Coupon Code Existence  | Exists in database (`SAVE10`, `5OFF`) | Valid         | `SAVE10`, `5OFF`        |
| Coupon Code Existence  | Does not exist                        | Invalid       | `FAKE99`, `NOTREAL`     |
| Cart Subtotal (SAVE10) | ≥ $50.00                              | Valid         | $87.42, $123.67         |
| Cart Subtotal (SAVE10) | < $50.00                              | Invalid       | $49.99, $25.00          |
| Cart Subtotal (5OFF)   | > $0.00                               | Valid         | $12.34, $7.89           |
| Cart Subtotal (5OFF)   | = $0.00                               | Invalid       | $0.00                   |
| Cart Subtotal          | Odd-cent values                       | Valid         | $87.33, $123.67         |
| Discount Impact        | Leaves positive balance               | Valid         | $87.42 - $8.74 = $78.68 |
| Discount Impact        | Equals cart total                     | Valid         | $5.00 - $5.00 = $0      |
| Discount Impact        | Exceeds cart total (capped at $0)     | Valid         | $3.00 - $5.00 = $0      |

---

## Boundary Value Analysis (BVA)

For BVA, we test the "edges" of valid ranges.

### Boundaries to Test

- **Coupon Code Length:**  
  - Just below: 3 chars  
  - Lower boundary: 4 chars  
  - Upper boundary: 8 chars  
  - Just above: 9 chars

- **Cart Subtotal for SAVE10:**  
  - Just below: $49.99  
  - On boundary: $50.00  
  - Just above: $50.01

- **Cart Subtotal for 5OFF:**  
  - On boundary: $0.00  
  - Just above: $0.01

- **Discount Calculation:**  
  - Discount = subtotal  
  - Discount > subtotal

### BVA Table

| Input                  | Boundary Value        | Valid/Invalid | Expected Behavior                             |
| ---------------------- | --------------------- | ------------- | --------------------------------------------- |
| Coupon Code Length     | 3 chars (`SAV`)       | Invalid       | Show "Invalid Coupon Code"                    |
| Coupon Code Length     | 4 chars (`SAVE`)      | Valid*        | Process coupon if code exists                 |
| Coupon Code Length     | 8 chars (`SAVE1234`)  | Valid*        | Process coupon if code exists                 |
| Coupon Code Length     | 9 chars (`SAVE12345`) | Invalid       | Show "Invalid Coupon Code"                    |
| Cart Subtotal (SAVE10) | $49.99                | Invalid       | Show "Invalid Coupon Code" (minimum not met)  |
| Cart Subtotal (SAVE10) | $50.00                | Valid         | Apply 10% discount, new total: $45.00         |
| Cart Subtotal (SAVE10) | $50.01                | Valid         | Apply 10% discount, new total: $45.01         |
| Cart Subtotal (5OFF)   | $0.00                 | Invalid       | Show "Invalid Coupon Code" (no items in cart) |
| Cart Subtotal (5OFF)   | $0.01                 | Valid         | Apply $5 discount, total capped at $0.00      |
| Cart Subtotal (5OFF)   | $5.00                 | Valid         | Apply $5 discount, new total: $0.00           |
| Cart Subtotal (5OFF)   | $5.01                 | Valid         | Apply $5 discount, new total: $0.01           |
| Discount Cap (5OFF)    | Cart: $3.00, $5 off   | Valid         | New total capped at $0.00 (not negative)      |
| Discount Cap (5OFF)    | Cart: $12.34, $5 off  | Valid         | New total: $7.34                              |

\*Valid format, but will show "Invalid Coupon Code" if the code doesn't exist.

---

## Test Cases

**23 test cases** covering positive, negative, and boundary scenarios, with realistic e-commerce values and explicit requirement coverage.

### Test Case Table

| TC-ID  | Req(s)      | Description                                                                                                                        | Steps                                                                                                 | Test Data                  | Expected Result                                                                                   |
|--------|-------------|------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|----------------------------|---------------------------------------------------------------------------------------------------|
| TC-001 | 03,04,05    | Verify 10% percentage discount (SAVE10) correctly applies to cart containing $87.42 when minimum $50 threshold is met              | 1. Checkout<br>2. Subtotal $87.42<br>3. Apply `SAVE10`                                                | $87.42, `SAVE10`           | Discount applied, total $78.68                                                                    |
| TC-002 | 03,04,05    | `SAVE10` rejected below minimum                                                                                                    | 1. Checkout<br>2. Subtotal $49.99<br>3. Apply `SAVE10`                                                | $49.99, `SAVE10`           | "Invalid Coupon Code", total $49.99                                                               |
| TC-003 | 03,04,05    | `SAVE10` applies at exact minimum                                                                                                  | 1. Checkout<br>2. Subtotal $50.00<br>3. Apply `SAVE10`                                                | $50.00, `SAVE10`           | Discount applied, total $45.00                                                                    |
| TC-004 | 03,04,05    | `SAVE10` applies just above minimum                                                                                                | 1. Checkout<br>2. Subtotal $50.01<br>3. Apply `SAVE10`                                                | $50.01, `SAVE10`           | Discount applied, total $45.01                                                                    |
| TC-005 | 03,04,05    | Verify $5 fixed amount discount (5OFF) applies to cart subtotal of $12.34 with no minimum purchase requirement                     | 1. Checkout<br>2. Subtotal $12.34<br>3. Apply `5OFF`                                                  | $12.34, `5OFF`             | Discount applied, total $7.34                                                                     |
| TC-006 | 03,06       | `5OFF` caps total at $0.00 if discount > subtotal                                                                                  | 1. Checkout<br>2. Subtotal $3.00<br>3. Apply `5OFF`                                                   | $3.00, `5OFF`              | Discount applied, total $0.00                                                                     |
| TC-007 | 03,06       | `5OFF` reduces total to $0.00 when discount = subtotal                                                                             | 1. Checkout<br>2. Subtotal $5.00<br>3. Apply `5OFF`                                                   | $5.00, `5OFF`              | Discount applied, total $0.00                                                                     |
| TC-008 | 02,05       | Coupon code <4 chars rejected                                                                                                      | 1. Checkout<br>2. Subtotal $87.42<br>3. Apply `ABC`                                                   | $87.42, `ABC`              | "Invalid Coupon Code", total $87.42                                                               |
| TC-009 | 02,05       | Coupon code 4 chars accepted (lower boundary)                                                                                      | 1. Checkout<br>2. Subtotal $91.27<br>3. Apply `5OFF`                                                  | $91.27, `5OFF`             | Discount applied, total $86.27                                                                    |
| TC-010 | 02,05       | Coupon code >8 chars rejected                                                                                                      | 1. Checkout<br>2. Subtotal $123.67<br>3. Apply `VERYLONGCODE`                                         | $123.67, `VERYLONGCODE`    | "Invalid Coupon Code", total $123.67                                                              |
| TC-011 | 02,05       | Coupon code with special chars rejected                                                                                            | 1. Checkout<br>2. Subtotal $42.50<br>3. Apply `SAVE@10`                                               | $42.50, `SAVE@10`          | "Invalid Coupon Code", total $42.50                                                               |
| TC-012 | 02,05       | Empty coupon code rejected                                                                                                         | 1. Checkout<br>2. Subtotal $87.33<br>3. Leave code empty                                              | $87.33, (empty)            | "Invalid Coupon Code", total $87.33                                                               |
| TC-013 | 02,03,05    | Case-insensitive validation for `SAVE10`                                                                                           | 1. Checkout<br>2. Subtotal $87.42<br>3. Apply `save10`                                                | $87.42, `save10`           | Discount applied, total $78.68                                                                    |
| TC-014 | 02,03,05    | Case-insensitive validation for `5OFF` (mixed case)                                                                                | 1. Checkout<br>2. Subtotal $91.27<br>3. Apply `5oFf`                                                  | $91.27, `5oFf`             | Discount applied, total $86.27                                                                    |
| TC-015 | 05          | Non-existent coupon code rejected                                                                                                  | 1. Checkout<br>2. Subtotal $87.42<br>3. Apply `FAKE99`                                                | $87.42, `FAKE99`           | "Invalid Coupon Code", total $87.42                                                               |
| TC-016 | 01,05       | Coupon input field present and visible                                                                                             | 1. Checkout<br>2. Observe input field                                                                 | N/A                        | Input field visible and enabled                                                                   |
| TC-017 | 03,04       | Verify SAVE10 percentage discount calculation accuracy on high-value cart ($387.50) to ensure proper decimal precision             | 1. Checkout<br>2. Subtotal $387.50<br>3. Apply `SAVE10`                                               | $387.50, `SAVE10`          | Discount applied, total $348.75                                                                   |
| TC-018 | 03          | `5OFF` with subtotal just above $5.00                                                                                              | 1. Checkout<br>2. Subtotal $5.01<br>3. Apply `5OFF`                                                   | $5.01, `5OFF`              | Discount applied, total $0.01                                                                     |
| TC-019 | 02,05       | Verify 8-character code at upper boundary is accepted (but not in database)                                                        | 1. Checkout<br>2. Subtotal $123.67<br>3. Apply `DISCOUNT`                                             | $123.67, `DISCOUNT`        | "Invalid Coupon Code" (format valid, but doesn't exist)                                           |
| TC-020 | 02,03,06    | Verify only one coupon can be applied per order (REQ-06)                                                                           | 1. Checkout<br>2. Subtotal $87.42<br>3. Apply `SAVE10`<br>4. Attempt to apply `5OFF`                  | $87.42, `SAVE10`, `5OFF`   | System rejects second coupon or replaces first coupon. Appropriate error shown.                   |
| TC-021 | 02,03,05    | Verify system handles leading/trailing whitespace                                                                                  | 1. Checkout<br>2. Subtotal $75.00<br>3. Apply `" SAVE10 "` (with spaces)                              | $75.00, `" SAVE10 "`       | System trims spaces and accepts OR rejects as invalid format                                      |
| TC-022 | 02,05       | Verify numeric-only code with valid length is rejected                                                                             | 1. Checkout<br>2. Subtotal $60.00<br>3. Apply `12345678`                                              | $60.00, `12345678`         | "Invalid Coupon Code" (format valid but doesn't exist in database)                                |
| TC-023 | 03,04,05    | Verify percentage discount with odd dollar amount                                                                                  | 1. Checkout<br>2. Subtotal $87.33<br>3. Apply `SAVE10`<br>4. Verify discount calculation precision    | $87.33, `SAVE10`           | "Discount Applied!" New total: $78.60 (rounded to 2 decimal places)                               |

### Coverage Summary

- **Positive scenarios:** TC-001, TC-003, TC-004, TC-005, TC-007, TC-009, TC-013, TC-014, TC-016, TC-017, TC-018, TC-021, TC-023
- **Negative scenarios:** TC-002, TC-008, TC-010, TC-011, TC-012, TC-015, TC-019, TC-020, TC-022
- **Boundary tests:** TC-003, TC-004, TC-006, TC-007, TC-009, TC-018, TC-019, TC-021
- **All requirements covered:** REQ-01 through REQ-06
- **REQ-06 ("one coupon per order") explicitly tested in TC-020**
- **Odd-cent and decimal precision tested in TC-023**
- **Whitespace handling tested in TC-021**

