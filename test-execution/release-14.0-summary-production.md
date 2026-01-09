# Release 14.0 Bug and Test Summary

**Release Milestone:** Release 14.0  
**Version: v1.26.1.8
**Date:** 2026-01-09
**Test Lead:** @yourusername  

## Bug Summary

| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
| #101   | TC-015       | High     | Payment| Closed   | @dev1       | Payment gateway timeout |
| #50   | TC-029       | Medium   | Confirmation email not received  | Open | @dev2       | Users completing either the Demo Scheduling or Request Invitation forms are shown a success confirmation screen. However, no email is received for either action.|
| #65   | TC-032       | Medium   | Incorrect or Delayed Bookmark Query Loading  | Closed | @dev2       | When a user opens the Bookmarked Queries section and clicks on any saved bookmark, the system does not immediately open the correct bookmarked query.  |
| #64   | TC-033       | Medium   | AI Response Interrupted When Navigating to Session History  | Closed | @dev2       | When a user initiates a query and then navigates to Session History before the AI completes its response, the AI stops generating output.  |
| #63   | TC-034       | Medium   | No Option to Stop Query Execution  | Closed | @dev2       | When a user enters a query, and the system begins generating a response, there is no visible option to stop or cancel the query once it has started. |
| #68   | TC-035       |  Minor    | The Welcome Tour is not visible in Firefox| Closed   | @dev1       | The Welcome Tour does not appear when accessing the application through Firefox |
| #69   | TC-036       | Medium     | Page Refreshes After Stopping Query| Open   | @dev1       | When a user stops an active query and selects another query, the entire page refreshes automatically, interrupting the workflow. |
| #70   | TC-037       | Medium     | Jurisdiction & Adopted Codes Not Visible After Navigation| Closed   | @dev1       | After opening Bookmarked Queries or Session History and then navigating back to the main page, the Jurisdiction and Adopted Codes sections do not appear. |
| #71   | TC-038       | Medium     | Menu Options Not Visible on Mobile | Closed   | @dev1       | On mobile devices, after clicking the menu icon, Code & Preferences, Session History, and Collaboration options are not visible. |
| #72   | TC-039       | Minor     | Stream reading error| Open   | @dev1       | When the user searches for any query, the system shows a “stream reading error” and fails to load the AI response properly. |
| #73   | TC-040       | Medium     | Jurisdiction Missing After Navigation| Closed   | @dev1       | After navigating away and coming back to the main page, the selected Jurisdiction is missing / not displayed. |
| #74   | TC-041       | High     | “Response Preference” During AI Response Causes Page Refresh | Close   | @dev1       | When the user searches a query, and the AI is generating a response, changing the Response Preference (7 available options) during the response sometimes refreshes the entire page unexpectedly. |
| #77   | TC-042       | Medium  | Skip, Back, and Next buttons malfunction in Welcome Tour. | Closed | @dev1 |  Welcome Tour |
| #78   | TC-043       | Medium  | Plan Downgrade for Active Tenant (B2B) | Closed | @dev1 | Downgrade applies immediately; excess queries fail with upgrade prompt; |
| #79   | TC-044       | Medium  | Halfway Query Interrupt (Queries Check) (B2C)|Open  | @dev1 | The interrupted query does not deduct a credit; the quota remains unchanged. |
| #80   | TC-045       | Medium  | Degrade Plan (premium to starter) (B2C)|Closed  | @dev1 | Plan status shows will change to starter after 1month ,no change occurs. |
| #86   | TC-046       | High  | Tenant Not Displayed in URL for B2B| Open | @dev1 |  Tenant is not visible in the URL in the B2B application; manual tenant entry in the URL is required to make it work|
| #85   | TC-047       | High  | Concurrent Login on Different Browsers – Query Detected Only Once| Open | @dev1 |  Query is detected only once when the same B2C, B2B account is used simultaneously on different browsers|
| #87   | TC-048       | Medium  | Bookmarked Queries Filter | Open | @dev1 |The filter functionality in the Bookmarked Queries section is not working. When a user applies any filter option, the bookmarked queries list does not update or change accordingly. |
| #88   | TC-049       | Medium  | Session History Filter | Open | @dev1 |When a user filters by a date range that has no corresponding sessions, it does not filter out the existing sessions. |
| #89   | TC-050       | Medium  | Session History Filter is not working | Open | @dev1 |The filter in the Session History section is inaccurate. When a user selects a specific start and end date, the session list does not update to show only the relevant queries specific to that date. |


## Test Execution Overview

| Test Status | Count | Percentage |
|-------------|-------|------------|
| Passed      | 90    | 90%        |
| Failed      | 10    | 10%        |
| Blocked     | 0     | 0%         |

## Key Insights and Recommendations

- Prioritize fixing high-priority bugs before release.
- Follow up on blocked test cases.
