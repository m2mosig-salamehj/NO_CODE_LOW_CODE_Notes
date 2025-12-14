## Benchmark Execution --- Lovable (Course CRUD Benchmark)

### 1\. Context and Objective

The objective of this benchmark is to evaluate how **Lovable**, an AI-assisted application builder, interprets and implements a small but structured CRUD specification using a single natural language prompt.\
The benchmark focuses not only on UI generation, but also on **implicit data modeling**, **relationships**, and **concept completeness**, which are core topics of the Coding Without Coding course.

The benchmark prompt used is intentionally simple and inspired by common software development tools such as GitHub or Linear.

**Benchmark prompt:**

> _I want a CRUD application with milestones, (sub)issues, labels, author, and assignees._

---

### 2\. First Interaction --- Initial Generation

Lovable was given the benchmark prompt without any additional clarification or constraints.

After a short generation phase, Lovable produced a fully functional **issue tracking application** with a modern, Linear-inspired dark UI. The generated application includes sidebar navigation, multiple views, and interactive modals.

📸 **Image 1 --- Global Issues View**\
_(Insert screenshot showing the "All Issues" page with multiple issues)_

At this stage, Lovable already supports:

- Creating, editing, and deleting issues

- Assigning status and priority

- Associating issues with milestones

- Adding labels with colors

- Displaying authors and assignees via avatars

The tool makes several **implicit assumptions**, notably that users already exist and that authentication is out of scope.

---

### 3\. Milestones and Labels Management

Lovable generates dedicated views for both **milestones** and **labels**, each with CRUD capabilities.

📸 **Image 2 --- Milestones Page**\
_(Insert screenshot showing milestones with progress bars and due dates)_

Milestones are not just static containers: Lovable automatically computes a completion percentage based on issue status. This indicates that the tool internally reasons about relationships between issues and milestones, even though this model is not explicitly exposed to the user.

📸 **Image 3 --- Labels Page**\
_(Insert screenshot showing label list with colors)_

Labels are treated as reusable entities and can be attached to issues in a many-to-many fashion.

---

### 4\. First Evaluation Against the Benchmark

After the first generation, most benchmark requirements are clearly satisfied:

- Issues ✔

- Milestones ✔

- Labels ✔

- Author ✔ (implicit)

- Assignees ✔ (multiple)

However, **sub-issues** are only mentioned by the tool and not visibly implemented in the UI. Issues appear as **flat entities**, without any parent--child relationship.

At this point, sub-issues are **claimed but not observable**, which motivates a controlled follow-up.

---

### 5\. Second Interaction --- Requesting Sub-Issues Explicitly

A second prompt was issued to Lovable:

> _Add support for sub-issues, where an issue can have child issues._

After this interaction, Lovable modifies the application by introducing a **Sub-issues section** inside the issue detail view.

📸 **Image 4 --- Issue Detail View with "Sub-issues (0)"**\
_(Insert screenshot showing the sub-issues section before creation)_

Lovable now allows the creation of a sub-issue linked to a parent issue.

📸 **Image 5 --- Create Sub-Issue Modal**\
_(Insert screenshot of the "Create Sub-issue" dialog)_

This confirms that Lovable is capable of modeling a **reflexive relationship** (Issue → Issue), at least at a minimal level.

---

### 6\. Observed Limitations of Sub-Issues

While sub-issues can be created and are displayed under their parent issue, several important limitations were observed:

📸 **Image 6 --- Sub-Issue Listed Under Parent Issue**\
_(Insert screenshot showing the sub-issue under the parent issue)_

- Sub-issues cannot be clicked to open a detailed view

- Sub-issues cannot be edited or deleted

- Sub-issues cannot themselves have sub-issues (no recursive hierarchy)

- Sub-issues do not appear in global issue lists

- Sub-issues cannot be searched or filtered

As a result, sub-issues behave as **second-class entities** rather than full issues.

Structurally, the model implemented by Lovable corresponds to:

`Issue
 └── SubIssue   (depth = 1 only)`

rather than a fully recursive issue hierarchy.

---

### 7\. Storage and Model Visibility

Lovable explicitly states that all data is stored **in memory** using Zustand. No persistence is enabled by default.

This has two important implications:

- The underlying data model is **hidden** from the user

- Users cannot directly inspect or modify relationships, cardinalities, or constraints

All modeling decisions are inferred and controlled by the tool.

---

### 8\. Summary and Interpretation

This benchmark shows that Lovable excels at rapidly generating a **convincing and usable CRUD interface** from a single natural language prompt. Core concepts such as issues, milestones, labels, and assignments are handled well with minimal user effort.

However, more advanced modeling concepts---particularly **recursive relationships** and **full lifecycle management of sub-entities**---are only partially supported. Sub-issues exist structurally but lack the completeness and autonomy of regular issues.

This highlights a key characteristic of AI coding assistants: they prioritize **speed and usability** over **explicit modeling control**, which can become a limitation when domain complexity increases.
