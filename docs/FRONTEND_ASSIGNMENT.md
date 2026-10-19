# Frontend Engineering Assessment: E-Commerce Web Application

Welcome to your frontend engineering assessment!

You have been provided with a complete, fully tested, and operational backend and REST API running on Next.js App Router, Prisma ORM, and PostgreSQL.

Your task is to **design and implement the user-facing frontend application** that interfaces with these APIs.

---

## 1. What Has Been Provided For You

The starter repository contains:
1. **Fully Operational Backend**: Route handlers in `app/api/*` covering authentication, products CRUD, search, category filtering, and pagination.
2. **Database & Seed Data**: Pre-configured Prisma schema and sample database seeded with products and accounts.
3. **Automated Backend Test Suite**: Run `npm test` at any time to verify that the backend is working.
4. **Typed API Client**: Check `lib/api/client.ts` for pre-built, typed helper functions with built-in cookie support (`apiClient.auth.*` and `apiClient.products.*`).
5. **API Documentation**: Refer to `docs/API.md` for complete endpoint schemas and status codes.
6. **Minimal Development Shell**: Basic navigation and placeholder pages so you can see routing in action.

---

## 2. Your Assignment

Your goal is to replace the starter development placeholders with a responsive, modern e-commerce web application.

### Key Requirements

#### 1. Public Product Catalogue (`/products`)
- Design an attractive product grid or list view displaying product card items (image, title, category, price, stock badge).
- Implement interactive **search input** (debounced or on submit) querying `/api/products?search=...`.
- Implement **category filter dropdown or pill buttons** dynamically populated from `/api/categories`.
- Implement **sorting controls** (price low-to-high, high-to-low, newest, name).
- Implement clean **pagination or load more** with page navigation and total count display.
- Handle **empty results state** gracefully when no products match.
- Provide accessible **loading skeletons or spinners** while data is being fetched.

#### 2. Dynamic Product Detail Page (`/products/[slug]`)
- Render dynamic product details fetched via `GET /api/products/slug/[slug]`.
- Display high-resolution product imagery with fallback for missing images.
- Show formatted price, category tag, stock availability indicators (In Stock / Low Stock / Out of Stock).
- Provide descriptive copy and clean layout.
- Handle 404 Not Found cleanly when an invalid slug is entered.

#### 3. Authentication & User Profile (`/login`)
- Create an intuitive login form with client-side validation and clear error feedback.
- Support login using the seeded test accounts (or new registered users).
- Display user role badges (`ADMIN` vs `CUSTOMER`).
- Support a clean logout action that clears the session cookie and updates UI state immediately.
- (Bonus) Provide customer registration capability using `POST /api/auth/register`.

#### 4. Administrative Product Management (`/admin/products`, `/admin/products/new`, `/admin/products/[id]/edit`)
- Create an administrative dashboard table showing product inventory, stock levels, and active status.
- Build the **New Product Form** (`/admin/products/new`) with comprehensive validation and error highlights.
- Build the **Edit Product Form** (`/admin/products/[id]/edit`) pre-populated with existing product data.
- Implement **Delete Confirmation** modal or dialog with immediate list updates upon deletion.
- Verify that administrative mutations are guarded on the server. If a non-admin attempts a mutation, show an informative error banner.

---

## 3. Seeded Test Credentials

Use these accounts to test both customer and administrator flows:

| Role | Email | Password | Permissions |
| :--- | :--- | :--- | :--- |
| **Administrator** | `admin@store.dev` | `AdminPassword123!` | Full permissions: create, edit, delete products |
| **Customer** | `customer@example.com` | `CustomerPassword123!` | Read catalogue, cannot perform product mutations |

---

## 4. Using the Reusable API Client

To speed up your frontend development, a typed client is provided in `lib/api/client.ts`. It handles JSON serialization, headers, and credentials automatically:

```typescript
import { apiClient } from "@/lib/api/client";

// Example 1: Fetch products
const response = await apiClient.products.list({
  page: 1,
  limit: 12,
  category: "Audio",
  sortBy: "price",
  order: "asc",
});
console.log(response.data); // ProductItem[]
console.log(response.pagination); // { page, limit, total, totalPages }

// Example 2: Login
await apiClient.auth.login({
  email: "admin@store.dev",
  password: "AdminPassword123!",
});

// Example 3: Check current user session
const session = await apiClient.auth.me();
console.log(session.data.user);
```

You are free to enhance or customize `lib/api/client.ts` or use React Query / SWR if you prefer.

---

## 5. Evaluation Rubric

Submissions will be assessed across five core criteria:

1. **User Experience & Visual Polish (30%)**:
   - Clean, modern visual design and layout hierarchy.
   - Smooth transitions, thoughtful hover states, and responsive design across mobile, tablet, and desktop viewports.
2. **State Management & Data Integration (25%)**:
   - Proper management of loading, error, empty, and success states.
   - Synchronization of URL query parameters with filter/search state.
3. **Component Architecture & Clean Code (20%)**:
   - Modular, reusable React components with clean props interfaces.
   - Proper use of TypeScript types.
4. **Form Handling & Validation (15%)**:
   - Clear input validation feedback and error states.
   - Prevention of double-submission and optimistic/safe UI updates.
5. **Security & Session Awareness (10%)**:
   - Correct handling of HttpOnly cookie sessions.
   - Appropriate UI role guards matching server permissions.

Good luck! We look forward to reviewing your implementation.
