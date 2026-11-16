# Safi Laundry

A responsive mint-green and white laundry booking and operations site for Safi Laundry in Addis Ababa. This first release intentionally has no AI integration.

## Current features
- Customer account creation, sign-in, password-reset email, pickup booking, unique Safi order references and order tracking.
- Customer feedback and post-delivery reviews.
- Washer workspace limited by database policies to orders assigned to that washer; washers can update laundry stages.
- Admin workspace for order status, washer assignment, inspection quotes, payment status, feedback resolution, review responses and feedback-based service insights.
- Responsive static website with HTML sanitization and basic security response headers.

## Backend and launch setup
1. The app uses the existing Supabase project configured in app.js. Its browser key is a publishable client key; row-level security must remain enabled.
2. Apply supabase/migrations/202611160001_assigned_washer_access.sql to the same Supabase database if it has not already been applied.
3. Create the owner account and confirm its email. Then grant the admin role in the Supabase SQL editor (replace the email):

   insert into public.user_roles (user_id, role)
   select id, 'admin'::public.app_role
   from auth.users
   where lower(email)=lower('OWNER_EMAIL_HERE')
   on conflict (user_id,role) do nothing;

4. Washer accounts must be provisioned and given the washer role by an authorized administrator. The dashboard invitation form records an email address only; it does not send an email or grant access.
5. Prices are confirmed after garment inspection. Yango is a manual delivery handoff only; no automatic driver booking is integrated.

## Netlify
- Repository: team3nerd/safilaundry-next
- Production branch: main
- Build command: leave empty
- Publish directory: .
- netlify.toml contains the static publish setting; _headers adds baseline security headers.

## Before accepting real customer orders
- Confirm the owner account has the admin role.
- Provision and test a separate washer account.
- Test signup, email confirmation, sign-in, password reset, booking, customer tracking, assigned washer progress, admin quote/status/assignment, feedback and reviews.
- Confirm pickup coverage, business contact details, delivery process and customer privacy notice.
