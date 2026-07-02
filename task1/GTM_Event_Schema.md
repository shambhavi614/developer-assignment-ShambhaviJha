# Task 01 – GTM Event Tracking Plan for OrthoNow

## Understanding the Problem

OrthoNow currently has only basic page view tracking in GA4. This means the marketing team knows how many people visit the website, but they cannot understand:

- Which clinic pages users are interested in
- How many people start booking an appointment
- Where users leave the booking process
- Whether people prefer calling or WhatsApp
- How many patient guides are downloaded
- Which blogs actually engage readers

Before running paid campaigns, proper event tracking is essential. A well-planned Google Tag Manager (GTM) setup will allow every important user interaction to be measured and analysed.

# GTM Event Schema

| Event Name | Trigger Type | Key Parameters | Used In GA4 |
|------------|-------------|---------------|-------------|
| page_view | Page View | page_title, page_location, page_path | Page reports |
| clinic_page_view | Page View (Clinic URLs) | clinic_name, city, page_path | Clinic performance report |
| booking_step_complete | Custom Event (dataLayer) | step_number, step_name, clinic_location | Funnel Exploration |
| booking_completed | Custom Event (dataLayer) | booking_id, clinic_location, specialty | Conversion Report |
| call_now_click | Click Trigger (tel:) | page_name, clinic_location, phone_number | Engagement & Conversion |
| whatsapp_click | Click Trigger (wa.me) | page_name, clinic_location, source_page | Engagement Report |
| patient_guide_form_submit | Form Submission | user_name, phone_number, guide_name | Lead Generation Report |
| patient_guide_download | File Download | guide_name, file_name, download_source | Conversion Report |
| blog_scroll_50 | Scroll Depth (50%) | article_title, page_path, scroll_percent | Content Engagement |
| blog_scroll_90 | Scroll Depth (90%) | article_title, page_path, scroll_percent | Highly Engaged Readers |
| form_error | Custom Event | form_name, field_name, error_message | Debugging Report |
| session_engaged | GA4 Enhanced Measurement | engagement_time, page_path, device | Engagement Metrics |

# Booking Funnel Tracking

The appointment booking form has three steps.

Instead of relying only on GTM, the website's front-end should push events into the `dataLayer` whenever a user successfully completes a step.

This approach is much more reliable because GTM cannot automatically understand which step of a custom multi-step form the user has completed.

## Step 1 – Select Clinic & Specialty

### GTM Trigger

- Custom Event
- Event Name:
  ```
  booking_step_complete
  ```
- Condition:
  ```
  step_number = 1
  ```

### dataLayer Push

```json
{
  "event": "booking_step_complete",
  "step_number": 1,
  "step_name": "location_specialty_selected",
  "clinic_location": "Indiranagar",
  "specialty": "Knee Replacement"
}
```

## Step 2 – Enter Patient Details

### GTM Trigger

- Custom Event
- Event Name:
  ```
  booking_step_complete
  ```
- Condition:
  ```
  step_number = 2
  ```

### dataLayer Push

```json
{
  "event": "booking_step_complete",
  "step_number": 2,
  "step_name": "patient_details_entered",
  "clinic_location": "Indiranagar",
  "preferred_date": "2026-07-15",
  "form_status": "completed"
}
```

## Step 3 – Booking Confirmation

### GTM Trigger

- Custom Event
- Event Name:
  ```
  booking_completed
  ```

### dataLayer Push

```json
{
  "event": "booking_completed",
  "booking_id": "BK20260715001",
  "clinic_location": "Indiranagar",
  "specialty": "Knee Replacement",
  "booking_status": "confirmed"
}
```

# Measuring Funnel Drop-off

Each booking step sends its own event to GA4.

The funnel would look like this:

| Funnel Step | Event |
|-------------|-------|
| Step 1 | booking_step_complete (step_number = 1) |
| Step 2 | booking_step_complete (step_number = 2) |
| Step 3 | booking_completed |

Using GA4 Funnel Exploration, we can measure:

- Users reaching Step 1
- Users progressing to Step 2
- Users completing Step 3
- Drop-off percentage between each step

Example:

| Step | Users |
|------|------:|
| Step 1 | 1000 |
| Step 2 | 720 |
| Step 3 | 540 |

This immediately shows:

- **28% drop-off** between Step 1 and Step 2
- **25% drop-off** between Step 2 and Step 3

These insights help identify where users are abandoning the booking process so improvements can be made.

# GA4 Funnel Exploration Setup

**Step 1**

Event:

```
booking_step_complete
```

Filter:

```
step_number = 1
```

---

**Step 2**

Event:

```
booking_step_complete
```

Filter:

```
step_number = 2
```

---

**Step 3**

Event:

```
booking_completed
```

No additional filter required.

---

# Google Ads Conversion to Import

**Conversion Event:**

```
booking_completed
```

### Why choose this event?

Although actions like phone calls, WhatsApp clicks, and patient guide downloads indicate user interest, they do not necessarily result in an appointment.

A completed booking represents the primary business goal for OrthoNow and is the strongest indicator of a successful lead.

Importing `booking_completed` into Google Ads enables Smart Bidding strategies (such as Maximize Conversions or Target CPA) to optimize campaigns based on actual appointment bookings rather than intermediate actions.

This helps improve advertising performance and ensures the marketing budget is focused on generating real patient appointments.

# Important Implementation Note

Google Tag Manager **cannot automatically detect progress inside a custom multi-step booking form**.

The website's **front-end developer** is responsible for pushing the required events into the `dataLayer` whenever a user successfully completes each booking step.

Once these events are available, GTM listens for them using Custom Event Triggers and forwards the data to GA4.

This separation keeps tracking accurate and makes future maintenance much easier.

# Outcome

Implementing this tracking plan gives OrthoNow complete visibility into how users interact with the website.

The marketing team will be able to:

- Measure appointment bookings accurately
- Identify where users abandon the booking process
- Track phone calls and WhatsApp enquiries
- Monitor patient guide downloads
- Analyse engagement with clinic pages and blogs
- Build remarketing audiences based on user behaviour
- Optimize Google Ads using real appointment conversions instead of page views

Overall, this GTM implementation provides the data foundation needed for effective performance marketing and continuous conversion optimization.