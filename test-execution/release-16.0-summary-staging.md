# Release 16.0 Bug and Test Summary

**Release Milestone:** Release 16.0  
**Version**: v1.26.1.26
**Date:** 2026-01-029
**Test Lead:** @yourusername  

## Bug Summary

| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
| #50   | TC-029       | Medium   | Confirmation email not received  | Open | @dev2       | Users completing either the Demo Scheduling or Request Invitation forms are shown a success confirmation screen. However, no email is received for either action.|
| #69   | TC-036       | Medium     | Page Refreshes After Stopping Query| Open   | @dev1       | When a user stops an active query and selects another query, the entire page refreshes automatically, interrupting the workflow. |
| #72   | TC-039       | Minor     | Stream reading error| Open   | @dev1       | When the user searches for any query, the system shows a “stream reading error” and fails to load the AI response properly. |
| #79   | TC-044       | Medium  | Halfway Query Interrupt (Queries Check) (B2C)|Open  | @dev1 | The interrupted query does not deduct a credit; the quota remains unchanged. |
| #87   | TC-048       | Medium  | Bookmarked Queries Filter | Close | @dev1 |The filter functionality in the Bookmarked Queries section is not working. When a user applies any filter option, the bookmarked queries list does not update or change accordingly. |
| #88   | TC-049       | Medium  | Session History Filter | Close | @dev1 |When a user filters by a date range that has no corresponding sessions, it does not filter out the existing sessions. |
| #89   | TC-050       | Medium  | Session History Filter is not working | Open | @dev1 |The filter in the Session History section is inaccurate. When a user selects a specific start and end date, the session list does not update to show only the relevant queries specific to that date. |
| #90   | TC-051       | Medium  | Welcome tour issue | Close | @dev1 |In the Welcome Tour, after logging in, the tour is not visible after the 4th step. After the user progresses through the first 4 steps, clicking the Next button causes the tour navigation (Next/Back buttons) to disappear, with the user stuck without a way to proceed or exit. |
| #91   | TC-052       | Medium  | Developer Portal Open | Open | @dev1 |After payment failure, clicking the back button redirects to the Developer Portal page |
| #92   | TC-053       | Medium  | Remember me  | Open | @dev1 |Remember me - when you slide it, the circle within the bar disappears.|
| #100   | TC-054       | Medium  | Developer Mode Opens After Logout from External Login Page  | Open | @dev1 |When a user logs in via External Login and navigates to the external authentication page, clicking Logout unexpectedly opens Developer Mode.|
| #102   | TC-055       | Medium  | External login  | Open | @dev1 |When a user logs in via External Login.|
| #103   | TC-056       | Medium  | QueryCounter  | Open | @dev1 |QueryCounter|
| #104   | TC-057       | Medium  | Tenant Name Below QueryCounter  | Open | @dev1 |Tenant Name Below QueryCounter|
| #105   | TC-058       | Medium  | Preimum highlight feature  | Open | @dev1 |Preimum highlight feature|
| #106   | TC-059       | Medium  | In Mobile its working fine, but now the overlap of text is in Desktop.  | Open | @dev1 |In Mobile its working fine, but now the overlap of text is in Desktop.|
| #107   | TC-060       | Medium  | the jurisdiction issue and not in alphabet order  | Open | @dev1 |the jurisdiction issue and not in alphabet order|
| #108   | TC-061       | Medium  | the payment history feature in the my account tab below the subscription.  | Open | @dev1 |the payment history feature in the my account tab below subscription.|
| #109   | TC-062       | Medium  | Recommended Follow-Up Questions Not Displaying in AHJ   | Open | @dev1 |Recommended Follow-Up Questions Not Displaying in AHJ |
| #110   | TC-063       | Medium  | Missing Space in “Session History” Text  | Open | @dev1 |Missing Space in “Session History” Text|
| #111   | TC-064       | Medium  | Email Notification Not Sent on Tenant User Creation(b2b)  | Open | @dev1 |Email Notification Not Sent on Tenant User Creation|
| #112   | TC-065       | Medium  | Verification Emails Delivered to Spam Folder(b2b) | Open | @dev1 |Verification Emails Delivered to Spam Folder|






## Test Execution Overview

| Test Status | Count | Percentage |
|-------------|-------|------------|
| Passed      | 80    | 80%        |
| Failed      | 20    | 20%        |
| Blocked     | 0     | 0%         |

## Key Insights and Recommendations

- Prioritize fixing high-priority bugs before release.
- Follow up on blocked test cases.
