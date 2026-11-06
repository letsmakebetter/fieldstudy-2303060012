## A1 – HTTPS and Redirects

The Merit Hotels website was tested to check its HTTPS connection, HTTP-to-HTTPS redirection and SSL certificate information.

When the HTTP version of the website (`http://www.merithotels.com/en/`) was accessed, it redirected to the HTTPS version. The browser did not display a "Not secure" warning when the website was accessed through HTTPS.

During one test, the website displayed a Cloudflare Error 525 (SSL handshake failed). The error page indicated that the browser and Cloudflare were working, but the connection between Cloudflare and the origin host had failed. However, the website loaded successfully when tested again.

The SSL certificate information showed:
- Common Name (CN): `merithotels.com`
- Issuer: Google Trust Services (WE1)
- Valid from: 20 September 2026
- Expiry date: 19 December 2026

The certificate was within its stated validity period on the audit date (4 November 2026).

**Finding:** The website redirects HTTP traffic to HTTPS and provides a secure browser connection when accessible. However, an intermittent SSL handshake error (Cloudflare Error 525) was observed during testing.

**Evidence:** HTTP-to-HTTPS redirect observation; SSL certificate details. The Error 525 was observed but no screenshot was captured.
## A2 – URL Structure

Five URLs from the Merit Hotels website and its reservation system were examined.

The main website URLs generally use HTTPS, lowercase letters and hyphens to separate words. However, the reservation system uses longer and more complex URLs with multiple query parameters, including check-in and check-out dates, guest counts and room information. Some parameter names also contain uppercase letters and underscores.

The reservation URL is therefore less readable than the main website URLs and could be simplified.

**Finding:** The main website has generally readable URL structures, but reservation URLs are unnecessarily complex and contain multiple parameters.

**Evidence:** A2-url-example.png, A2-reservation.png
### A3 – Network Request Analysis

The main document request was inspected using the browser's Developer Tools Network panel. The request method was GET, the server returned status code 200 OK, and the Content-Type was text/html; charset=UTF-8. This indicates that the main HTML document was successfully requested and delivered.

**Evidence:** A3-network-headers.png
The Fetch/XHR filter displayed 12 requests, indicating that the website also performs background data requests in addition to loading the main HTML document.
The Fetch/XHR filter displayed 12 requests, indicating that the website also performs background data requests in addition to loading the main HTML document.

**Evidence:** A3-fetch-xhr.png
The Network panel recorded 75 requests, with 11.9 MB transferred and 86.7 MB of resources. DOMContentLoaded was 759 ms, while the Finish time was 29.23 seconds. The request count and transferred data increased during the initial loading period, showing that the page continued loading additional resources after the first requests.

**Evidence:** A3-network-overview.png
## A4 – Non-existent Page

A non-existent URL was tested by visiting `/no-such-page-123`. The website displayed a custom error page with the message "The page you are looking for could not be found." It also provided a button to return to the homepage. The header and footer sections remained visible, allowing visitors to continue navigating the website.

The Network panel confirmed that the server returned **404 Not Found**, meaning the website correctly handles non-existent pages and does not produce a soft 404.

**Finding:** The website provides a user-friendly 404 page and returns the correct HTTP status code.

**Evidence:** A4-error-page.png
## A5 – Cache-Control

The main HTML document returned the following Cache-Control header:

`no-store, no-cache, must-revalidate`

This indicates that the browser is instructed not to store the main document in its cache.

An image resource returned:

`max-age=2592000, must-revalidate`

This allows the image to be cached for up to 30 days before revalidation is required.

**Finding:** The website uses different caching policies for HTML documents and images. The main document is not stored in the browser cache, while images can be cached for 30 days.

**Evidence:** A5-document-cache.png, A5-image-cache.png
## A6 – Rendering Strategy

The website uses a hybrid rendering approach. The HTML source code (Ctrl+U) already contains important page structures and content, including the navigation menu, reservation form, hotel sections and the `owl-home-big` element. These elements were also visible in the live DOM through the browser's Elements panel.

This indicates that at least some of the page content is delivered as HTML rather than being created entirely by client-side JavaScript. JavaScript may also be used to add interactive functionality and modify the page after loading.

**Finding:** The website appears to use a hybrid approach combining server-delivered HTML with client-side JavaScript.

**Evidence:** A6-view-source.png, A6-elements.png
**H1 Heading Check**

The Merit Hotels homepage was inspected using both the HTML source code (Ctrl+U) and the browser's Elements panel. No H1 elements were found in either view. Some section headings, such as "Otellerimiz", are marked up using paragraph (`<p>`) elements instead.

**Finding:** The homepage does not contain an H1 heading. Adding a descriptive H1 could improve the semantic structure of the page and make its main topic clearer to search engines and assistive technologies.

**Evidence:** A7-h1-source.png, A7-h1-elements.png
**Sponsored / UGC Links**

The HTML source code contains several links, including hotel navigation links, legal pages, email links and social media links. The social media links to Facebook, X, Instagram and YouTube use the `rel="nofollow"` attribute.

No `rel="sponsored"` or `rel="ugc"` attributes were found in the inspected source code. No clear advertising or user-generated comment links were identified during the inspection.

**Finding:** The inspected source uses `nofollow` for social media links, while no sponsored or UGC link attributes were identified.

**Evidence:** A7-link-rel-check.png
**B1 – Slow Network Loading**

The website was tested using the 3G network throttling option in Chrome Developer Tools. Under normal network conditions, the page finished loading in approximately 2 seconds, while under 3G conditions it took approximately 6 seconds. The page loaded successfully in both cases. No unusual visual problems, broken images or obvious loading errors were observed in the inspected sections.

**Finding:** The website takes longer to load under slower network conditions, but the inspected content remained visually functional.

**Evidence:**
- B1-no-throtling-2sec-loading-early.png
- B1-3g-6sec-loading-later.png
- B1-3g-whiteview-starting-load.png
### B2 – Search with No Results

The homepage does not provide a general text-based search field. Instead, it offers a reservation search panel where users select a hotel, dates and guest information. Since there is no general text search field, testing an impossible search term such as `zzzqqq` was not possible.

**Finding:** The website does not appear to provide a general-purpose text search function.

**Evidence:** B2-no-general-search.png
### B3 – Offline Error State

The website was tested using the Offline option in Chrome Developer Tools. After refreshing the homepage, Chrome displayed the `ERR_INTERNET_DISCONNECTED` error instead of loading the website. The browser suggested checking the network connection and reconnecting to Wi-Fi. No custom offline message or retry option from the website was observed.

**Finding:** The website does not appear to provide a custom offline experience. Visitors see the browser's default connection error instead.

**Evidence:** B3-offline-error.png
### B4 – Form Validation
The reservation form was inspected, but it was not submitted because submission could initiate a real reservation process. Therefore, form submission validation could not be tested safely.
**Finding:** Not tested safely.
### B5 – Form Labels

The contact form displays visible labels such as Name, Surname, Country, Email, Phone and Special Requests. However, clicking the labels does not move focus to the corresponding input fields. The country selection control opens when its own selection area is clicked, but the label itself does not activate it.

**Finding:** Although the form fields have visible labels, the labels do not appear to be clickable or properly associated with their controls.

**Evidence:** B5-form-labels.png
### B6 – Keyboard Accessibility

The website can be navigated using the keyboard. The Tab key moves forward between interactive elements, Shift+Tab moves backward, and Enter opens selected links. Visible blue focus indicators appear on some buttons. However, some links in the top navigation menu do not show a clearly visible focus indicator.

**Finding:** Basic keyboard navigation works, but focus visibility is inconsistent.

**Evidence:** B6-keyboard-navigation.png
### B7 – Lighthouse Accessibility Audit

Lighthouse was used to evaluate the accessibility of the Merit Hotels homepage. The website received an accessibility score of **75/100**. The audit identified several accessibility issues, including carousel buttons without accessible names, a footer logo image without alternative text, and insufficient color contrast in some text elements.

**Finding:** The website has some accessibility problems that may make navigation and content understanding more difficult for users with disabilities, especially users who rely on screen readers or have visual impairments.

**Top issues identified:**
1. Some carousel buttons do not have accessible names.
2. The footer logo image is missing alternative text.
3. Some text elements do not have sufficient color contrast.

**Evidence:** B7-home-accessibility.png
**Inner Page Test – Merit Royal Hotel**

The Lighthouse accessibility audit was also performed on the Merit Royal Hotel detail page. The page received an accessibility score of **77/100**. The audit identified issues such as carousel buttons without accessible names, a banner image without alternative text, social media links without discernible names, and insufficient color contrast.

**Finding:** The inner page has accessibility issues similar to those found on the homepage. These issues may make the website harder to use for visitors who rely on screen readers or have visual impairments.

**Evidence:** B7-inner-accessibility.png
### B8 – Vague Links and Buttons

During the website inspection, several button and link labels were found to be unclear or too general. For example, the **“Details”** button on the Premium Deluxe room card does not specify which details will be displayed. A clearer label would be **“View Premium Deluxe Room Details.”**

The **“Continue”** button on the reservation page does not explain what happens next. A more informative label would be **“Continue to Contact Information.”** Similarly, the **“Search”** button in the reservation panel could be improved to **“Search Rooms for Selected Hotel and Dates.”**

The **“Agency Discount Code”** section does not clearly look interactive, making it difficult for users to recognize that it can be selected. It could be presented as a clearly labelled expandable button, such as **“Enter Agency Discount Code.”** Additionally, the gallery navigation indicators are difficult to notice, so users may not realize that more photos are available.

**Finding:** Some interface elements need clearer labels and more visible interactive indicators to help users understand available actions.

**Evidence:** B8-vague-details.png, B8-vague-continue.png, B8-vague-search.png, B8-agency-code.png, B8-gallery-indicators.png
# Part C – Overall Evaluation and Recommendations

## C1 – Website Strengths

**1. Secure connection:** The website uses HTTPS and automatically redirects HTTP requests to HTTPS. This helps protect visitors while browsing the website. Evidence: A1-http.png.

**2. Helpful error page:** When a non-existing page is opened, the website displays a custom 404 error page with a link to return to the homepage. This helps visitors recover from navigation errors. Evidence: A4-error-page.png.

## C2 – Main Problems and Recommendations

### 1. Reservation Information and Navigation Are Inconsistent

**Problem:** The reservation system does not consistently preserve user selections. On the homepage, visitors can select a hotel, check-in and check-out dates, and guest information. However, after clicking the Search button, the hotel name may remain selected while dates and guest information reset to default values. Visitors must select these details again.

Further problems occur when using browser navigation. For example, after returning with the browser Back button, the selected hotel may support children, but the child selection field disappears because the default Diamond hotel state remains displayed. Returning to the homepage through the hotel logo or homepage link can also remove all selections and redirect visitors to the English homepage (`/en/`).

Refreshing the page, temporarily losing the internet connection, or refreshing during room selection may also cause previously selected information to disappear.

**Affected users:** All visitors using the reservation system, especially those navigating between booking steps or experiencing an unstable internet connection.

**Recommendation:** Preserve the selected hotel, dates, room and guest information across page navigation, browser Back and Forward actions, page refreshes and temporary network interruptions. Keep the displayed form synchronized with the selected hotel and restore the user's previous selections whenever possible.

**Impact:** High  
**Effort:** High

**Evidence:** C-reservation-02-homepage-selection.png, C-reservation-03-after-search.png, C-reservation-04-browser-back.png, C-reservation-05-homepage-back.

### 2. Accessibility and Interaction Problems

**Problem:** Lighthouse gave the homepage an accessibility score of 75/100 and the inner hotel page 77/100. The audits identified carousel buttons without accessible names, images missing alternative text, insufficient colour contrast and social media links without accessible names. Visible form labels did not move focus to their corresponding fields when clicked. The agency discount code control also needs clearer keyboard and screen-reader accessibility.

**Affected users:** Visitors using screen readers, keyboard navigation or assistive technologies, as well as users with visual impairments or those who cannot use a mouse.

**Recommendation:** Add accessible names to buttons and links, provide alternative text for meaningful images, improve colour contrast, correctly associate form labels with their inputs and ensure expandable controls can be operated using a keyboard.

**Impact:** High  
**Effort:** Medium

**Evidence:** B5-form-labels.png, B6-keyboard-navigation.png, B7-home-accessibility.png, B7-inner-accessibility.png, B8-agency-code.png.

### 3. Page Loading and Resource Usage

**Problem:** During the Network inspection, the homepage loaded 75 requests and transferred approximately 11.9 MB of data. The total resource size was approximately 86.7 MB in that measurement. Loading also took longer under the 3G network condition.

**Affected users:** Visitors with slow internet connections, limited mobile data or less powerful devices.

**Recommendation:** Optimise images, compress large resources, reduce unnecessary requests and load non-essential images and scripts only when needed.

**Impact:** Medium–High  
**Effort:** Medium–High

**Evidence:** A3-network-overview.png, B1-3g-6sec-loading-later.png.

## C3 – Additional Usability Improvements

The following issues were also observed during the inspection:

1. **Footer contact information:** The phone number and email address at the bottom of the website are not clickable. Unlike the Facebook and Instagram links, they do not open a calling or email application. Adding `tel:` and `mailto:` links would make contacting the hotel easier.

2. **Gallery touchpad navigation:** On a laptop, using two fingers on the touchpad does not change gallery photos, while dragging a photo with one finger works. The gallery should support common touchpad gestures and provide alternative navigation controls.

3. **Gallery indicators:** The small navigation dots below the gallery are difficult to notice. They should be more visible, with a clear indication of the currently selected photo.

4. **Room and guest selection:** Some room and guest combinations can be selected without a sufficiently clear warning when the combination is unsuitable. The website should validate room capacity and display understandable messages.

5. **Favicon consistency:** When Turkish is selected, some hotel pages display a generic globe icon instead of the Merit Hotels logo in the browser tab. The correct favicon should be displayed consistently across the Turkish pages.

6. **Unclear button labels:** Labels such as "Details", "Continue" and "Search" do not always explain the action clearly. More descriptive wording would help visitors understand what happens next.

7. **Agency discount code control:** The "Agency Discount Code" text appears interactive and opens when clicked, but its interactive nature is not sufficiently clear for visitors who cannot use a mouse. It should have a clear focus indicator and accessible keyboard interaction.

8. **Language consistency after homepage navigation:** Returning to the homepage through the hotel logo or homepage link can redirect visitors to the English version and remove their previous reservation selections. The website should preserve the selected language and reservation information.

## C4 – Overall Conclusion

Merit Hotels provides a secure website with a helpful custom error page and useful hotel information. However, the most important issue is the inconsistent reservation process, which can cause visitors to lose their selections and repeat steps. Accessibility and page performance also need improvement. Addressing these problems, together with the additional usability issues, would make the website more reliable, accessible and easier to use.