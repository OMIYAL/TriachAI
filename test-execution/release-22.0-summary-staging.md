# Release 22.0 Bug and Test Summary

**Release Milestone:** Release 22.0  
**Version**: v1.26.2.13
**Date:** 2026-02-14
**Test Lead:** @yourusername  

## Bug Summary

| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
| #50   | TC-029       | Medium   | Confirmation email not received  | Open | @dev2       | Users completing either the Demo Scheduling or Request Invitation forms are shown a success confirmation screen. However, no email is received for either action.|
| #69   | TC-036       | Medium     | Page Refreshes After Stopping Query| Close  | @dev1       | When a user stops an active query and selects another query, the entire page refreshes automatically, interrupting the workflow. |
| #72   | TC-039       | Minor     | Stream reading error| Open   | @dev1       | When the user searches for any query, the system shows a “stream reading error” and fails to load the AI response properly. |
| #79   | TC-044       | Medium  | Halfway Query Interrupt (Queries Check) (B2C)|Open  | @dev1 | The interrupted query does not deduct a credit; the quota remains unchanged. |
| #91   | TC-052       | Medium  | Developer Portal Open | Close | @dev1 |After payment failure, clicking the back button redirects to the Developer Portal page |
| #92   | TC-053       | Medium  | Remember me  | Open | @dev1 |Remember me - when you slide it, the circle within the bar disappears.|
| #100   | TC-054       | Medium  | Developer Mode Opens After Logout from External Login Page  | Close | @dev1 |When a user logs in via External Login and navigates to the external authentication page, clicking Logout unexpectedly opens Developer Mode.|
| #102   | TC-055       | Medium  | External login |Close | @dev1 |When a user logs in via External Login.|
| #103   | TC-056       | Medium  | QueryCounter  | Close | @dev1 |QueryCounter|
| #104   | TC-057       | Medium  | Tenant Name Below QueryCounter  | Open | @dev1 |Tenant Name Below QueryCounter|
| #105   | TC-058       | Medium  | Preimum highlight feature  | Close | @dev1 |Preimum highlight feature|
| #106   | TC-059       | Medium  | In Mobile its working fine, but now the overlap of text is in Desktop.  | Close | @dev1 |In Mobile its working fine, but now the overlap of text is in Desktop.|
| #107   | TC-060       | Medium  | the jurisdiction issue and not in alphabet order  | Close | @dev1 |the jurisdiction issue and not in alphabet order|
| #108   | TC-061       | Medium  | the payment history feature in the My Account tab below the subscription.  | Close | @dev1 |the payment history feature in the my account tab below subscription.|
| #109   | TC-062       | Medium  | Recommended Follow-Up Questions Not Displaying in AHJ   | Close | @dev1 |Recommended Follow-Up Questions Not Displaying in AHJ |
| #110   | TC-063       | Medium  | Missing Space in “Session History” Text  | Close | @dev1 |Missing Space in “Session History” Text|
| #111   | TC-064       | Medium  | Email Notification Not Sent on Tenant User Creation(b2b)  | Open | @dev1 |Email Notification Not Sent on Tenant User Creation|
| #112   | TC-065       | Medium  | Verification Emails Delivered to Spam Folder(b2b) | Open | @dev1 |Verification Emails Delivered to Spam Folder|
| #115   | TC-066       | Medium  | Sessions Not Displayed After Applying Jurisdiction Filter on Page 2 | Close | @dev1 |Session Filter Fails When Jurisdiction Is Selected After Navigating to Page 2 |
| #113   | TC-067       | Medium  | Search Text Display Issue on Mobile & Desktop | Close | @dev1 |Search text should be clearly visible, properly aligned, and fully readable on both mobile and desktop screens.|
| #116   | TC-068       | Medium  | Bookmarked Queries Not Displayed After Applying Jurisdiction Filter on Page 2 | Close | @dev1 |Bookmarked Queries Filter Fails When Jurisdiction Is Selected After Navigating to Page 2|
| #117   | TC-069       | Medium  | Menu and “+” Icon Not Displayed Properly on Mobile | Open | @dev1 |Menu and “+” Icon UI Misalignment / Not Visible on Mobile View|
| #114   | TC-067       | Medium  | Real-Time Query Counter Not Updating| Close | @dev1 |Real-Time Query Counter Does Not Update Automatically(B2B, B2C)|




## Test Execution Overview

| Test Status | Count | Percentage |
|-------------|-------|------------|
| Passed      | 80    | 80%        |
| Failed      | 20    | 20%        |
| Blocked     | 0     | 0%         |

## Key Insights and Recommendations

- Prioritize fixing high-priority bugs before release.
- Follow up on blocked test cases.
