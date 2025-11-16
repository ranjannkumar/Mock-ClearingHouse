1️⃣ claim.submitted

## Purpose

Sent when the clearinghouse successfully receives and accepts your submitted claim (837P/837I).

**This means:**

The file was transmitted correctly

Basic HIPAA validation passed

The claim is now in the clearinghouse queue

**When It Happens**

Immediately after the clearinghouse ingests the EDI 837 file.

## ✔ Example 1
{
  "event_type": "claim.submitted",
  "timestamp": "2025-01-12T10:32:14Z",
  "clearinghouse_claim_id": "CH-4421987",
  "client_reference_id": "CLAIM-10001",
  "payer_id": "US123",
  "submitter_id": "1234567890",
  "status": "ACCEPTED",
  "validation": {
    "hipaa_level_1": "passed",
    "hipaa_level_2": "passed"
  }
}

## ✔ Example 2
{
  "event_type": "claim.submitted",
  "timestamp": "2025-02-18T14:52:10Z",
  "clearinghouse_claim_id": "CH-998120",
  "client_reference_id": "CLM-4455",
  "payer_id": "60054",
  "submitter_id": "9876543210",
  "status": "ACCEPTED",
  "validation": {
    "hipaa_level_1": "passed"
  }
}

2️⃣ claim.rejected

Reject due to clearinghouse validation OR payer 999/277CA rejection.

## ✔ Example 1 (Clearinghouse Reject – 999)
{
  "event_type": "claim.rejected",
  "timestamp": "2025-01-12T10:41:03Z",
  "clearinghouse_claim_id": "CH-4421987",
  "client_reference_id": "CLAIM-10001",
  "status": "REJECTED",
  "reason_codes": [
    {
      "code": "A7:21",
      "source": "999",
      "description": "Diagnosis code missing or invalid"
    }
  ]
}

## ✔ Example 2 (Payer Reject – 277CA)
{
  "event_type": "claim.rejected",
  "timestamp": "2025-02-18T15:05:20Z",
  "clearinghouse_claim_id": "CH-998120",
  "client_reference_id": "CLM-4455",
  "status": "REJECTED",
  "reason_codes": [
    {
      "code": "A3:255",
      "source": "277CA",
      "description": "Subscriber/Insured not found"
    }
  ]
}

3️⃣ claim.accepted_by_payer

This means payer accepted the claim into adjudication.

## ✔ Example 1
{
  "event_type": "claim.accepted_by_payer",
  "timestamp": "2025-01-12T11:10:05Z",
  "clearinghouse_claim_id": "CH-4421987",
  "client_reference_id": "CLAIM-10001",
  "payer_claim_id": "PAYER-4400123",
  "status": "IN_PROCESSING",
  "message": "Claim accepted for adjudication"
}

## ✔ Example 2
{
  "event_type": "claim.accepted_by_payer",
  "timestamp": "2025-02-18T16:01:40Z",
  "clearinghouse_claim_id": "CH-998120",
  "client_reference_id": "CLM-4455",
  "payer_claim_id": "PN-991877",
  "status": "IN_PROCESSING"
}

4️⃣ claim.status_update

These are 277 status responses (realistic statuses).

## ✔ Example 1 – “Pending Review”
{
  "event_type": "claim.status_update",
  "timestamp": "2025-01-13T09:22:40Z",
  "clearinghouse_claim_id": "CH-4421987",
  "client_reference_id": "CLAIM-10001",
  "payer_claim_id": "PAYER-4400123",
  "status": "PENDING_REVIEW",
  "payer_message": "Submitted claim is currently pending review"
}

## ✔ Example 2 – “Additional Documentation Requested”
{
  "event_type": "claim.status_update",
  "timestamp": "2025-02-20T08:33:55Z",
  "clearinghouse_claim_id": "CH-998120",
  "client_reference_id": "CLM-4455",
  "payer_claim_id": "PN-991877",
  "status": "ADDITIONAL_INFO_REQUIRED",
  "reason_codes": [
    {
      "code": "CO-97",
      "description": "The benefit for this service is included in another service"
    }
  ]
}

5️⃣ claim.remittance_received

This is conversion of EDI 835 ERA → JSON.

## ✔ Example 1 – Paid Claim
{
  "event_type": "claim.remittance_received",
  "timestamp": "2025-01-15T13:10:00Z",
  "clearinghouse_claim_id": "CH-4421987",
  "client_reference_id": "CLAIM-10001",
  "payer_claim_id": "PAYER-4400123",
  "status": "PAID",
  "payment": {
    "payment_amount": 118.50,
    "charged_amount": 150.00,
    "allowed_amount": 120.00,
    "patient_responsibility": 1.50,
    "payment_date": "2025-01-15",
    "check_number": "CHK002201"
  },
  "era_breakdown": [
    {
      "service_line": "99213",
      "line_charge": 150.00,
      "line_allowed": 120.00,
      "line_paid": 118.50,
      "adjustments": [
        { "group": "CO", "code": "45", "amount": 30.00 }
      ]
    }
  ]
}

## ✔ Example 2 – Denied Claim
{
  "event_type": "claim.remittance_received",
  "timestamp": "2025-02-22T16:20:10Z",
  "clearinghouse_claim_id": "CH-998120",
  "client_reference_id": "CLM-4455",
  "payer_claim_id": "PN-991877",
  "status": "DENIED",
  "denial_codes": [
    {
      "code": "CO-16",
      "description": "Claim lacks information"
    },
    {
      "code": "M51",
      "description": "Missing or incomplete documentation"
    }
  ],
  "payment": {
    "payment_amount": 0,
    "charged_amount": 220.00,
    "allowed_amount": 0,
    "payment_date": "2025-02-22"
  }
}