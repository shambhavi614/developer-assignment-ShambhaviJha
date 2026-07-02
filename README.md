# Developer Assignment – Shambhavi Jha

This repository contains my submission for the **Namoza Developer Assignment**.

## Repository Structure

```
developer-assignment-ShambhaviJha/
│
├── README.md
├── TASK-01-GTM_Event_Schema.md
├── TASK-02-index.html
├── TASK-02-PageSpeed-Screenshot.png
├── Assets-Landing-page-preview.png
├── TASK-03-Integration_Design.md
```


# Live Demo

**Netlify Deployment**
https://orthonow-healthcare.netlify.app/


# Landing Page Preview

> Replace the image below with your screenshot.



# Google PageSpeed Insights

The landing page was optimized for fast loading, lightweight assets, and mobile responsiveness.

<img width="1920" height="916" alt="image" src="https://github.com/user-attachments/assets/ca9ce2f5-a539-4dad-9f56-bc7cb5787321" />


# Task 01 – GTM Event Tracking Plan

Designed a complete Google Tag Manager event tracking strategy for OrthoNow including:

- Page Views
- Clinic Page Tracking
- Multi-Step Booking Funnel
- Phone Call Tracking
- WhatsApp Click Tracking
- Patient Guide Downloads
- Blog Engagement
- Google Ads Conversion Event
- GA4 Funnel Exploration

# Task 02 – Consultation Landing Page

Built a conversion-focused landing page using:

- HTML5
- CSS3
- Vanilla JavaScript

### Features

- Responsive Mobile-First Design
- Minimal 2-Field Consultation Form
- Trust Elements
- Strong Primary CTA
- GTM dataLayer Push
- Thank-you State (No Page Reload)
- Lightweight & Performance Optimized

### GTM Event Fired

```javascript
window.dataLayer.push({
  event: "consultation_form_submitted",
  form_name: "landing_page_consultation",
  user_name: "...",
  phone_length: 10,
  city: "Bengaluru",
  timestamp: "..."
});
```

# Task 03 – Integration Design

Designed an end-to-end CRM integration including:

- HubSpot CRM
- HubSpot CRM Search API
- Contact Create / Update Logic
- Phone Number Deduplication
- Karix WhatsApp Business API
- Google Ads Conversion Tracking
- Retry Queue Strategy
- SLA Monitoring

# Tech Stack

- HTML5
- CSS3
- Vanilla JavaScript
- Google Tag Manager (GTM)
- Google Analytics 4 (GA4)
- HubSpot CRM
- Karix WhatsApp Business API
- Google Ads

# Loom Walkthrough

The Loom video demonstrates:

- GTM Event Tracking Plan
- Landing Page Demo
- Live `window.dataLayer` Event
- CRM Integration Architecture

**Loom Link**

> Add your Loom link here after recording.

# Author

**Shambhavi Jha**

Computer Science Engineering

GitHub: https://github.com/shambhavi614

## Thank You

Thank you for reviewing my submission. I appreciate the opportunity to complete this assignment and look forward to your feedback.
