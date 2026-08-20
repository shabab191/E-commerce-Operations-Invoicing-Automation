# n8n Automated E-commerce Workflow: Order Alert, PDF Invoice Generation & Daily Sales Summary

An advanced n8n workflow designed to automate e-commerce operations. It fetches the latest orders, alerts admins via email, generates a dynamic PDF invoice in BDT currency, sends the invoice directly to the customer's email, and provides a daily end-of-day sales summary[cite: 2].

---

## 📌 Features / What It Does
* **Automated Order Tracking:** Runs every 5 minutes to check for new orders from your store API[cite: 2].
* **Admin Alert System:** Instantly sends detailed order notifications to admin emails[cite: 2].
* **PDF Invoice Generation:** Automatically formats order details and generates a professional PDF invoice in **BDT** currency using an external PDF generation service[cite: 2].
* **Customer Auto-Invoicing:** Automatically emails the generated PDF invoice directly to the customer's email address[cite: 2].
* **Daily Sales Summary:** Runs a daily cron job (at 11:55 PM) to calculate total sales and order status breakdowns, sending a summary report to the management team[cite: 2].

---

## 🛠️ Problem & Solution Overview

* **Problem 1 (Currency Issue):** The default invoice generator creates invoices in USD (`$`) or other foreign currencies, which did not match local e-commerce requirements.
  * **Solution:** Modified the payload building code node to explicitly include `"currency": "BDT"`[cite: 2].
* **Problem 2 (Admin vs. Customer Separation):** Earlier, order alerts were only sent to admins, and customers did not receive their automated PDF invoices.
  * **Solution:** Split the workflow from the order detection step into two parallel paths: one for admin alerts and another dedicated path for generating and emailing PDF invoices directly to customers[cite: 2].
* **Problem 3 (Binary Data Error):** Direct conditional routing sometimes dropped file attachments causing binary data errors.
  * **Solution:** Streamlined the direct sequence from Payload Builder -> PDF Generator -> Gmail node to seamlessly pass the invoice file attachment (`data`)[cite: 2].

---

## ⚙️ Required Placeholders & Configuration Setup

Before activating this workflow in your n8n instance, make sure to update and replace the following sensitive placeholders with your own credentials:

1. **HTTP Request Headers (`Get Latest Orders` & `Get Today's Orders` nodes):**
   * Replace the Authorization Bearer Token with your store's API Seller Token:
     ```text
     Bearer YOUR_STORE_API_BEARER_TOKEN_HERE
     ```
   * Update the API endpoint URL if your store is hosted on a different domain (`https://your-store-api.com/api/seller/orders`)[cite: 2].

2. **Email Notification Nodes (`Send New Order Email` & `Send Daily Summary Email`):**
   * **`fromEmail`**: Replace with your sender email address (e.g., `your-email@gmail.com`)[cite: 2].
   * **`toEmail`**: Replace with your respective admin email addresses (e.g., `admin1@gmail.com, admin2@gmail.com`)[cite: 2].
   * **SMTP Credentials**: Link your own SMTP credential ID in n8n[cite: 2].

3. **Invoice Generator Node (`Generate Invoice Pdf`):**
   * Replace the Authorization API key with your own invoice-generator.com API key:
     ```text
     Bearer YOUR_INVOICE_GENERATOR_API_KEY
     ```

4. **Customer Email Node (`Send a message` / Gmail Node):**
   * Connect your own **Gmail OAuth2** account credentials in n8n instead of the default template account[cite: 2].

---

## 🚀 How to Import
1. Download or copy the workflow JSON file from this repository.
2. Open your n8n dashboard, click on **Workflows** -> **Import from File**.
3. Configure your credentials (SMTP, Gmail OAuth2, and API keys) as mentioned above.
4. Test and activate the workflow!