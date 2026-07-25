# Ciger Admin Dashboard 

## Short Summary

`ciger` is the admin dashboard for the commerce system. It is a Next.js application used to manage inventory, orders, refunds, customers, employees, brands, blogs, carousel images, service pricing, discount codes, notifications, invoices, profile data, and analytics. It uses Zustand stores and an Axios API client to communicate with the backend. The dashboard has strong feature breadth, but should improve route protection, configuration management, naming consistency, responsive layout, and frontend validation.

## Project Identity

- Folder: `ciger`
- Role: Admin/back-office application
- Framework: Next.js App Router
- Language: TypeScript and React
- Package name: `nextjs-project-initialize`
- Main API client: `lib/axios.ts`
- State management: Zustand stores under `app/store`
- UI folders: `Components` and `app`
- Start scripts:
  - `npm run dev`: `next dev --turbopack`
  - `npm run build`: `next build --turbopack`
  - `npm run start`: `next start`
  - `npm run lint`: `eslint`

## Technology Stack

- Next.js 15.5.6
- React 19.1.0
- TypeScript
- Tailwind CSS 4
- Zustand
- Axios
- js-cookie/js-cookies
- Quill/react-quilljs for blog/content editing
- jsPDF for invoice/PDF generation
- lucide-react and HugeIcons

## Application Structure

Important areas:

- `app/page.tsx`: root redirect entry.
- `app/layout.tsx`: root metadata and global font.
- `app/pages/layout.tsx`: authenticated dashboard shell with navbar and sidebar.
- `app/auth`: admin and employee auth flows.
- `app/pages`: dashboard feature routes.
- `app/store`: Zustand stores for API-backed state.
- `Components`: feature UI modules.
- `lib/axios.ts`: configured backend API client.

## Main Routes and Screens

Authentication:

- `/auth/signin`
- `/auth/signup`
- `/auth/forget-password`
- `/auth/otp`
- `/auth/new-password`
- `/auth/welcome`
- `/auth/employeeRegister`
- `/auth/employeeOtpVerify`
- `/auth/employeeResetPass`

Dashboard:

- `/pages/dashboard`
- `/pages/analytics`
- `/pages/order`
- `/pages/order/viewOrder/[id]`
- `/pages/refunds`
- `/pages/refunds/[id]`
- `/pages/customers`
- `/pages/customers/[di]`
- `/pages/inventory`
- `/pages/inventory/addItem`
- `/pages/inventory/editItem`
- `/pages/inventory/viewItem`
- `/pages/brand`
- `/pages/brand/create`
- `/pages/brand/edit/[id]`
- `/pages/brand/view/[id]`
- `/pages/blogs`
- `/pages/blogs/create`
- `/pages/blogs/edit/[id]`
- `/pages/blogs/view/[id]`
- `/pages/discountCode`
- `/pages/discountCode/create`
- `/pages/discountCode/edit/[id]`
- `/pages/discountCode/view/[id]`
- `/pages/carousel`
- `/pages/servicePricing`
- `/pages/servicePricing/edit`
- `/pages/employee`
- `/pages/notifications`
- `/pages/profile`
- `/invoice`
- `/invoice/[id]`

## State and API Layer

The dashboard uses Zustand stores to organize API interactions:

- `userStore.tsx`: admin auth, signup, signin, OTP, reset password, signout.
- `inverntoryStore.tsx`: products, brands, product create/update/delete, image handling.
- `brandStore.tsx`: brand list/detail/create/update/delete.
- `blogStore.tsx`: blog list/detail/create/update/delete.
- `discountCodeStore.tsx`: discount CRUD.
- `servicePricing.tsx`: service pricing fetch/update.
- `useOrderStore.tsx`: orders list, detail, update.
- `useCustomerStore.tsx`: customer/order view support.

The shared Axios instance points to:

- `https://ciger-backend-2.onrender.com`

Local and alternate production URLs are commented out.

## Main Admin Capabilities

### Catalog Management

Admins can create, edit, view, and delete inventory products. Products support multiple images, brand association, category/subcategory, price, discount, color, stock quantity, `isBest`, `isNew`, and stock status.

### Brand and Blog Management

The dashboard manages brand images and blog entries. Blog editing uses rich text dependencies, indicating a content-management workflow.

### Order and Refund Management

Admins can view all orders, open order details, update order status/tracking, trigger tracking emails, process refunds through the backend, update refunded status, and send refund emails.

### Discount and Service Pricing

Admins can manage percentage discount codes and service pricing values that the storefront reads.

### Carousel Management

Admins can upload and delete carousel images used by the storefront landing page.

### Employee Approval

The dashboard fetches employee/admin accounts and allows approval changes. This maps to the backend `Admin` model and `employee` routes.

### Analytics

Analytics pull yearly/monthly revenue breakdown data from `/dashboard/yearly` and monthly formatted breakdown data from `/dashboard`.

## Strengths

- Large back-office feature coverage.
- Uses domain-specific stores instead of pushing every request into components.
- Has route-level organization for create/edit/view pages.
- Integrates with backend domains cleanly through a central Axios helper.
- Includes invoice/refund/order screens, which are important for real operations.
- Uses reusable shell layout with sidebar and navbar.

## Main Risks and Issues

1. Frontend route protection is unclear.
   - The dashboard shell exists, but there is no obvious middleware guard in the inspected files.
   - Admin pages should be protected from unauthenticated users and unapproved employees.

2. Backend authorization dependency is risky.
   - The frontend calls many admin write endpoints, but backend routes do not consistently enforce admin auth.
   - Frontend hiding is not enough; backend must protect the operations.

3. API base URL is hardcoded.
   - `lib/axios.ts` points directly to Render.
   - Use environment variables such as `NEXT_PUBLIC_API_BASE_URL`.

4. Naming consistency needs cleanup.
   - `inverntoryStore.tsx` is misspelled.
   - Route names mix singular/plural and camelCase.
   - This increases onboarding cost and bug risk.

5. Some API paths may be fragile due to case mismatch.
   - Example: the admin brand store posts to `/brand/createbrand`, while the backend route is declared as `/createBrand`.
   - Express routing is usually case-insensitive by default, but relying on that makes deployments and proxies less predictable.

6. Layout may be weak on small screens.
   - `app/pages/layout.tsx` uses a fixed two-column grid (`grid-cols-[2fr_6fr]`), which can squeeze content on mobile.

7. Client validation and error handling appear uneven.
   - Some flows handle API errors, but there is no shared error pattern or form schema layer.

8. No automated tests are present.
   - There are no visible component, store, or route tests.

9. Generated PDFs and refund/payment workflows need strong verification.
   - Invoice/refund logic affects money and customer communication, so it should have explicit test coverage.

## Recommended Next Steps

1. Add Next middleware or layout-level auth checks for dashboard routes.
2. Move API base URL into `.env.local` and deployment environment variables.
3. Align frontend endpoint casing and names with backend routes.
4. Rename misspelled files carefully with import updates.
5. Add shared API error handling and form validation.
6. Improve mobile behavior of dashboard layout.
7. Add tests for admin auth, product creation/update, order status update, refund workflow, and invoice generation.
8. Add a project README that replaces the default Next.js scaffold content.

