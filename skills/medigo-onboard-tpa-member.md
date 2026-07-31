---
name: Onboard a TPA member under an insurance policy
description: Create a member, an insurance policy and its terms, then attach the member as an insured member.
api: openapi/medigo-openapi-original.yml
operations:
- createMember
- getMember
- createInsurancePolicy
- createTerm
- getTermsByPolicyNumber
- createInsuredMember
---

# Onboard a TPA member under an insurance policy

Set up an insured member in MEDIGO's third party administration (TPA) surface.

## Auth
Send `Authorization: Bearer <partner-api-token>`; JSON in and out; keep the
`Trace-Id` response header. Errors are `{ code, message }`.

## Steps
1. **Create the member.** Call `createMember`; you own the `partner_member_id`
   used to reference this person. Verify with `getMember`.
2. **Create the insurance policy.** Call `createInsurancePolicy`; note the
   returned `policy_number`.
3. **Create the term(s).** Call `createTerm` for the coverage term under that
   policy; note its `external_id`. List a policy's terms any time with
   `getTermsByPolicyNumber`.
4. **Attach the insured member.** Call `createInsuredMember` against
   `terms/{external_id}/insured-members`, linking the member
   (`partner_member_id`) to the term.

## Notes
- Relationships: a policy has many terms (via `policy_number`); a term has many
  insured members (via `external_id`); an insured member belongs to a member
  (via `partner_member_id`). See data-model/medigo-data-model.yml.
- Use `removeInsuredMember` to detach a member from a term.
- No idempotency key is documented — reconcile with `getMember` /
  `getTermsByPolicyNumber` after a failed write instead of blind retry.
