I want to build a small project that gives me hands-on familiarity with modern TypeScript, React, Next.js, and the Vercel AI SDK.

Project: Message-to-Action Assistant

Goal:
Build a compact Next.js application where a user pastes text from an email, text message, note, or book passage. The application should use an LLM to identify whether the text implies an actionable task, extract the relevant details, flag missing or ambiguous information, and propose a structured action for human review.

Examples:
- “Can we grab coffee next Thursday around 6?” → propose calendar event
- “Remind me to return the library book by August 3.” → propose reminder
- “Please send Sarah the revised deck tomorrow.” → propose task
- A book passage containing no personal action → classify as informational
- “Let’s talk sometime next week.” → flag missing date/time

This is a learning project, not a production personal assistant. Keep it small enough to complete in a few hours.

Stack:
- Next.js App Router
- TypeScript
- React
- Vercel AI SDK
- OpenAI provider
- Zod
- Tailwind
- No database initially
- No authentication
- No real Gmail, iMessage, calendar, or reminder integrations in v1
- No Trigger.dev unless the basic version is complete

Core flow:
1. User selects a source type: email, message, note, or passage.
2. User pastes the text into a form.
3. The client submits it to a Next.js Route Handler.
4. The server uses the Vercel AI SDK to extract structured output.
5. The output is validated with Zod.
6. The UI displays:
   - detected intent
   - confidence
   - title
   - people involved
   - proposed date and time
   - location
   - source evidence
   - missing information
   - ambiguities
   - proposed action
7. The user can approve, edit, or reject the proposal.
8. Approval calls a fake deterministic tool such as:
   - previewCalendarEvent()
   - previewReminder()
   - previewTask()
9. The tool returns a mocked structured result. It must not use the LLM to perform the final action.

Suggested intents:

type Intent =
  | "schedule_event"
  | "create_reminder"
  | "create_task"
  | "follow_up"
  | "informational"
  | "unknown";

Suggested schema:

interface MessageAnalysis {
  intent: Intent;
  confidence: number;
  title?: string;
  people: string[];
  date?: string;
  time?: string;
  location?: string;
  sourceType: "email" | "message" | "note" | "passage";
  evidence: Array<{
    field: string;
    excerpt: string;
  }>;
  missingFields: string[];
  ambiguities: string[];
  proposedAction: string;
}

Architecture constraints:
- Keep LLM interpretation separate from deterministic action execution.
- Validate model output with Zod.
- Keep the API key server-side.
- Use a Route Handler instead of calling the model from the browser.
- Clearly represent idle, loading, error, review, approved, edited, and rejected states.
- Do not introduce a database, vector search, autonomous agent loop, or background jobs in v1.
- Use straightforward React state without a global state library.
- Prefer simple, readable code over abstractions.
- Treat dates carefully. The model should extract what the message says, while deterministic code should validate and normalize dates before approval.

Suggested file structure:

app/
  page.tsx
  api/analyze/route.ts
components/
  message-form.tsx
  analysis-review.tsx
  action-preview.tsx
lib/
  schemas.ts
  analyze-message.ts
  action-tools.ts
  date-validation.ts
  sample-messages.ts

Please proceed in stages:
1. Propose the architecture and file structure.
2. Explain the relevant Next.js, TypeScript, Zod, and AI SDK concepts.
3. Scaffold the project.
4. Implement the smallest working vertical slice:
   pasted text → structured analysis → review screen → mocked action preview.
5. Run lint, type-check, tests where useful, and production build.
6. Explain every major implementation decision.
7. Identify two sensible v2 extensions, but do not build them yet.

I want to actively learn. Pause after each major stage and ask me to inspect, explain, or modify one small part myself rather than generating the entire project without explanation.