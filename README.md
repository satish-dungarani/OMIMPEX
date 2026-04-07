# Om Impex Web Application

Phase 1 delivers the technical foundation for a dynamic business website and admin panel powered by Supabase.

## What is included

- Feature-ready folder structure for frontend, backend integration, docs, and migrations
- Environment variable template
- Typed environment loader for Vite
- Supabase browser client configuration
- SQL migration with all core and supporting tables
- Initial seed file for baseline settings

## Prerequisites

- Node.js 20+
- npm 10+
- Supabase project

## Setup Instructions

1. Install dependencies:
   - `npm install`
2. Copy env template:
   - `cp .env.example .env`
3. Fill required env values in `.env`:
   - `VITE_SUPABASE_URL`
   - `VITE_SUPABASE_ANON_KEY`
   - If you receive `NEXT_PUBLIC_*` keys, copy the same values into `VITE_*` keys for Vite runtime.
4. Run SQL migrations in Supabase SQL Editor (in this order):
   - `supabase/migrations/20260406_phase1_foundation.sql`
   - `supabase/migrations/20260406_phase10_inquiry_infra.sql`
   - `supabase/migrations/20260406_phase11_global_settings_seed.sql`
   - `supabase/migrations/20260406_phase14_value_added_features.sql`
5. Optionally run seed SQL:
   - `supabase/seed/001_initial_seed.sql`
   - `supabase/seed/002_dummy_data.sql` (10-15 sample records across modules)
6. Create storage bucket for media uploads:
   - Bucket name: `media-library`
   - Access: public (for company logo and frontend media)
7. Create storage bucket for inquiry file uploads:
   - Bucket name: `inquiry-files`
   - Access: public (or private with signed URLs if you harden later)
8. Deploy inquiry email edge function (required for admin reply + inquiry emails):
   - `supabase functions deploy inquiry-notify --project-ref <your-project-ref>`
   - Set required secrets:
     - `supabase secrets set SUPABASE_SERVICE_ROLE_KEY=... --project-ref <your-project-ref>`
     - `supabase secrets set RESEND_API_KEY=... --project-ref <your-project-ref>`
   - Optional secrets:
     - `MAIL_FROM`
9. Start local development:
   - `npm run dev`

## Phase 14 Notes

- Newsletter subscriptions are stored in `public.newsletter_subscribers`.
- Catalog PDF link is managed from Admin -> Settings -> Marketing.

## Folder Structure

```txt
src/
  app/
    routes/
  components/
    common/
    layout/
  features/
    admin/
    company/
    inquiries/
    media/
    navigation/
    pages/
    products/
    seo/
    settings/
  lib/
    supabase/
  hooks/
  services/
  types/
supabase/
  migrations/
  seed/
  functions/
docs/
```

## Database Modules Covered

- Users/Admins: `admin_users`
- Company Information: `company_info`
- CMS Pages: `pages`
- Navigation: `navigation_menu`, `footer_links`
- Catalog: `product_categories`, `products`, `product_media`
- Inquiries: `inquiries`, `inquiry_attachments`
- Media: `media`
- Settings: `settings`
- Other required modules: `clients`, `testimonials`, `sliders`, `email_templates`, `seo_meta`, `activity_logs`

## Notes

- `admin_users.id` references `auth.users.id` from Supabase Auth
- `deleted_at` is included where soft delete may be required
- `updated_at` is auto-maintained with trigger function `set_updated_at()`
- RLS policies are intentionally deferred to Phase 2 (Authentication and access control)
- Global settings are saved in `settings` table under `setting_key = global_settings`

## Phase 13 SEO and Performance

- Global SEO helper now injects:
  - canonical URL
  - Open Graph tags
  - Twitter card tags
  - robots and keywords
- Structured data (JSON-LD) is injected globally in public layout:
  - Organization schema
  - WebSite schema
- Static SEO files:
  - `public/robots.txt`
  - `public/sitemap.xml`
- Caching header template for static hosting:
  - `public/_headers`
- Performance updates:
  - lazy loading and async decoding for gallery/product/content images

Important:
- Replace `https://www.omimpex.com` in `public/sitemap.xml` and `public/robots.txt` with your live domain before production launch.

## Phase 16 Testing

- Use `docs/phase-16-testing-checklist.md` as the execution checklist for functional, responsive, browser, performance, and security QA.

## Phase 17 Documentation

- Admin guide: `docs/admin-user-guide.md`
- Developer guide: `docs/developer-documentation.md`
- API docs: `docs/api-documentation.md`
- Database schema docs: `docs/database-schema.md`
- Deployment guide: `docs/deployment-guide.md`
- Video scripts: `docs/video-tutorial-scripts.md`
- Screenshot checklist: `docs/screenshots-capture-checklist.md`
