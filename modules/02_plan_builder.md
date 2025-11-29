### Module 2 — Plan Builder (Options → Days)

**Goal**

Turn a list of candidate activities into a day-by-day itinerary with
Morning, Midday, Afternoon, and Evening slots.

**Inputs**

- Number of days in the trip.
- Lodging location (home base).
- List of candidate activities, each with:
  - name and type (e.g., attraction, park, restaurant, event)
  - location (or neighbourhood)
  - estimated duration
  - price / cost range
  - typical opening hours or time-of-day suitability
- Optional: daily budget and user preferences (e.g., likes museums, hates nightlife).

**Output**

- A list of days, each with:
  - Morning activity (or “Free / unassigned”)
  - Midday activity (or “Free / unassigned”)
  - Afternoon activity (or “Free / unassigned”)
  - Evening restaurant or event (or “Free / unassigned”)

**Preprocessing**

- Group activities by location:
  - “Near lodging” = same neighbourhood or short travel time from lodging.
  - “Close by” = short travel time from the previous activity.
- Optionally mark activities as “daytime-friendly” vs “evening-friendly”.

**Algorithm**

For each day in the trip:

1. Start with an empty day plan and reset the remaining daily budget/time.

2. **Pick Morning activity**
   - Choose an activity that:
     - is near lodging,
     - fits in the morning time window,
     - fits within remaining budget.
   - Prefer activities matching user preferences.
   - Mark the activity as used so it is not scheduled again.

3. **Pick Midday activity**
   - From remaining activities, choose one that:
     - is close to the Morning activity or lodging,
     - fits after travel time from Morning,
     - keeps the total cost within the daily budget.
   - If possible, vary the type (e.g., not two similar museums in a row).
   - Mark as used.

4. **Pick Afternoon activity**
   - Choose an activity that:
     - offers a different theme or type from earlier in the day,
     - is reachable from the Midday location,
     - fits in the remaining time and budget.
   - Mark as used.

5. **Pick Evening restaurant or event**
   - Prefer restaurants or events:
     - near the last activity or near lodging,
     - suitable for evening.
   - If none are available, leave Evening as “Free / choose any nearby restaurant later”.

6. **Handle missing options**
   - If no valid activity is found for a slot (e.g., budget or time too tight),
     set that slot to “Free / unassigned” and optionally note the reason.

7. Save the completed day plan and move on to the next day.
