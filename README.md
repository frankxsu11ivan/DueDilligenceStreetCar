# DueDilligenceStreetCar
Due Dilligence
Crafting flowchart detailsI'm piecing together options for generating a flowchart and checklist, considering Python and Python-based tools to ensure precise visualizations and an option for a Markdown checklist.Crafting the flowchartOK, let’s draw a flowchart using Python's matplotlib. Rectangles and decision diamonds with arrows will be included, and I'll save the final image as 'due_diligence_unknown_utility_flowchart.png'.Coding and executingPythonimport matplotlib.pyplot as plt
from matplotlib.patches import Rectangle, FancyArrow, Polygon
from matplotlib.lines import Line2D

# Helper functions

def add_process(ax, xy, text, width=3.6, height=0.9):
    x, y = xy
    rect = Rectangle((x - width/2, y - height/2), width, height, linewidth=1.5, edgecolor='black', facecolor='#E8F0FE')
    ax.add_patch(rect)
    ax.text(x, y, text, ha='center', va='center', fontsize=9, wrap=True)
    return (x, y, width, height)


def add_decision(ax, xy, text, width=3.6, height=1.2):
    x, y = xy
    hw, hh = width/2, height/2
    diamond = Polygon([[x, y+hh], [x+hw, y], [x, y-hh], [x-hw, y]], closed=True, linewidth=1.5, edgecolor='black', facecolor='#FFF4CC')
    ax.add_patch(diamond)
    ax.text(x, y, text, ha='center', va='center', fontsize=9, wrap=True)
    return (x, y, width, height)


def arrow(ax, start, end, text=None, text_offset=(0,0), style='-|>', connectionstyle='arc3', color='black'):
    x1, y1 = start
    x2, y2 = end
    arr = FancyArrow(x1, y1, x2-x1, y2-y1, width=0.0001, head_width=0.18, head_length=0.24, length_includes_head=True, color=color)
    ax.add_patch(arr)
    if text:
        ax.text((x1+x2)/2 + text_offset[0], (y1+y2)/2 + text_offset[1], text, fontsize=8, ha='center', va='center', color=color)

# Build the flowchart
plt.figure(figsize=(10, 16))
ax = plt.gca()
ax.set_xlim(0, 10)
ax.set_ylim(0, 30)
ax.axis('off')

# Title
ax.text(5, 29.2, 'Due Diligence for Unknown Utility (Power, Gas, Water)', ha='center', va='center', fontsize=14, fontweight='bold')

# Section headers
ax.text(0.5, 27.8, '#1 Research', ha='left', va='center', fontsize=11, fontweight='bold')
ax.text(0.5, 17.2, '#2 GIS Mapping', ha='left', va='center', fontsize=11, fontweight='bold')
ax.text(0.5, 12.2, '#3 Visual Identification', ha='left', va='center', fontsize=11, fontweight='bold')

# Start
start = add_process(ax, (5, 26.5), 'Start: Unknown utility encountered (Power / Gas / Water)')

# Research 1: PointMan & Boots on the Ground
p1 = add_process(ax, (5, 25), 'Research 1: PointMan app + boots on the ground at suspected utility location')
d1 = add_decision(ax, (5, 23.6), 'Is this utility in PointMan line work?')

# Yes branch from d1
p1_yes = add_process(ax, (2.7, 22.1), 'If YES: Document attributes (owner, material, size, depth) and proceed to validation')

# No branch from d1
p1_no = add_process(ax, (7.3, 22.1), 'If NO: Flag as unverified; continue with 811 + local history check')

# Research 2: 811
p2 = add_process(ax, (5, 20.6), 'Research 2: Call 811. Request locates and utility identification')
d2 = add_decision(ax, (5, 19.2), 'Does 811 identify the utility?')

# Yes from d2
p2_yes = add_process(ax, (2.7, 17.7), 'If YES: Record ticket, owner, facility type, and marks; schedule validation as needed')

# No from d2
p2_no = add_process(ax, (7.3, 17.7), 'If NO: Treat as potentially private/unknown; escalate to local contacts')

# Research 3: Call Mark
p3 = add_process(ax, (5, 16.2), 'Research 3: Call Mark for history; meet on site if possible')
d3 = add_decision(ax, (5, 14.8), 'Does Mark know this utility?')

# Yes from d3
p3_yes = add_process(ax, (2.7, 13.3), 'If YES: Capture history (owner, status, conflicts) and proceed to mapping & visual check')

# No from d3
p3_no = add_process(ax, (7.3, 13.3), 'If NO: Maintain unknown status; increase caution and plan field validation')

# Section 2: GIS Mapping
m1 = add_process(ax, (5, 11.0), 'GIS Mapping: Check internal/project GIS and public datasets')
d4 = add_decision(ax, (5, 9.6), 'Does GIS mapping identify the utility?')

# Yes from d4
m1_yes = add_process(ax, (2.7, 8.1), 'If YES: Align field marks with GIS; verify attributes and note discrepancies')

# No from d4
m1_no = add_process(ax, (7.3, 8.1), 'If NO: Keep listed as unknown; prioritize field verification')

# Section 3: Visual Identification
v1 = add_process(ax, (5, 6.2), 'Visual Identification: Inspect corridor for signs of abandonment or activity')
d5 = add_decision(ax, (5, 4.8), 'Are there indicators the utility is abandoned?')

v1_notes = add_process(ax, (5, 3.2), 'Indicators: a) cut line, b) drilled-through (not energized), c) hot patching aligned with utility')

# Termination / Next steps
end_yes = add_process(ax, (2.7, 1.7), 'If ABANDONED LIKELY: Verify with owner if possible; otherwise pothole/test point before decisions')
end_no = add_process(ax, (7.3, 1.7), 'If ACTIVE/POSSIBLY ACTIVE: Treat as live; schedule test holes, EM detection, and conflict mitigation')

# Connectors
# Start -> p1
arrow(ax, (5, 26.0), (5, 25.45))
# p1 -> d1
arrow(ax, (5, 24.55), (5, 24.05))
# d1 to yes/no
arrow(ax, (4.2, 23.6), (3.2, 22.7), text='Yes', text_offset=(-0.2, 0.1))
arrow(ax, (5.8, 23.6), (6.8, 22.7), text='No', text_offset=(0.2, 0.1))

# Merge to 811
arrow(ax, (3.2, 21.65), (4.2, 20.9))
arrow(ax, (6.8, 21.65), (5.8, 20.9))
# to p2
arrow(ax, (5, 20.15), (5, 19.65))
# d2 yes/no
arrow(ax, (4.2, 19.2), (3.2, 18.5), text='Yes', text_offset=(-0.2, 0.1))
arrow(ax, (5.8, 19.2), (6.8, 18.5), text='No', text_offset=(0.2, 0.1))
# merge to p3
arrow(ax, (3.2, 17.25), (4.2, 16.5))
arrow(ax, (6.8, 17.25), (5.8, 16.5))
arrow(ax, (5, 15.75), (5, 15.25))
# d3 yes/no
arrow(ax, (4.2, 14.8), (3.2, 14.1), text='Yes', text_offset=(-0.2, 0.1))
arrow(ax, (5.8, 14.8), (6.8, 14.1), text='No', text_offset=(0.2, 0.1))
# to GIS
arrow(ax, (3.2, 12.85), (4.2, 11.6))
arrow(ax, (6.8, 12.85), (5.8, 11.6))
arrow(ax, (5, 10.55), (5, 10.05))
# d4 yes/no
arrow(ax, (4.2, 9.6), (3.2, 8.9), text='Yes', text_offset=(-0.2, 0.1))
arrow(ax, (5.8, 9.6), (6.8, 8.9), text='No', text_offset=(0.2, 0.1))
# to Visual
arrow(ax, (3.2, 7.65), (4.2, 6.8))
arrow(ax, (6.8, 7.65), (5.8, 6.8))
arrow(ax, (5, 5.75), (5, 5.25))
# d5 to notes
arrow(ax, (5, 4.2), (5, 3.65), text=None)
# split from notes to outcomes
arrow(ax, (4.2, 2.75), (3.2, 2.2), text='Likely Abandoned', text_offset=(-0.2, 0.1))
arrow(ax, (5.8, 2.75), (6.8, 2.2), text='Active/Possible', text_offset=(0.2, 0.1))

# Legend
legend_items = [
    Line2D([0], [0], color='black', lw=1.5),
    Rectangle((0,0),1,1, facecolor='#E8F0FE', edgecolor='black'),
    Polygon([[0,0],[0,0],[0,0],[0,0]], closed=True, facecolor='#FFF4CC', edgecolor='black')
]
ax.legend(legend_items, ['Flow', 'Process', 'Decision'], loc='lower center', bbox_to_anchor=(0.5, 0.03), ncol=3, frameon=False)

plt.tight_layout()
plt.savefig('due_diligence_unknown_utility_flowchart.png', dpi=200, bbox_inches='tight')
print('Saved: due_diligence_unknown_utility_flowchart.png')
Status: Success

Output: Saved: due_diligence_unknown_utility_flowchart.png

Crafting checklistPiecing together a clear checklist with headers and bullet items, including export options to Word/PDF/Excel, and incorporating an image as per citation instructions.Generating response Awesome—on it, Frank. I put this into two parts for you:

1) Flowchart — “Due Diligence for Unknown Utility (Power, Gas, Water)”
Here’s a clean flow that matches your steps and wording for Research, GIS Mapping, and Visual Identification.
Download/preview:


2) Field Checklist — “Due Diligence for Unknown Utility”

Scope: Unknown power, gas, or water lines.
Goal: Determine owner/status and reduce risk (avoid hits, confirm abandonment vs active, plan validation).

#1 — Research
1.1 PointMan + Boots on the Ground

☐ Open PointMan at suspected utility location
☐ Walkdown corridor (paint, tags, pedestals, valves, meters, handholes, risers, tracer wire, rectifiers, vents, hydrants, etc.)
Decision: Is this utility in PointMan line work?

☐ Yes → ☐ Document attributes (Owner, Facility type, Material, Size, Depth/elev., Last update, Confidence)
☐ No → ☐ Flag as unverified and continue to 811 + history



1.2 811 Locate Ticket

☐ Call 811 and obtain ticket # __________
☐ Request facility type(s) that align with the observed alignment
Decision: Does 811 identify the utility?

☐ Yes → ☐ Record Owner/Operator, Facility type, Mark colors, Contact
☐ No → ☐ Treat as private/unknown; plan local history review & field validation



1.3 Local Knowledge — Call Mark & Meet on Site

☐ Call Mark (history + on-site review) — time/date __________
Decision: Does Mark know this utility?

☐ Yes → ☐ Capture history (Owner, Status, Approx. install/abandon date, Conflicts)
☐ No → ☐ Maintain unknown status; increase caution and schedule validation




#2 — GIS Mapping

☐ Check internal/project GIS layers
☐ Check available public datasets (municipal, county, DOT, utility portals if available)
Decision: Does GIS mapping identify the utility?

☐ Yes → ☐ Align field marks with GIS centerline; ☐ Note discrepancies (offset, depth, size, material)
☐ No → ☐ Keep as unknown; ☐ Prioritize field verification (test holes / detection)




#3 — Visual Identification (Abandonment Indicators)

☐ Corridor inspection for physical signs:

a) ☐ Cut line (visible severed pipe/cable with clear termination)
b) ☐ Hole drilled through indicating not energized (e.g., vent/drilled cut-back; still treat with caution)
c) ☐ Hot patching aligned with utility path (prior cutouts/repairs along alignment)


Decision: Are there indicators the utility is abandoned?

☐ Likely Abandoned →

☐ Attempt owner confirmation (if known)
☐ Pothole/Test point before design or removal decisions
☐ Update notes and status in PointMan/GIS


☐ Active/Possibly Active →

☐ Treat as live
☐ Schedule test holes (vac, cores), EM detection/locating
☐ Develop conflict mitigation (offsets, depth checks, design change)






Documentation (All Stages)

☐ Photos (context + close-ups)
☐ Sketch with offsets/references (centerline, back-of-curb, ROW)
☐ Coordinates / stationing / depth notes
☐ Contacts, ticket numbers, dates
☐ Final status: Known Owner / Unknown / Likely Abandoned / Active
☐ Next actions & responsible party


Want it exported?
I can package this into:

Word (DOCX): one-page checklist + embedded flowchart
PDF: print-ready sheet
Excel: checkbox checklist for tracking across multiple locations
PowerPoint: for project reviews

Tell me your preference, and I’ll generate the file(s) with your logo/header and any project identifiers you want (e.g., corridor name, project ID).Provide your feedback on BizChat
