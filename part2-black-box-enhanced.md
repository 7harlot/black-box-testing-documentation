# Part 2: Black-Box Test Case Design - ENHANCED VERSION

## Improvements Made for Uniqueness:

### 1. Enhanced EP Analysis with Additional Partitions
- Added EP1-V2 for numeric-only codes
- Added EP1-I6 for whitespace handling
- Added EP3-V3 for odd-cent cart values
- More diverse and realistic examples

### 2. Additional Unique Test Cases (TC-019 through TC-023)

| TC-ID  | Req(s)   | Description                                            | Test Steps                                                                                                                                            | Test Data                                                         | Expected Result                                                                           |
| ------ | -------- | ------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| TC-019 | 02,05    | Verify 8-character code at upper boundary is accepted  | 1. Navigate to checkout page<br>2. Ensure cart subtotal is $123.67<br>3. Enter coupon code in input field<br>4. Click "Apply"                         | Cart Subtotal: $123.67<br>Coupon Code: DISCOUNT                   | "Invalid Coupon Code" (format valid, but doesn't exist)                                   |
| TC-020 | 02,03,06 | Verify only one coupon can be applied per order        | 1. Navigate to checkout page<br>2. Ensure cart subtotal is $100.00<br>3. Apply SAVE10 successfully<br>4. Attempt to apply 5OFF<br>5. Observe behavior | Cart Subtotal: $100.00<br>First Code: SAVE10<br>Second Code: 5OFF | System rejects second coupon or replaces first coupon. Display appropriate error message. |
| TC-021 | 02,03,05 | Verify system handles leading/trailing whitespace      | 1. Navigate to checkout page<br>2. Ensure cart subtotal is $75.00<br>3. Enter " SAVE10 " (with spaces)<br>4. Click "Apply"                            | Cart Subtotal: $75.00<br>Coupon Code: " SAVE10 "                  | System either trims spaces and accepts OR rejects as invalid format                       |
| TC-022 | 02,05    | Verify numeric-only code with valid length is rejected | 1. Navigate to checkout page<br>2. Ensure cart subtotal is $60.00<br>3. Enter 8-digit numeric code<br>4. Click "Apply"                                | Cart Subtotal: $60.00<br>Coupon Code: 12345678                    | "Invalid Coupon Code" (format valid but doesn't exist in database)                        |
| TC-023 | 03,04,05 | Verify percentage discount with odd dollar amount      | 1. Navigate to checkout page<br>2. Ensure cart subtotal is $87.33<br>3. Enter SAVE10<br>4. Click "Apply"<br>5. Verify discount calculation precision  | Cart Subtotal: $87.33<br>Coupon Code: SAVE10                      | "Discount Applied!" New total: $78.60 (rounded to 2 decimal places)                       |

### 3. More Specific Test Data Values

**Replace generic values with realistic e-commerce amounts:**
- Instead of $100.00, use $87.42, $123.67, $91.27
- Instead of $10.00, use $7.89, $12.34, $42.50
- This makes the submission feel more authentic and less template-based

### 4. Enhanced Test Case Descriptions

**TC-001 (Enhanced):**
- Old: "SAVE10 applies when subtotal meets minimum"
- New: "Verify 10% percentage discount (SAVE10) correctly applies to cart containing $87.42 worth of items when minimum $50 threshold is met"

**TC-005 (Enhanced):**
- Old: "5OFF applies, no minimum"
- New: "Verify $5 fixed amount discount (5OFF) applies to cart subtotal of $12.34 with no minimum purchase requirement enforced"

**TC-017 (Enhanced):**
- Old: "SAVE10 with large subtotal calculates correctly"
- New: "Verify SAVE10 percentage discount calculation accuracy on high-value cart ($387.50) to ensure proper decimal precision"

### 5. Update Numbers in Existing Test Cases

Suggest replacing these generic values:
- TC-001: $100.00 → $87.42 (result: $78.68)
- TC-005: $10.00 → $12.34 (result: $7.34)
- TC-017: $500.00 → $387.50 (result: $348.75)

This creates more realistic test data that stands out from standard round numbers.

---

## Recommended Action Items:

1. ✅ Update EP table Input 3 with realistic dollar amounts
2. ✅ Add TC-019 through TC-023 to your test case table
3. ⚠️ Expand "Steps (Summary)" column to full numbered steps for final PDF
4. ⚠️ Update existing TC test data with non-round numbers (optional but recommended)
5. ⚠️ Add note explaining you're testing REQ-06 "one coupon per order" with TC-020

---

## Why These Changes Matter:

- **Demonstrates deeper analysis**: Shows you thought beyond obvious test cases
- **Realistic scenarios**: E-commerce carts rarely have perfect round numbers
- **Better requirements coverage**: TC-020 explicitly tests REQ-06
- **Edge case awareness**: Whitespace handling (TC-021) shows attention to detail
- **Precision testing**: TC-023 tests decimal rounding, which classmates might miss

Your submission will now stand out as more thoughtful and comprehensive than generic template-based submissions!
