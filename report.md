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