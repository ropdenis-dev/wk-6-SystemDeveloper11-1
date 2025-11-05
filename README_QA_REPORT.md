# CleanCity QA Testing Report

## 🧭 Project Overview
**CleanCity: Waste Pickup Scheduler** is a React-based web application that simulates a waste management system for African cities.  
It allows users to:
- Request waste pickups  
- View and filter requests on the dashboard  
- Report missed pickups via the feedback page  
- Read awareness content about proper waste management  
- Use the Admin Panel to manage and update pickup requests  

The system includes **intentional bugs** and **validation flaws** for QA testing practice.

---

## Test Objectives
The purpose of this QA project is to:
- Validate form inputs and data submission behavior  
- Identify UI/UX and accessibility issues  
- Test filtering, sorting, and data persistence  
- Check responsiveness across devices  
- Detect boundary and regression issues  
- Implement automated testing using **React Testing Library**

---

## Testing Environment

| Component | Details |
|------------|----------|
| **Operating System** | Windows 10 (64-bit) |
| **Browser** | Google Chrome v129 |
| **Framework** | React 18.2.0 |
| **Testing Tools** | React Testing Library, Cypress, Lighthouse, Chrome DevTools |
| **Node.js Version** | 18.x |
| **Storage** | localStorage (no backend) |
| **Devices Tested** | Laptop (1366x768), Mobile Emulator (Pixel 5) |

---

## Manual Test Cases & Results

| **Test ID** | **Test Case Description** | **Steps** | **Expected Result** | **Actual Result** | **Status** |
|--------------|---------------------------|------------|----------------------|-------------------|-------------|
| TC001 | Submit form without required fields | Go to Home → Leave all inputs empty → Click Submit | Validation errors for all fields | Date field doesn’t show an error | ❌ Fail |
| TC002 | Submit valid waste pickup form | Fill all inputs correctly → Submit | Form submitted successfully → Success message appears | Works correctly | ✅ Pass |
| TC003 | Filter requests by location (Eldoret) | Open Dashboard → Apply filter “Eldoret” | Should show only Eldoret requests | Nairobi requests also appear | ❌ Fail |
| TC004 | Update status in Admin Panel | Open Admin → Mark a request as “Scheduled” | UI updates instantly | UI doesn’t refresh (needs reload) | ❌ Fail |
| TC005 | Enter long input text | Input 500+ characters in form fields | Layout remains intact | Layout breaks slightly | ⚠️ Partial |
| TC006 | Responsive design | Resize browser / open on mobile emulator | Layout adjusts correctly | Works mostly fine | ✅ Pass |
| TC007 | Feedback submission | Go to Feedback → Enter valid Request ID → Submit | Success message displayed | Works correctly | ✅ Pass |

---

## Bugs / Defects Found

| **Bug ID** | **Description** | **Severity** | **Page/Feature** | **Status** |
|-------------|----------------|---------------|------------------|-------------|
| BUG-001 | Missing validation on date field | Medium | Home Form | Open |
| BUG-002 | Filter by location displays wrong data | High | Dashboard | Open |
| BUG-003 | Admin status update doesn’t refresh UI | High | Admin Panel | Open |
| BUG-004 | Images missing `alt` text | Medium | Awareness Page | Open |
| BUG-005 | Layout breaks with long text inputs | Low | Forms | Open |

---

## Accessibility & UI Findings

| **Issue** | **Description** | **Severity** |
|------------|-----------------|---------------|
| Missing `alt` attributes | Awareness images lack alternative text | Medium |
| Low contrast text in buttons | Hard to read under dark mode | Low |
| Non-descriptive button labels | Buttons like “Submit” should specify context | Low |
| No ARIA roles in tables | Tables lack roles for screen readers | Medium |

---

## Boundary & Performance Observations
- Long text inputs cause layout stretching.  
- Filtering large datasets leads to slower rendering.  
- Application reloads needed after some admin actions.  
- Mobile view slightly overlaps table headers.  
- LocalStorage persistence works but lacks data cleanup.

---

## Automated Testing

### Using React Testing Library

**Sample test for form validation:**
```javascript
import { render, screen, fireEvent } from "@testing-library/react";
import App from "../App";

test("shows error when submitting empty form", () => {
  render(<App />);
  const submitButton = screen.getByText(/submit/i);
  fireEvent.click(submitButton);
  expect(screen.getByText(/please fill all fields/i)).toBeInTheDocument();
});
