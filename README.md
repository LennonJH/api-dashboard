## What it does
1. Calls three API endpoints.
2. Processes all three using functions and loops
3. Prints clean dashboard using JSON

## Sample Use
```
#WEEK 5 PROJECT
#Building a mult-API dashboard from three endpoints using all the lesson learnt this week.

#STEP 1: PREVUEW ALL THREE ENDPOINT
def fetch_members():
    return [
        {"name": "James Omondi", "steps": 9200, "protocol": "OMAD", "sleep": 7.5, "cold_shower": True},
        {"name": "Sandra Weru", "steps": 10500, "protocol": "2MAD", "sleep": 8.0, "cold_shower": True},
        {"name": "Patrick Njiru", "steps": 8100, "protocol": "OMAD", "sleep": 6.5, "cold_shower": False},
        {"name": "Grace Achieng", "steps": 11000, "protocol": "OMAD", "sleep": 7.0, "cold_shower": True},
        {"name": "Brian Kamau", "steps": 7400, "protocol": "2MAD", "sleep": 9.0, "cold_shower": True},
        {"name": "Kevin Mwangi", "steps": 10800, "protocol": "OMAD", "sleep": 7.5, "cold_shower": True},
    ]

def fetch_weekly_summary():
    return {
        "week": "2024-W47",
        "daily_totals": [54200, 62000, 58400, 71000, 49600, 68000, 65200],
        "days": ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"],
        "member_count": 6
    }

def fetch_active_skills():
    return {
        "week": "2024-W47",
        "skills": [
            {"name": "welding",  "instructor": "Patrick Njiru", "enrolled": 8},
            {"name": "tiling",  "instructor": "James Omondi", "enrolled": 12},
            {"name": "copywriting", "instructor": "Sandra Weru", "enrolled": 15},
            {"name": "phone repair", "instructor": "Kevin Mwangi", "enrolled": 10},
            {"name": "beekeeping", "instructor": "Grace Achieng", "enrolled": 6},
        ]
    }

#Preview
members = fetch_members()
weekly = fetch_weekly_summary()
skills = fetch_active_skills()

print(f"Members endpoint: {len(members)} records")
print(f"Weekly endpoint: {len(weekly['daily_totals'])} days of data")
print(f"Skills endpoint: {len(skills['skills'])} active skills")

#PROCESS ALL THE THREE
def process_members(members, step_goal=10000):
    goal_met = [m for m in members if m["steps"] >= step_goal]
    avg_steps = round(sum(m["steps"] for m in members)/len(members))
    shower_count = sum(1 for m in members if m["cold_shower"])
    protocols = {}
    for m in members:
        p = m["protocol"]
        protocols[p] = protocols.get(p, 0) + 1
    return {
        "total": len(members),
        "goal_met": len(goal_met),
        "avg_steps": avg_steps,
        "cold_shower": shower_count,
        "protocols": protocols,
        "top": max(members, key=lambda m: m["steps"]) ["name"]
    }

def process_weekly(weekly):
    totals = weekly["daily_totals"]
    days = weekly["days"]
    best_idx = totals.index(max(totals))
    return {
        "best_day": days[best_idx],
        "best_total": max(totals),
        "weekly_avg": round(sum(totals)/len(totals)),
        "total_steps": sum(totals)
    }

def process_skills(skills_data):
    skills = skills_data["skills"]
    total_enrolled = sum(s["enrolled"] for s in skills)
    most_popular = max(skills, key=lambda s: s["enrolled"])
    return {
        "active_count": len(skills),
        "total_enrolled": total_enrolled,
        "most_popular": most_popular["name"],
        "top_enrollment": most_popular["enrolled"]
    }

#Run All Three
raw_members = [
    {"name": "James Omondi", "steps": 9200, "protocol": "OMAD", "sleep": 7.5, "cold_shower": True},
    {"name": "Sandra Weru", "steps": 10500, "protocol": "2MAD", "sleep": 8.0, "cold_shower": True},
    {"name": "Patrick Njiru", "steps": 8100, "protocol": "OMAD", "sleep": 6.5, "cold_shower": False},
    {"name": "Grace Achieng", "steps": 11000, "protocol": "OMAD", "sleep": 7.0, "cold_shower": True},
    {"name": "Brian Kamau", "steps": 7400, "protocol": "2MAD", "sleep": 9.0, "cold_shower": True},
    {"name": "Kevin Mwangi", "steps": 10800, "protocol": "OMAD", "sleep": 7.5, "cold_shower": True},
]
raw_weekly = {"week": "2024-W47", "daily_totals": [54200, 62000, 58400, 71000, 49600, 68000, 65200],
"days": ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"], "member_count": 6}
raw_skills = {"week": "2024-W47", "skills": [{"name": "welding","instructor": "Patrick Njiru","enrolled": 8},{"name": "tiling","instructor": "James Omondi","enrolled": 12},{"name": "copywriting","instructor":
"Sandra Weru","enrolled": 15},{"name": "phone repair","instructor": "Kevin Mwangi","enrolled": 10},{"name": 
"beekeeping","instructor": "Grace Achieng","enrolled": 6}]}

member_stats = process_members(raw_members)
weekly_stats = process_weekly(raw_weekly)
skill_stats = process_skills(raw_skills)

print("Member stats:", member_stats)
print("Weekly stats:", weekly_stats)
print("Skill stats: ", skill_stats)

#STEP 3: FULL DASHBOARD
import json

#---Data(simulated API responses)---
raw_members = [
    {"name": "James Omondi", "steps": 9200, "protocol": "OMAD", "sleep": 7.5, "cold_shower": True},
    {"name": "Sandra Weru", "steps": 10500, "protocol": "2MAD", "sleep": 8.0, "cold_shower": True},
    {"name": "Patrick Njiru", "steps": 8100, "protocol": "OMAD", "sleep": 6.5, "cold_shower": False},
    {"name": "Grace Achieng", "steps": 11000, "protocol": "OMAD", "sleep": 7.0, "cold_shower": True},
    {"name": "Brian Kamau", "steps": 7400, "protocol": "2MAD", "sleep": 9.0, "cold_shower": True},
    {"name": "Kevin Mwangi", "steps": 10800, "protocol": "OMAD", "sleep": 7.5, "cold_shower": True},
]
daily_steps = [54200, 62000, 58400, 71000, 49600, 68000, 65200]
day_names = ["Mon", "Tue", "Wed", "Thur", "Fri", "Sat", "Sun"]
skills_list = [
    {"name": "welding", "enrolled": 8},
    {"name": "tiling", "enrolled": 12},
    {"name": "copywriting", "enrolled": 15},
    {"name": "phone repair", "enrolled": 10},
    {"name": "beekeeping", "enrolled": 6},
]

#---Process---
STEP_GOAL = 10000
goal_met = [m for m in raw_members if m["steps"] >= STEP_GOAL]
avg_steps = round(sum(m["steps"] for m in raw_members) / len(raw_members))
showers = sum(1 for m in raw_members if m["cold_shower"])
best_day = day_names[daily_steps.index(max(daily_steps))]
top_skill = max(skills_list, key=lambda s: s["enrolled"])

#---Output
W = 52
print("=" * W)
print(f"  SMP COMMAND CENTRE DASHBOARD  |  Week 2024-W47")
print("=" * W)

print(f"\n  SECTION 1: MEMBER PERFORMANCE")
print(f"    {'Total members:':<28} {len(raw_members)}")
print(f"    {'HIT  {STEP_GOAL:,} step goal:':}")
print(f"    Hit {STEP_GOAL:,} step goal:   {len(goal_met)}/{len(raw_members)}")
print(f"    {'Average steps:':<28} {avg_steps:,}")
print(f"    {'Cold showers today:':<28} {showers}/{len(raw_members)}")
print(f"    Goal hitters: {', '.join(m['name'] for m in goal_met)}")

print(f"\n   SECTION 2: WEEKLY STEPS")
for day, total in zip(day_names, daily_steps):
    bar = "#" * (total // 5000)
    print(f"  {day:4} {total:>7,} {bar}")
print(f"  Best day: {best_day} ({max(daily_steps):,} total steps)")
print(f"  Week avg: {round(sum(daily_steps)/len(daily_steps)):,} steps/day")

print(f"\n   SECTION 3: ACTIVE SMP SKILLS")
for s in sorted(skills_list, key=lambda x: -x["enrolled"]):
    print(f"  {s['name']:15} {s['enrolled']} enrolled")
print(f"Most popular: {top_skill['name']} ({top_skill['enrolled']} enrolled)")

print(f"\n{'=' * W}")

#JSON export
export = {
    "week": "2024-W47",
    "members": {"total": len(raw_members), "goal_met": len(goal_met), "avg_steps": avg_steps},
    "weekly": {"best_day": best_day, "total_steps": sum(daily_steps)},
    "skills": {"active": len(skills_list), "most_popular": top_skill["name"]}
}
print("\nJSON export:")
print(json.dumps(export, indent=2))
```
## Sample Output
```
====================================================
  SMP COMMAND CENTRE DASHBOARD  |  Week 2024-W47
====================================================

  SECTION 1: MEMBER PERFORMANCE
    Total members:              6
    HIT  {STEP_GOAL:,} step goal:
    Hit 10,000 step goal:   3/6
    Average steps:               9,500
    Cold showers today:          5/6
    Goal hitters: Sandra Weru, Grace Achieng, Kevin Mwangi

   SECTION 2: WEEKLY STEPS
  Mon    54,200 ##########
  Tue    62,000 ############
  Wed    58,400 ###########
  Thur   71,000 ##############
  Fri    49,600 #########
  Sat    68,000 #############
  Sun    65,200 #############
  Best day: Thur (71,000 total steps)
  Week avg: 61,200 steps/day

   SECTION 3: ACTIVE SMP SKILLS
  copywriting     15 enrolled
  tiling          12 enrolled
  phone repair    10 enrolled
  welding         8 enrolled
  beekeeping      6 enrolled
Most popular: copywriting (15 enrolled)

====================================================

JSON export:
{
  "week": "2024-W47",
  "members": {
    "total": 6,
    "goal_met": 3,
    "avg_steps": 9500
  },
  "weekly": {
    "best_day": "Thur",
    "total_steps": 428400
  },
  "skills": {
    "active": 5,
    "most_popular": "copywriting"
  }
}
```
## Stack
Python, JSON, API
