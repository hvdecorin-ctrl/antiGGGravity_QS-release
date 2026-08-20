# antiG-QS — Command Guide

This is the F1 contextual-help reference for every antiG-QS command. Click **F1** on any antiG-QS ribbon button in Revit to land here.

antiG-QS is independent of any other antiGGGravity product: its own Hardware ID, its own activation keys, its own update channel. Nothing here applies to — or is affected by — a separate antiGGGravity license.

---

## Contents

**Quantity Survey**
- [QS Center](#qs-center)
- [Quantity Takeoff](#quantity-takeoff)
- [Quick Qty](#quick-qty)
- [BOQ Export](#boq-export)
- [Quantity Variations](#quantity-variations)
- [Elemental Map](#elemental-map)

**License**
- [Hardware ID](#hardware-id)
- [Request License](#request-license)
- [Check Update](#check-update)

---

## Before You Start

- **Everything in the Quantity Survey panel is read-only.** No command opens a transaction or changes your model — they only read and report.
- **Units** follow **NZS 4202:1995**: concrete in m³, formwork in m², reinforcement and structural steel in tonnes.
- **Elemental codes** follow the **NZIQS Elemental Analysis of Building Works (5th ed., 2017)** — shipped as an indicative starting map, not an authoritative standard. Override it per office or per project with **Elemental Map** (see below) or a hand-edited `antiGQS.QS.Elements.json`.
- **MEP and Architectural quantities read the current document only.** Linked consultant models (MEP, Architectural) are not traversed. If your structure is a host file with MEP/Architectural linked in rather than modelled locally, those quantities will read as zero — this is expected, not a bug.

---

## Quantity Survey

### QS Center

**What it does**
Opens the QS Intelligence Center — a single workspace with a left-hand navigation rail across seven sections: **Dashboard**, **Structural**, **Architectural**, **MEP**, **Quantity Intelligence**, **Model Validation**, and **Reporting Center**.

**How to use it**
1. Click **QS Center**. The workspace opens modeless — you can keep working in Revit while it's open.
2. It scans the model once on open. Use the footer's **Refresh Model Scan** button to re-scan after model changes — sections don't auto re-scan on every click.
3. **Dashboard** gives you the headline numbers across all disciplines.
4. **Structural / Architectural / MEP** give a thin, discipline-specific overview.
5. **Quantity Intelligence** is the full explorer: filter, group (up to multiple levels), search, and toggle which disciplines (Structural/Architectural/MEP) are included.
6. **Model Validation** lists every audit finding (see the audit rules noted under each takeoff type below) in one virtualized grid.
7. **Reporting Center** is where you jump to **Full Project Report** (opens Quantity Takeoff), **Bill Of Quantities** (opens BOQ Export), or **Quantity Variation** (opens Quantity Variations) with the current scan already loaded.

**Notes**
- Re-running the ribbon command activates the existing QS Center window instead of opening a second one.
- QS Center shares one scan across all its sections — this is what makes it fast on large models; it is not a substitute for re-running Quantity Takeoff if you need a snapshot to save.

---

### Quantity Takeoff

**What it does**
Measures concrete, formwork, reinforcement, structural steel, timber, MEP, and architectural finishes from the model, grouped by NZIQS elemental code.

**How to use it**
1. Click **Quantity Takeoff**.
2. Tick the measures you want under **Structural**, **MEP**, and **Architectural** — each row has its own checkbox, plus a master "select all" per row.
3. Click **Run**. Results appear grouped by elemental code, with unit and quantity per line.
4. From here you can jump straight into **BOQ Export** or save the result as a snapshot for later comparison in **Quantity Variations**.

**What gets measured**
- **Structural**: concrete volume, formwork area (gross contact area, no deductions for openings), reinforcement weight/length (via the same unit-weight table as Quick Rebar Q'ty — the figures always agree), structural steel and timber tonnage.
- **MEP** *(current document only)*: pipe/duct/cable tray/conduit by length and size band, duct lagging area (only if Ductwork is also ticked), fittings/valves/equipment as "no." lines, classified by System Classification/Type where assigned.
- **Architectural** *(current document only)*: doors/windows counted and sized (Width × Height where those parameters resolve), Room-based Wall/Floor/Ceiling Finish areas.

**Audit rules raised automatically**
| Rule | Severity | Meaning |
|---|---|---|
| QS-10 | Info | Fabric/mesh reinforcement is not measured |
| QS-12 | Warning | An MEP element's size could not be resolved — still measured, flagged "size unresolved" |
| QS-13 | Error | An MEP element has zero length — excluded entirely |
| QS-14 | Warning | A Room has zero area (Not Placed or unbounded) |
| QS-15 | Info | A Room's Floor/Wall/Ceiling Finish parameter is blank |
| QS-16 | Warning | A door/window's Width/Height didn't resolve — still counted as a "no." line |

**Notes**
- Structural steel tonnage excludes connections and fittings.
- The BOQ aggregates by code + measure + description + unit — two different MEP systems at the same size will currently roll into one BOQ line (System Classification/Type are still preserved per-item if you drill into Quantity Intelligence).

---

### Quick Qty

**What it does**
Gives you a fast total — length, weight, volume, and area, grouped by Family And Type — without running a full takeoff.

**How to use it**
1. Select elements first if you want a scoped total, or leave nothing selected to use the active view/schedule's own elements.
2. Click **Quick Qty**. The window opens showing whichever scope applies.
3. Use **Refresh From Selection** or **Refresh From View** to switch scope without closing and reopening the tool.

**What it auto-detects**
Structural Steel / Timber / Concrete / Masonry, Reinforcement, Pipework, Ductwork, or Containment — per element, automatically, with no setup.

**Notes**
- This is the fastest way to sanity-check a number mid-design — it's not a substitute for Quantity Takeoff when you need the full NZIQS-coded breakdown for a formal report.

---

### BOQ Export

**What it does**
Runs a takeoff with the default measurement scope (including the full QS audit) and exports it straight to a formatted Excel Bill of Quantities.

**How to use it**
1. Click **BOQ Export**.
2. Choose which disciplines to include (Structural / Architectural / MEP checkboxes).
3. Click **Export** and choose where to save. The workbook opens with the BOQ and the audit findings both included.

**Notes**
- If you need a non-default measurement scope (e.g. only Structural, or a specific set of MEP measures), run **Quantity Takeoff** first with the scope you want, then use its own export rather than the quick BOQ Export default.

---

### Quantity Variations

**What it does**
Compares two saved snapshots, or a snapshot against the live model, and reports the movement per BOQ line.

**How to use it**
1. Save a snapshot from Quantity Takeoff at a design milestone (give it a label — e.g. "IFC issue" or "50% DD").
2. Later, click **Quantity Variations**.
3. Pick two snapshots to compare, or pick one snapshot and compare it against "Live Model" to see what's changed since that milestone.
4. The report shows each BOQ line with its old quantity, new quantity, and the delta.

**Notes**
- This is the tool to reach for when a client or PM asks "what changed since last week" — it turns that question into an exact, line-by-line answer instead of a guess.

---

### Elemental Map

**What it does**
Writes an editable copy of the NZIQS elemental code map beside the model (or to `%ProgramData%\antiGQS\`) so your office's own coding convention can be applied without waiting on a new build.

**How to use it**
1. Click **Elemental Map**. It writes `antiGQS.QS.Elements.json` beside the current model (or to the ProgramData location for an office-wide default).
2. Edit the JSON file — each entry maps a category/measure to an elemental code and description.
3. Re-run any takeoff command; the override is picked up automatically, first-found-location wins (project file beside the model, then ProgramData).

**Notes**
- The shipped codes are indicative, not an office-confirmed NZIQS breakdown — this command exists specifically so you don't have to accept them as-is.
- Finishes (Wall/Floor/Ceiling) are classified per-measure, not per-category — a single Room can produce three different finish lines, each with its own code.

---

## License

antiG-QS's licensing is entirely independent — its Hardware ID, activation keys, and update channel do not overlap with any other antiGGGravity product you may have installed.

### Hardware ID

**What it does**
Copies your machine's unique Hardware ID to the clipboard — the identifier needed to issue you an activation key. This command is always free and always available, even without a license.

**How to use it**
1. Click **Hardware ID**.
2. Your ID is copied to the clipboard automatically.
3. Send it to **antiGGGravity.info@gmail.com** (via **Request License**, or manually) to receive your activation key.
4. Paste the key you receive into the activation prompt shown in the same window.

**Notes**
- The Hardware ID is a one-way hash of machine-specific hardware characteristics — it identifies the PC, not you personally, and cannot be reversed to reveal hardware details.
- A key issued for antiG-QS will only validate on the machine whose Hardware ID it was issued for, and will not activate any other antiGGGravity product (or vice versa) — the two use independent signing keys.

### Request License

**What it does**
Opens a simple form to request a trial or full license, pre-filling an email to support. This command is always free and always available, even without a license.

**How to use it**
1. Click **Request License**.
2. Enter your email and the license type you're after.
3. Click **Send Email** to launch your mail client with the request pre-filled, or **Copy Request** to paste it somewhere else (e.g. Teams, a support portal).
4. You'll receive your activation key by email — enter it via **Hardware ID**'s activation prompt once it arrives.

### Check Update

**What it does**
Checks for a newer antiG-QS release and, if one exists, downloads and stages it for the next Revit restart. This command is always free and always available, even without a license.

**How to use it**
1. Click **Check Update**.
2. If you're already on the latest version, you'll be told so.
3. If an update is available, choose **Update This Version Only** (just the Revit version you're currently running) or **Update All Versions** (every installed Revit year on this PC).
4. Restart Revit to apply the staged update.

**Notes**
- Update checks also run quietly in the background on startup — if a newer version is found, the Check Update button label changes to show it, so you don't have to click through to find out.
- This checks antiG-QS's own release channel only — it has no effect on, and is not affected by, any other antiGGGravity product's update status.
