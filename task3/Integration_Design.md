# Task 03 – Integration Design (OrthoNow CRM Integration)

## End-to-End Integration Architecture

I would use a **custom backend API** (Node.js/Express or a serverless function) instead of connecting the landing page directly to HubSpot or using no-code tools like Zapier or Make. This approach provides better security, faster performance, and more control over validation, error handling, and logging.

### Flow

1. The patient submits the consultation form (Name, Phone, Clinic Preference).
2. The landing page sends the data to a secure backend endpoint using an HTTPS POST request.
3. The backend validates the input and searches HubSpot for an existing contact using the **phone number**. Since HubSpot deduplicates contacts by **email** by default, I would first search for the phone number using the **HubSpot CRM Search API**. If a contact exists, I would update it; otherwise, I would create a new contact.
4. While creating or updating the contact, the backend sets:
   - Name
   - Phone
   - Clinic Preference
   - Source = **Google Ads – Consultation Landing Page**
   - Lead Status = **New Enquiry**
5. Once HubSpot responds successfully, the backend sends a WhatsApp confirmation message through **Karix (Namoza WhatsApp Business API)**.
6. After both operations are accepted, the backend returns a success response to the landing page, which displays the thank-you message.
7. The landing page then pushes the **`consultation_form_submitted`** event to the `dataLayer`, allowing **Google Tag Manager (GTM)** to trigger the Google Ads Conversion Tag.

## Biggest Failure Point

The biggest risk is **duplicate contacts caused by HubSpot's default email-based deduplication**.

Since this landing page only collects a phone number, relying on HubSpot's default behavior would create multiple contacts if the same patient submits the form again.

To avoid this, every submission first performs a **Search API lookup using the phone number**. If a matching record is found, that contact is updated instead of creating a new one. This keeps the CRM clean and preserves the patient's interaction history.

If the HubSpot API is temporarily unavailable, I would store the lead in a retry queue (for example, using Redis or AWS SQS) so the data is not lost and can be synchronized automatically once HubSpot becomes available.

## WhatsApp SLA (Within 2 Minutes)

The two-minute SLA could be affected by:
- HubSpot API delays or downtime
- Karix API failures
- Network interruptions
- Backend server issues
- Message queue delays

To monitor this, I would log timestamps for every stage of the workflow:
- Form received
- HubSpot completed
- WhatsApp request sent
- WhatsApp delivery response received

These logs would be tracked in an application monitoring tool such as **Datadog**, **New Relic**, or **CloudWatch**, with alerts configured if the average processing time exceeds two minutes or if the Karix API returns repeated failures.

Additionally, failed WhatsApp requests would automatically retry with exponential backoff while notifying the operations team if multiple retries fail. This ensures patients receive timely confirmations and maintains a reliable user experience.