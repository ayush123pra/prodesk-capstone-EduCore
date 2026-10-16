# 🎓 EduCore: Enterprise Learning Management System (PRD)

**Status:** Sprint 13 — Architectural Blueprint (All features are currently in the planning phase; no code is implemented or deployed yet).
**Track:** Frontend Specialist (Track A)
**Repository:** `prodesk-capstone-EduCore`

---

## 1. High-Level Description
EduCore is a scalable, responsive Learning Management System (LMS) engineered to facilitate seamless online education. Inspired by industry standards like Coursera and Udemy, the platform provides distinct, role-based workflows for Instructors to manage educational content and Students to discover, enroll in, and track their progress through courses.

## 2. Problem Statement & Proposed Solution
**Problem:** Educational platforms often suffer from disjointed user experiences, complex course creation tools that deter instructors, and poor progress tracking that demotivates students.
**Solution:** EduCore provides a unified, role-based frontend architecture. By separating the instructor and student contexts and utilizing centralized state management (Zustand), the platform delivers a frictionless, real-time learning and teaching experience without page-reload bottlenecks.

## 3. Target Users
*   **Students:** Individuals seeking to browse course catalogs, enroll in subjects, watch video lessons, and track their educational progress.
*   **Instructors:** Subject matter experts who need intuitive tools to create courses, upload lesson materials, and structure syllabi.

## 4. Product Goals & Success Criteria
*   **Goal 1:** Deliver a complete end-to-end frontend MVP covering both student and instructor workflows.
*   **Goal 2:** Ensure highly responsive, mobile-first design across all views.
*   **Success Criteria:** 
    *   100% completion of P0 and P1 features within 4 sprints.
    *   Pass automated accessibility audits (WCAG 2.1 AA) using Storybook and `shadcn/ui`.
    *   Seamless integration of mock APIs without UI blocking or state desynchronization.

## 5. Functional Requirements
| ID | Feature | Description | Role |
| :--- | :--- | :--- | :--- |
| **FR-01** | Authentication | Users can register and log in, establishing an active session. | All |
| **FR-02** | Role-Based Access | Application restricts route access based on user role. | All |
| **FR-03** | Course Catalog | Users can browse, search, and filter available courses. | Student |
| **FR-04** | Enrollment | Users can enroll in a course, adding it to their dashboard. | Student |
| **FR-05** | Video Player UI | Users can watch lessons sequentially with a sidebar index. | Student |
| **FR-06** | Progress Tracking | Lessons can be marked complete, updating a progress bar. | Student |
| **FR-07** | Course Creation | Users can create new courses with titles and descriptions. | Instructor |
| **FR-08** | Lesson Management | Users can add ordered lessons (video URLs) to their courses. | Instructor |

## 6. Non-Functional Requirements
*   **Responsiveness:** Mobile-first design adapting fluidly to tablet and desktop viewports using Tailwind CSS grids/flexbox.
*   **Accessibility:** Semantic HTML, ARIA labels, and keyboard navigation support out-of-the-box via `shadcn/ui` primitives.
*   **Security:** Client-side route guarding (HOC or Next.js middleware routing) to prevent unauthorized access to instructor routes.
*   **Performance:** Optimized asset loading, lazy-loaded video components, and instantaneous UI updates via optimistic Zustand state mutations.
*   **Maintainability:** Strict component-driven architecture, modular state slices, and documented UI elements via Storybook.

## 7. Feature Priorities & 8. MVP Scope
### P0 — Must-Have (MVP Scope)
*   Student and instructor roles.
*   Login and registration screens.
*   Student dashboard (enrolled courses & progress).
*   Course catalog with search/filters.
*   Course learning page (video player UI & ordered lessons).
*   Lesson completion and course progress tracking.
*   Instructor dashboard (create/manage courses and lessons).

### P1 — Next Priority (MVP Scope)
*   Course descriptions and instructor profiles.
*   Course enrollment workflow.
*   Responsive mobile design.
*   Loading, error, and empty states (Skeleton UI).
*   Basic role-based access control.

### P2 — Future Enhancements (Strictly Out of Scope)
*   Certificates of completion.
*   Course ratings and reviews.
*   Payment gateway integration.
*   Advanced analytics and recommendations.

## 9. User Stories & Acceptance Criteria
*   **US-01 (Student):** As a student, I want to filter the course catalog so that I can find courses relevant to my interests.
    *   *Acceptance Criteria:* Search bar inputs update the rendered course list in real-time. Categories trigger immediate list filtering.
*   **US-02 (Student):** As a student, I want to mark a lesson as complete so that my overall course progress updates.
    *   *Acceptance Criteria:* Clicking the "Complete" toggle visually updates the lesson sidebar, recalculates the progress percentage, and updates the global store.
*   **US-03 (Instructor):** As an instructor, I want to add a new lesson to my course so that students have new material.
    *   *Acceptance Criteria:* Form submission triggers a mock POST request, updates the local course state, and renders the new lesson in the curriculum list.

## 10. Core User Journeys
1.  **Student Enrollment:** User lands on `/` -> Navigates to `/login` -> Authenticates as Student -> Redirected to `/catalog` -> Clicks Course -> Views Details -> Clicks "Enroll" -> Global state updates -> Redirected to `/student/dashboard`.
2.  **Lesson Completion:** Student on `/student/dashboard` -> Clicks active course -> Opens `/student/course/[id]` (Player UI) -> Watches video -> Clicks "Mark Complete" -> Next lesson unlocks -> Progress bar increments.
3.  **Instructor Course Management:** User on `/login` -> Authenticates as Instructor -> Redirected to `/instructor/dashboard` -> Clicks "Create Course" -> Fills Details -> Submits -> Redirected to Course Edit View -> Adds Video URL -> Publishes.

## 11. Proposed Routes & Navigation Structure
*   **Public:** `/` (Landing), `/login`, `/register`
*   **Student:** `/catalog`, `/student/dashboard`, `/student/course/[courseId]` (Learning Player)
*   **Instructor:** `/instructor/dashboard`, `/instructor/course/new`, `/instructor/course/[courseId]/edit`

## 12. Complete Technology Stack
*   **Framework:** Next.js (App Router) — Provides robust client-side routing, SEO capabilities, and API route handlers for mock endpoints.
*   **Language:** TypeScript — Ensures type safety, interface enforcement across components, and fewer runtime errors.
*   **Styling:** Tailwind CSS — Utility-first CSS framework for rapid, responsive layout creation without leaving the JSX.
*   **UI Components:** shadcn/ui & Lucide React — Provides unstyled, highly accessible component primitives (Radix) tailored with Tailwind.
*   **Global State:** Zustand — A lightweight, hook-based state management library that avoids the boilerplate of Redux while providing scalable store slices.
*   **Mock Network:** Next.js Route Handlers — To simulate backend fetch calls without a real database.

## 13. Data Model Summary (Frontend State)
The Zustand store will manage mock JSON structures representing the application state:
*   `User`: `{ id: string, name: string, role: 'student' | 'instructor', token: string }`
*   `Course`: `{ id: string, instructorId: string, title: string, description: string, category: string, thumbnailUrl: string }`
*   `Lesson`: `{ id: string, courseId: string, title: string, videoUrl: string, order: number }`
*   `Progress`: `{ studentId: string, courseId: string, completedLessonIds: string[] }`

## 14. UI/UX Wireframes
The UI is designed mobile-first, targeting three core viewports: Authentication, Dashboards, and the Learning Player.

🔗 **Figma Wireframes:** `[Insert Figma Public Link Here]`

## 15. Architecture & State Tree
The application relies on a modular Zustand store structure interacting with Mock Next.js Route Handlers.

```mermaid
graph TD
    UI[React Components] -->|Dispatches Actions| Store[Zustand Root Store]
    
    Store --> AuthSlice[Auth Slice]
    Store --> CourseSlice[Course Slice]
    Store --> ProgressSlice[Progress Slice]
    
    AuthSlice -->|Reads/Writes| LocalStorage[(Local Storage Session)]
    
    CourseSlice -->|Fetches Mock Data| MockAPI[Next.js Route Handlers]
    ProgressSlice -->|Mutates Mock Data| MockAPI
    
    MockAPI -.->|Simulates DB| MockJSON[(Mock JSON DB)]# prodesk-capstone-EduCore
