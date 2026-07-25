# Smokenza Admin Dashboard 

## Short Summary

`Smokenza-Dashoard` is the admin dashboard for the commerce system. It is a Next.js application used to manage inventory, orders, refunds, customers, employees, brands, blogs, carousel images, service pricing, discount codes, notifications, invoices, profile data, and analytics. It uses Zustand stores and an Axios API client to communicate with the backend. The dashboard has strong feature breadth, but should improve route protection, configuration management, naming consistency, responsive layout, and frontend validation.

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

