# Pass‐All‐Tests Checklist for Wine & Cheese Discovery App

Link to the actual application: [Wine & Cheese Discovery App](https://projectintermarche.bubbleapps.io/version-test/index)

Spreadsheet for test cases: [Test Cases Spreadsheet](https://docs.google.com/spreadsheets/d/1Uc4m3wUfS8h6ZmRr6mdvs4oUeVg59lZ_E5GDCVahomQ/edit?usp=sharing)

## FTR-001: Landing – Pick a Language

1. **Language Card Rendering**
   - All supported language cards (e.g., English, Français, Español, etc.) render in under 2 seconds.
   - Each card shows the correct flag image, language name, and has `aria-label="Language: <Name>, button"`.
2. **Selection Behavior**
   - Clicking or pressing Enter on any card:
     - Switches the app locale to that language.
     - Redirects immediately to `/landing`.
     - All UI labels on the landing page appear in the chosen language.
3. **Direct URL Protection**
   - Navigating to `/landing` without a session locale cookie must redirect back to `/` (Pick a Language).
4. **Accessibility**
   - Screen reader announces each card as “Language: <Name>, button.”
   - Keyboard users can Tab to each card and press Enter to select.
5. **Performance**
   - The entire “Pick a Language” screen (including flag images) loads in ≤ 2 seconds on both desktop and mobile.

---

## FTR-002: Landing Page – Search & Filters

1. **Toggle Wine/Cheese Context**
   - Default state is “Wine.”
   - Clicking “Cheese” toggles to Cheese mode:
     - “Cheese” appears visually active (plus `aria-pressed="true"`).
     - Search placeholder changes to “Search cheeses…” (localized).
     - Filter options (Price, Type, Region, etc.) update to cheese-specific values.
2. **Fuzzy Search**
   - Typing ≥ 1 character triggers a debounced lookup (≈ 300 ms).
   - Displays up to 10 matching suggestions (wine or cheese names, depending on mode).
   - Clicking a suggestion navigates to `/wine/<slug>` or `/cheese/<slug>`.
   - The Visualizer page must load in ≤ 1 second and display correct product details.
3. **Collapse Suggestions**
   - Clicking outside the suggestions dropdown closes it, leaving the input text unchanged.
4. **Filters Toggle**
   - Clicking “Filters” expands a second row of controls (Price, Type, Region, Alcohol).
   - Clicking “Filters” again collapses that row, leaving only first-line toggles visible.
5. **Checkbox Toggles**
   - In Cheese mode: checking “Lactose-Free” immediately filters featured (top 3) to lactose-free cheeses.
   - In Wine mode: checking “Sulfite-Free” and/or “Alcohol-Free” immediately filters featured wines.
   - The URL/querystring must update, for example:  
     ```
     ?filter[lactose_free]=true
     ```
6. **No-Result Searches**
   - Typing a string that matches nothing (e.g., “xyz123”):
     - Suggestion dropdown shows “No results found.”
     - Pressing Enter (submitting the search) displays “No results found” in the main area plus a “Clear Filters” link.
7. **Max-Length Enforcement**
   - Input is limited to 128 characters.
   - Pasting > 128 characters truncates to exactly 128.
   - Once the limit is reached, show a tooltip or inline message:  
     ```
     Max 128 characters allowed
     ```
8. **Accessibility**
   - Every checkbox has `role="checkbox"` with the correct `aria-checked` state.
   - Screen readers announce “<Filter Name>, checkbox, not checked” or “checked.”
   - Keyboard (Tab + Space/Enter) toggles each checkbox state.

---

## FTR-003: Price Pop-Up

1. **Default Values**
   - Clicking “Price” opens a modal/sliding pop-up.
   - Two slider thumbs start at 0 (Min) and 100 (Max).
   - Numeric inputs below show “0” (Min) and “100” (Max).
2. **Dragging Thumbs**
   - Dragging the left thumb to 10 instantly updates the Min input to 10.
   - Dragging the right thumb to 80 instantly updates the Max input to 80.
   - Underlying state becomes `price_min=10` and `price_max=80`.
3. **Typing Values**
   - Typing “20” in Min and pressing Enter moves the left thumb to 20.
   - Typing “80” in Max and pressing Enter moves the right thumb to 80.
4. **Click-Outside Behavior**
   - Clicking outside the modal when values are valid:
     - Closes the modal.
     - Applies filters:  
       ```
       ?filter[price_min]=20&filter[price_max]=80
       ```
5. **Validation**
   - Entering “150” in Min when Max=100:
     - Displays “Min cannot exceed Max.”
     - Thumb remains at 100; filter state does not change.
   - Entering “-5” in Min:
     - Displays “Price cannot be negative.”
     - Thumb stays at previous valid value (0).
   - Attempting to drag Min beyond Max is blocked; show “Min cannot exceed Max.”
6. **Keyboard Accessibility**
   - Left thumb has keyboard focus support:
     - Arrow Right increments by 1.
     - Numeric field and thumb update in sync.
     - `aria-valuenow` always matches the thumb position.
7. **Performance**
   - Opening the Price modal completes in ≤ 300 ms.

---

## FTR-004: Type Pop-Up

1. **Rendering**
   - Clicking “Type” opens a scrollable modal listing all wine or cheese types.
   - At least 5 items are visible without scrolling; a scrollbar indicates more.
2. **Selecting Types**
   - Checking “Cabernet” and “Merlot” then clicking outside:
     - Closes the modal.
     - Applies the filter:  
       ```
       ?filter[type]=Cabernet,Merlot
       ```
   - If no checkboxes are selected and the user clicks outside:
     - Modal closes.
     - No change to filters.
3. **Scrolling**
   - If there are ≥ 6 types, scrolling reveals additional items beyond the first 5.
4. **Accessibility**
   - Screen reader announces “Checkbox: Cabernet, not checked.”
   - Pressing Space toggles each checkbox and updates `aria-checked`.

---

## FTR-005: Region Pop-Up

1. **Dropdown Behavior**
   - Clicking “Region” opens a modal with a collapsed dropdown labeled “Select Region.”
2. **Selecting Regions**
   - Clicking the dropdown arrow expands an alphabetical list of regions.
   - Selecting “Bordeaux” closes the modal and applies:  
     ```
     ?filter[region]=Bordeaux
     ```
3. **Click-Outside Persistence**
   - If “Bordeaux” was already selected, opening and clicking outside (while collapsed):
     - Closes modal.
     - Keeps “Bordeaux” as the applied filter.
4. **Search Within Dropdown**
   - If > 20 regions exist, typing “Prove” filters to “Provence-Alpes-Côtes d’Azur,” etc.
5. **Keyboard Accessibility**
   - Tab → Enter to expand.
   - Arrow Down/Up to navigate.
   - Enter on “Champagne” applies `?filter[region]=Champagne` and closes modal.

---

## FTR-006: Alcohol Content Pop-Up

1. **Default Values**
   - Clicking “Alcohol Content” opens a modal.
   - Two slider thumbs are at 0 % (Min) and 18 % (Max).
   - Numeric inputs display “0%” and “18%.”
2. **Dragging Thumbs**
   - Drag left thumb to ~5 % → Min input updates to “5%” and state becomes `?filter[alcohol_min]=5`.
   - Drag right thumb to ~12 % → Max input updates to “12%” and state becomes `?filter[alcohol_max]=12`.
3. **Validation**
   - Typing “-1” in Min:
     - Displays “Value must be between 0% and 18%.”
     - Thumb remains at 0 %.
   - Attempting Min > Max (e.g., Min=15, Max=12):
     - Blocks the move.
     - Displays “Min cannot exceed Max.”
4. **Click-Outside**
   - Clicking outside closes modal and applies:  
     ```
     ?filter[alcohol_min]=5&filter[alcohol_max]=12
     ```
5. **Keyboard Accessibility**
   - Tab to left thumb; Arrow Right increments by 0.5 % each press.
   - `aria-valuenow` always matches the current thumb value.

---

## FTR-007: Cheese Selection Form (Stub)

1. **Navigation**
   - In Cheese mode, clicking “Wide Cheese” navigates to a stub page:
     - Header: “Cheese Selection Form”
     - Back arrow leads back to Landing (Cheese mode).
2. **Back Arrow**
   - Clicking Back returns to Landing (Cheese) and preserves any prior search/filter state.
3. **Offline/Error Handling**
   - If the network is offline (fetch fails), clicking “Wide Cheese” shows “Unable to load form. Retry” (no crash).
   - A visible “Retry” button re-attempts to load when clicked.
4. **Accessibility**
   - Screen reader announces “Back, button,” followed by “Cheese Selection Form.”

---

## FTR-008: Wine Selection Form & Sub-Pages

### 4.8.1 FTR-008A: Wine Selection Form Root

1. **Rendering**
   - Clicking “Wide Wine” loads the Form Root within 1 sec.
   - Displays four cards:  
     - “For Aperitif”  
     - “For a Dish”  
     - “For Dessert”  
     - “To Discover”  
   - A Back arrow remains visible.
2. **Option Selection**
   - Clicking “For Aperitif” navigates to the Time of Day page.
   - Clicking “Next” with no selection displays “Please select an option” and does not navigate.
3. **Accessibility**
   - Tab to each card; pressing Enter on “For Aperitif” loads the next page.

---

### 4.8.2 FTR-008B: Time of Day Page

1. **Rendering**
   - Shows two options: “In daytime” and “In evening” plus a Back arrow.
2. **Option Selection**
   - Clicking “In daytime” navigates to the Budget page with context “For Aperitif – Daytime.”
   - Clicking “In evening” navigates to Budget with “For Aperitif – Evening.”
   - Clicking “Next” with no selection shows “Please select an option” and does not proceed.
3. **Accessibility**
   - Tab to each option; pressing Enter on “In daytime” moves to the Budget page.

---

### 4.8.3 FTR-008C: Type of Dish Page

1. **Rendering**
   - Shows dish types (e.g., Red Meat, Seafood, Vegetarian, etc.) plus a Back arrow.
2. **Option Selection**
   - Clicking “Red Meat” proceeds to Budget with context “For a Dish – Red Meat.”
   - Clicking “Seafood” proceeds to Budget with “For a Dish – Seafood.”
   - Clicking “Next” with no selection shows “Please select an option.”
3. **Accessibility**
   - Tab to “Vegetarian”; Enter selects and loads Budget “For a Dish – Vegetarian.”

---

### 4.8.4 FTR-008D: Type of Dessert Page

1. **Rendering**
   - Shows “Sparkling” and “Regular” plus a Back arrow.
2. **Option Selection**
   - Clicking “Sparkling” proceeds to Budget with “For Dessert – Sparkling.”
   - Clicking “Regular” proceeds to Budget with “For Dessert – Regular.”
   - Clicking “Next” with no selection shows “Please select an option.”
3. **Accessibility**
   - Tab to “Regular”; Enter selects and navigates.

---

### 4.8.5 FTR-008E: Type of Wine Page

1. **Rendering**
   - Shows “I Don’t Know” and “Local Specialty” plus a Back arrow.
2. **Option Selection**
   - Clicking “I Don’t Know” proceeds to Budget with “To Discover – I Don’t Know.”
   - Clicking “Local Specialty” proceeds to Budget with “To Discover – Local Specialty.”
   - Clicking “Next” with no selection shows “Please select an option.”
3. **Accessibility**
   - Tab to “Sparkling”; Enter selects and navigates.

---

### 4.8.6 FTR-008F: Budget Page

1. **Default Values**
   - Two numeric inputs: Min = 0, Max = 500.
2. **Input Validation**
   - Entering Min = 20 and Max = 50, then clicking “Next” navigates to the Region page.
   - If Max < Min (e.g., Min = 100, Max = 50), show “Max price must be ≥ Min price” and do not navigate.
   - If both fields are blank and “Next” is clicked, show “Please set a budget.”
   - Clicking Back returns to the previous page, preserving selections.
3. **Accessibility**
   - Tab to Min and Max inputs, type values, press Enter, then Tab to “Next” and press Enter.

---

### 4.8.7 FTR-008G: Region Page

1. **Rendering**
   - A collapsed dropdown with Back arrow.
2. **Select Region**
   - Clicking the dropdown and selecting “Bordeaux” navigates to `/wine-results` with `?filter[region]=Bordeaux` plus all previous filters.
   - Clicking Back returns to Budget with Min/Max still populated.
3. **Validation**
   - Clicking “Next” (or “Submit”) without selecting a region shows “Please choose a region.”
4. **Error Handling**
   - If the regions API fails (500), display “Unable to load regions. Retry” and show a “Retry” button which reloads the dropdown on click.
5. **Accessibility**
   - Tab → Enter expands, arrow keys navigate, Enter on “Champagne” closes dropdown and navigates with `?filter[region]=Champagne`.

---

## FTR-009: Wine Results Page

1. **Initial Load**
   - Top of the page displays exactly three featured wine cards (distinct style).
   - Below that, the first 10 matching wine cards are visible.
   - No active filters other than those inherited from the Wine Selection Form.
2. **Filters Toggle**
   - “Filters” collapsed by default. Clicking it expands a row of filters: Price, Region, Alcohol, plus checkboxes (Lactose-Free, Sulfite-Free, Alcohol-Free, Tannic, Full-Bodied, Local Product).
   - Clicking again collapses that row.
3. **Apply Filters**
   - **Price**: Clicking opens the Price pop-up (FTR-003). After setting values and clicking outside, results update within 500 ms to only those wines in the selected range, and URL updates with `filter[price_min]` & `filter[price_max]`.
   - **Checkbox**: Checking “Lactose-Free” immediately updates results. If none match, show “No results found” plus “Clear Filters” link.
   - **Fuzzy Search**: Typing text (e.g. “Cabernet”) filters live as each letter is typed.
4. **Infinite Scroll / Load More**
   - If there are > 10 matches, scrolling to bottom automatically loads the next 10 within 500 ms.  
   - If a “Load More” button is present instead, clicking it appends the next page of results within 500 ms.
5. **Navigation**
   - Clicking “Next” (if present) navigates back to Wine Selection Form Root, preserving all previous context.
   - Clicking a wine card navigates to `/wine/<slug>`, with the Visualizer page loading in ≤ 1 sec.
6. **No Results**
   - If filters conflict (e.g., Alcohol-Free + Tannic), no results appear. Display “No results found” plus “Clear Filters.”
7. **Accessibility**
   - Keyboard: Tab to “Filters,” Enter expands; Tab to “Price,” Enter opens Price pop-up.
   - All checkboxes and inputs must have the correct ARIA roles and states.

---

## FTR-010: Wine Visualizer

1. **Data Rendering**
   - Navigating to `/wine/<slug>` (by clicking a card) loads the page fully (DOM + main image) in ≤ 1 sec.
   - Displays:
     - **Name** (e.g., “Château Margaux”) as `<h1>`.
     - **Bottle Image** with alt text “Château Margaux bottle.”
     - **Description**, **Region**, **Price**, **Alcohol Content**, **Tasting Notes**.
     - **Bottom Carousel** showing exactly three related items (wine or cheese).
2. **Add/Remove Favorites**
   - **Logged-In**:
     - Clicking “Add to Favorites” sends a 200 API request.
     - Button text toggles to “Remove from Favorites.”
     - Toast appears: “Added to favorites.”
     - If a favorites count is shown, it increments by 1.
     - Clicking “Remove from Favorites” reverses all of the above.
   - **Guest**:
     - Clicking “Add to Favorites” opens a login modal.
     - No API request is sent.
3. **Carousel Links**
   - Scrolling to the bottom carousel and clicking a cheese (e.g., “Brie de Meaux”) navigates to `/cheese/<slug>`; that Visualizer loads in ≤ 1 sec.
4. **Invalid ID**
   - Visiting `/wine/999999` shows a custom 404 page: “Product not found.” No blank page or uncaught exceptions.
5. **Accessibility**
   - Screen reader announces the `<h1>` as “Title: Château Margaux.”
   - The “Add to Favorites” button has `aria-pressed` or an equivalent state indicator.
   - All descriptive text (region, tasting notes) must be accessible to screen readers.

---

## FTR-011: Cheese Results Page

1. **Initial Load**
   - Displays the first 10 cheese cards, each showing name, image, and price.
2. **Filtering**
   - Clicking “Filters” expands filter options (Checkboxes, Region pop-up, etc.).
   - Checking “Lactose-Free” and selecting Region “Normandy” updates results to cheeses matching both filters. URL becomes:  
     ```
     ?filter[lactose_free]=true&filter[region]=Normandy
     ```
   - Typing “Camembert” in the page’s search bar:
     - Debounces lookup (~300 ms).
     - Suggestion dropdown appears.
     - Clicking “Camembert” opens `/cheese/<slug>` (Visualizer).
3. **No Results**
   - Conflicting filters (e.g., “Lactose-Free” + “Contains Lactose”) result in “No results found” plus “Clear Filters.”
4. **Navigation**
   - Clicking any cheese card (e.g., “Brie de Meaux”) opens `/cheese/<slug>`, loading within 1 sec.
5. **Accessibility**
   - The “No results found” message is announced as “No results found. Clear Filters.”
   - All checkboxes and dropdowns have proper ARIA semantics.

---

## FTR-012: Cheese Visualizer

1. **Data Rendering**
   - Navigating to `/cheese/<slug>` (by clicking a card) loads the page fully (DOM + main image) in ≤ 1 sec.
   - Displays:
     - **Name** (e.g., “Brie de Meaux”) as `<h1>`.
     - **Image** with alt text “Brie de Meaux wedge.”
     - **Description**, **Region**, **Price**, **Badges** (“Lactose-Free,” “Pasteurized,” if applicable).
     - **Bottom Carousel** showing exactly three related items.
2. **Add/Remove Favorites**
   - **Logged-In**:
     - Clicking “Add to Favorites” sends a 200 API request.
     - Button toggles to “Remove from Favorites.”
     - Toast: “Added to favorites.” Favorites count increments.
     - Clicking “Remove from Favorites” reverses the above.
   - **Guest**:
     - Clicking “Add to Favorites” opens login modal; no API call.
3. **Carousel Links**
   - Scrolling to bottom carousel and clicking a wine (e.g., “Château Margaux”) navigates to `/wine/<slug>`, loading in ≤ 1 sec.
4. **Invalid ID**
   - Visiting `/cheese/999999` displays a custom 404 “Product not found” page.
5. **Accessibility**
   - Screen reader announces “Cheese: Brie de Meaux. Lactose-Free badge.”
   - The “Add to Favorites” button has correct ARIA state.

---

## General Implementation Guidelines

1. **Backend Data & API**
   - All endpoints (search, filter, product details, favorites) must return correct JSON in ≤ 500 ms for filter calls, ≤ 1 sec for full page data.
   - Return 404 and error payloads properly for nonexistent IDs or failing endpoints.
2. **Frontend Implementation**
   - Every UI component (slider, pop-up, modal, dropdown, checkbox, toggle) must follow the exact behaviors specified above.
   - All text (labels, placeholders, error messages) must match the Test Plan verbiage exactly.
3. **State Management & URL Sync**
   - Every filter change updates the querystring, e.g.:
     ```
     ?filter[price_min]=20&filter[price_max]=50
     ```
   - Directly loading a URL with those parameters reproduces the exact filter state in the UI.
4. **Responsive & Cross-Browser Testing**
   - Manually verify on:
     - **Mobile**: iOS 14+ Safari, Android 11+ Chrome
   - Verify functionality at breakpoints: 320 px, 768 px.
5. **Accessibility Verification**
   - Run **Axe Core** during CI to catch automated WCAG 2.1 AA violations.
   - Manually verify with **NVDA** on Windows and **VoiceOver** on macOS that all interactive elements are reachable and announced.
6. **Performance Checks**
   - Use Lighthouse or DevTools to confirm:
     - “Pick a Language” page < 2 sec.
     - Any modal/pop-up < 300 ms to appear.
     - Product Visualizer pages (DOM + images) < 1 sec.
     - Filter toggles refresh results < 500 ms.
7. **Error & Edge Cases**
   - Simulate network timeouts (e.g., disable fetch) for region/pop-up → verify “Unable to load…” message appears.
   - Visit invalid IDs to confirm custom 404 pages render correctly.
8. **Automated Regression Suite (Recommended)**
   - Build a **Cypress** or **Playwright** suite covering all outlined test scenarios:
     - Use resilient selectors (avoid brittle CSS).
     - Assert text exactly matches the expected strings.
     - Verify ARIA attributes and URL updates.
9. **Final Manual Verification**
   - After implementing each feature, run through the corresponding test cases in the Test Plan:
     - Confirm each “Expected Result” matches exactly.
     - Fill “Actual Result” and ensure it says “Pass” for every step.
   - Log any defects and fix them promptly.
   - Re-run until no failures remain.
