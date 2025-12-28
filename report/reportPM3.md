## Note
This document has been adapted for portfolio purposes. Content authored by other team members has been removed, and the remaining sections reflect only my individual contributions. The original complete report was submitted jointly as part of the team project for UBC DSCI 320.


### Dataset Description
The Canadian National Fire Database (CNFDB) is a comprehensive collection of forest fire locations and perimeters compiled from Canadian fire management agencies across provinces, territories, and Parks Canada. The database is standardized into a common format for Canada-wide analysis, though it contains only fires that agencies have chosen to report and map. The original dataset includes forest fire records from 1930–2023. Our analysis focuses on a sampled subset (n = 20,000) with observations between 1950 and 2023.

### Intended Audience
Our intended audience is the general Canadian public. These are people who are curious about wildfires in their country or even concerned citizens wanting to understand fire patterns in their region. We expect this project will help Canadians visualize and understand where and when major wildfires have occurred across the country, turning complex national fire data into meaningful and interpretable plots. By presenting this information in an engaging, interactive format, we aim to increase public awareness of Canada’s fire history and patterns. This understanding is particularly relevant given increased public attention to wildfire seasons and their impact on communities, air quality, and ecosystems.

### General Takaways
The data tells a story of transformation in Canada's fire landscape. We are on a downtrend with fewer fires are occurring, but when they do ignite, they're burning with greater intensity across larger areas, with certain causes, such as human and natural, showing a stronger association with these more severe events. These findings reveal the need for fire management strategies that prioritize containment capabilities over frequency-based resource planning. Additionally, some crucial attributes were missing values, highlighting the imporance of proper data collection strategies when dealing with data that has real-world impacts.

| **Attribute Name** | **Attribute Type** | **Data Semantics** | **Cardinality** |
|--------------------|--------------------|--------------------|-----------------|
| **NFDBFIREID** | Nominal | Constructed ID combining province-year-fire_id | 436,564 |
| **SRC_AGENCY** | Nominal | Province/Territory/parks code | 13 (BC, AB, SK, MB, ON, QC, NS, NB, NL, YT, NT, PEI, PC) |
| **NAT_PARK** | Nominal | National park identifier (if applicable) | 48 unique values |
| **FIRE_ID** *(Recurring fire?)* | Ordinal | Constructed ID of year-fire_id. Fire identifier | 329,627 unique values |
| **FIRENAME** | Nominal | Name or location description of fire (if applicable) | 28,913 unique values |
| **YEAR** | Quantitative / Temporal | Calendar year of fire occurrence | 80 unique years [1950–2024] |
| **MONTH** | Quantitative | Month of fire occurrence | 12 [1–12] |
| **DAY** | Quantitative | Day of fire occurrence | ~31 [1–31] |
| **LATITUDE** | Quantitative | Geographic coordinate (North–South) | Continuous [-16.03 – 90.00] |
| **LONGITUDE** | Quantitative | Geographic coordinate (East–West) | Continuous [-180.00 – 180.00] |
| **SIZE_HA** | Quantitative | Fire size in hectares | [0.001–44.5+] |
| **CAUSE** | Nominal | Specific fire cause category <br> 5 unique values [H, H-PB, N, RE, U] <br> H = Human <br> H-PB = Human Prescribed Burn <br> N = Natural <br> RE = Reignition <br> U = Undetermined |  |
| **FIRE_TYPE** | Ordinal / Nominal | Type of fire event (different from severity; if includes severity, then ordinal) <br> 27 unique values [Fire, IFR, PB, 4, 5, Grass, Forest, Other, Wildfire, Mutual Aid, Dump, 99, Type 3, Type 5, Type 4, Type 1, Type 2, Ground, Surface, Crown, Req For Assist, Request for Assist, 1, 2, 3, Prescribed Burn, OFR] |  |
| **RESPONSE** | Nominal / Ordinal? | Response type to fire (might be ordinal as response can escalate in priority) <br> 10 unique values [FUL, MON, MOD, MNP, MDP, No Action, Actioned, Limited Action, SUP, PRO] <br> (Could split into another ordinal column) |  |
| **PROTZONE** | Nominal | Protection zone classification | 37 unique values |
| **PRESCRIBED** *(Filtered version of FIRE_TYPE)* | Ordinal | Whether fire was a prescribed burn <br> 3 unique values [N/A, No, PB] |  |
| **REP_DATE** | Temporal | Date fire was reported (if applicable) | 18,212 unique values |
| **OUT_DATE** | Temporal | Date fire was declared out | 12,393 unique values |
| **ATTK_DATE** | Temporal | Date fire suppression began | 60 unique values |
<br>

## Xinghao Huang - Understanding Fire Severity Across Canada

### View 1: Wildfire Event Frequency Over Time by Fire Cause

#### Questions and Low-Level Tasks

- Analytic Question 1 "How do wildfire frequency vary across regions and causes, and how do these patterns change over time"
- Tasks:
    - Characterize Distribution (Fire Event Frequency): How are fire events distributed geographically across Canada?
    - Compare (Regional Differences): How does fire activity differ between provinces in terms of fire cause? 
    - Characterize (Cause Composition): What is the distribution of fire causes within a selected province? 
    - Compare (Regional Fire Cause Contrast): How does the proportion of different fire causes differ within a province? 
    - Filter (Temporal Exploration): How does the spatial distribution of fire events change from 1940 to 2023?
    - Find Anomalies (Spatial-Temporal): Which provinces show unusually high or unusually low fire activity in certain years?
    - Identify Outliers (Cause Frequency): Are there provinces where a single fire cause overwhelmingly dominates compared to others?
    - Cluster (Spatial Patterns): Do clusters of high fire activity appear in certain regions during specific years?

#### The Actual Visualization
![Dashboard 1 Demo](../images/dashboard%20gifs/Dashboard1XH.gif?v=2)

*Note: The gif is 59 seconds in total*

#### View Description
- This view consists of two visualizations arranged in a left–right layout:
- Chart 1 – Layered Geographic Point Map (Left):
    - Mark: Filled geographic shapes (provinces) + point marks (individual fire events)
    - Channels Exploited:
        - Position (X, Y): Latitude & longitude - quantitative spatial encoding that accurately places fire events geographically.
        - Color (Province fill): Selection state (full highlight for selected province; muted gray for others).
        - Color (Fire points): Fire cause (nominal).
        - Opacity: Used to fade points outside the selected province or year.
        - Tooltip (point): Display summary information about year, month, cause, fire size, and agency/province.
- Chart 2 - Fire Cause Frequency Bar Chart (Right):
    - Mark: Bar
    - Channels Exploited:
        - Position (X): Fire cause (nominal).
        - Position (Y): Frequency of fire cause (quantitative).
        - Color: Fire cause (nominal).
        - Tooltip: Display summary information about cause, frequency, and average fire size.
- Suitability: These choices are well-suited for the intended analytic tasks because:
    - Geographic map captures spatial fire patterns and enables clear comparison of where events occur across provinces.
    - Point marks encode individual fire events and provides precise geographic placement of each record on the map.
    - The bar chart allows comparison of how fire frequency varies between different fire causes.
    - Connection between map and bar chart reinforce cross-view relationships, helping users connect spatial patterns with cause distributions.
    - Opacity adjustments reduce clutter while highlighting relevant marks, ensuring that selected regions and causes remain visually dominant.
    - Tooltips provide additional information about detailed inspection of individual fire events or cause summaries without cluttering the visualization.
 
#### Interaction and Characteristics
- Province Selection (Click):
    - Type: Point selection on categorical geographic units (province polygons).
    - Scope: Affects both the geographic fire-event map and the fire-cause summary bar chart.
    - Feedback:
        - Selected provinces are highlighted in yellow/gold and others become muted gray.
        - Only fire events from the selected province(s) remain fully visible.
        - The bar chart updates to show cause distribution within the selected province(s).
    - Enables:
        - Linking spatial patterns with cause patterns of the selected provinces, which ensures users focus on a single region without losing the broader spatial details.
        - Allows users to look closely at one selected province and see a detailed breakdown of fire events by cause.
        - Filters the data and understands how different fire causes contribute to the overall distribution in that province.
- Cause Selection (Click):
	- Type: Point selection on fire cause
	- Scope: Affects the points on the map.
	- Feedback: When a cause is clicked in the bar chart, all fire events with that cause are highlighted in their assigned color on the map. The non-selected fire events fade on the map.
	- Enables:
		- Allows users to directly compare the spatial distribution of fires by cause.
		- Makes it easy to spot geographic clustering for each cause (e.g., lightning-heavy regions).
- Year Slider (Global Temporal Control):
    - Type: Numeric/temporal parameter bound to a slider.
    - Scope: Filters both map points and bar-chart counts.
    - Feedback:
        - Fire points on the map update instantly to reflect the chosen year (i.e., only the fire events that happened in the selected year are presented on the map).
        - The bar chart redisplays cause frequency for that year.
    - Enables:
        - Allows users to explore how the spatial distribution of fire events changes year by year.
        - Helps users focus on the frequency distribution of the selected provinces within a specific year by updating the bar chart
- Show All Years Checkbox (Global Override):
    - Type: Boolean parameter with checkbox binding.
    - Scope: Overrides temporal filtering for all charts.
    - Feedback:
        - When unchecked, charts revert to year-specific filtering, making the year slider workable.
        - When checked, the overall distribution of fire events is shown simultaneously, and it overrides the year slider (i.e., the year slider will not work if this checkbox is checked).
    - Enables:
        - Provides an overview of the spatial distribution of fire events from 1940 to 2023 within the sample.
        - Allows quick comparison between overall long-term patterns and patterns from individual years.

#### Important Note for Dashboard 1
- AI models were used to help match province names between the source data and our dataset, since the standard approach from the Altair documentation was not working as expected.
- AI models were used to help connect the map and bar chart and to debug unexpected errors that occurred during the process.

#### Critique of View
- Strengths:
    - The format is visually engaging, which helps maintain user attention and supports smoother exploration of the different patterns in the data.
    - The map places each fire event at its real latitude and longitude, making it easy for users to compare regions and understand the spatial patterns.
    - Opacity and province-fill highlighting provide distinguishable visual feedback, which allows users to see which region is selected and how the data updates.
    - The color scheme for each fire cause stays consistent across all filters, which makes the visualization easier to read and helps users quickly recognize patterns.
    - The bar chart effectively summarizes the frequency of each fire caused by the selected provinces.
    - The interaction between the map and the bar chart strengthens the connection between spatial patterns and cause distributions, helping users understand how different regions relate to specific fire causes.
    - Users can choose to view either the overall distribution or the detailed distribution for a specific year, depending on what they prefer to explore.
    - The ability to click either a cause in the bar chart or a fire event on the map and have both views update simultaneously creates a strong sense of coordination between spatial and categorical attributes. This reduces cognitive load by eliminating the need for users to manually cross-reference causes between views.

- Weaknesses:
    - In high-density regions (like BC and AB), many points appear very close to each other, making it difficult to distinguish individual fire events or inspect them by hovering.
    - Fire events from non-selected provinces remain visible and can still be inspected by hovering, which may distract users and affect how they perceive the fire events within the selected province.
    - Two-way linking may momentarily confuse users, as it is not always obvious whether the map or the bar chart triggered the current highlight state.
    - Year selection only allows one year at a time, which limits the ability to explore multi-year or decade-level trends. Users must remember the distributions from different years if they want to compare them across time.
    - Comparing the overall distribution with a specific year is not intuitive because both views cannot be shown at the same time, and users must go back and forth between them to see the differences.
    - The view uses a 20,000-record sample from a ~500,000-record dataset, and the sample may not be fully representative, especially some provinces that were filtered out intentionally.
     - Some of the colors used for fire causes and maps may be difficult for color-blind users to distinguish, which can reduce clarity and make the visualization less accessible.

### View 2: Average Wildfire Size by Severity: Regional and Cause-Based Comparisons

#### Questions and Low-Level Tasks
- Analytic Question 2: How does fire severity change across provinces and fire causes, and which fire events appear unusually severe compared to typical patterns across Canada?
- Tasks:
    - Characterize Distribution (Severity Patterns): How are wildfire sizes distributed across provinces and fire causes?
    - Compare (Regional Severity Differences): How does the average fire size vary across different provinces?
    - Compare (Cause-Based Severity): How do fire sizes differ between human-caused, natural, prescribed burn, re-ignition, and unknown causes?
    - Characterize (Extreme Events): What do the most extreme fire events look like in terms of size, and where do they occur?
    - Identify Outliers (Extreme Fires): Are there provinces or causes that contain unusually large or unexpected fires compared to national patterns?
    - Cluster (Severity Patterns Across Regions): Do provinces form natural groupings based on average fire size?
    - Cluster (Cause-Based Severity Patterns): Do certain fire causes consistently result in larger or smaller fires?
    - Filter (Severity-Based Exploration): How do severity categories (e.g., small, medium, large, extreme) help reveal different trends in fire size?
    - Find Anomalies (Unexpected Patterns): Which provinces or causes show severity trends that deviate from the expected national pattern?
    - Determine Range (Severity Distribution Insight): What is the range of fire sizes observed across regions and causes?
    - Summarize (Severity Overview): What are the overall national trends in wildfire severity regardless of region or cause?
 
#### The Actual Visualization
![Dashboard 2 Demo](../images/dashboard%20gifs/Dashboard2XH.gif)

*Note: The gif is 50 seconds in total*

#### View Description
- This view consists of three visualizations arranged in a vertical-plus-horizontal layout, with a jittered strip plot at the top and two bar charts placed side-by-side under the jittered strip plot.”
- Chart 1 - Jittered Strip Plot: Fire Size by Province (Top)
    - Mark: Circle
    - Channels Exploited: 
        - Position (X): Fire size (Quantitative), log-scale.
        - Position (Y): Province/Agency (Nominal).
        - Jitter yOffset: Makes individual points distinguishable, especially in dense provinces.
        - Color: Fire Cause (Nominal).
        - Tooltip: Display summary information about province/agency, fire cause, and fire size.
        - Opacity: Full opacity for selected points and faded (opacity = 0) for non-selected points.
- Chart 2 - Bar chart: Average Fire Size by Province (Bottom Left)
    - Mark: Bar
    - Channels Exploited:
        - Position (X): Average fire size (Quantitative).
        - Position (Y): Province/agency (Nominal).
        - Tooltip: Display summary information about province/agency and average fire size.
        - Color: Use Altair default blue color for the selected province and gray for non-selected provinces.
- Chart 3 - Lollipop Chart: Average Fire Size by Cause 
	- Mark: Rule + Circle
	- Channels Exploited:
		- Position (X): Average fire size (Quantitative).
        - Position (Y): Fire cause (Nominal).
        - Color: Selected cause is shown in its cause-specific color, and non-selected causes are shown in gray.
        - Tooltip: Display summary information about fire cause and its average fire size.
- Suitability: These choices are well-suited for the intended analytic tasks because:
    - The jittered strip plot allows users to identify outliers (extreme fire events) while still showing province-level distribution patterns.
    - The province bar chart summarizes averages of fire size within each province, which supports regional comparison and shows whether a province tends to experience larger or smaller fires, on average.
    - The lollipop chart for fire causes shows which causes are associated with unusually severe events or broader severity trends.
    - Connection between the three views reveals how individual fires relate to regional and/or cause-based severity patterns.

#### Interaction and Characteristics
- Interval Selection Brush (Shared Filter)
    - Type: Interval Selection.
    - Scope: Applies globally to all charts that include fire-size comparisons.
    - Feedback: The bar chart and lollipop chart will reflect the observation being selected by the brush.
    - Enables:
        - Focuses on specific ranges (e.g., only small fires or only large/extreme events).
        - Helps identify whether unusually severe fires cluster within certain provinces or causes.
- Cause Click Selection (Fire Cause Filters)  
    - Type: Single point selection on fire cause
    - Scope: Affects the jitter strip and lollipop chart.
    - Feedback: When a cause is clicked in the lollipop chart, all fire events with that cause are highlighted in their assigned color on the jitter strip plot. Causes that are not selected appear in gray in the lollipop chart, and all non-selected fire events become faded in the jitter strip plot.
    - Enables:
        - Direct comparison across causes by isolating one cause at a time.
        - Helps detect which categories produce unusually large or severe fires.
- Province Click Selection (Province/Agency Filters)
	- Type: Single province selection
	- Scope: Affects the jitter strip and bar chart
	- Feedback: When a province is clicked in the bar chart, all fire events from that province are highlighted on the jitter strip plot, while events from other provinces are faded. The selected province remains in the default blue color in the bar chart, and all unselected provinces appear in gray to emphasize the selection.
	- Enables:
        - Regional comparisons on severity and fire size.
        - Identification of provinces with extreme behavior compared to national patterns.
- Severity Dropdown (Global Severity Filters)
	- Type:  Dropdown menu for switching between severity groups such as small, medium, large, or extreme fires.
	- Scope:  Filters every visualization based on the selected severity class.
    - Feedback: Changing severity updates all charts instantly to show only events of that class. “Overall” returns all events with no severity restriction.
    - Enables:
        - Comparison between small/medium/large/extreme fires.
        - Supports analytic tasks focused on severity-specific patterns across regions and causes.
     
#### Important Note for Dashboard 2
- AI models supported the debugging and development of interactive features such as brushing and click selections when standard Altair documentation did not behave as expected.

#### Critique of View
- Strengths:
    - Users can see both the overall spread and the density of events across regions and causes.
    - Supports both regional and cause-based comparisons, since province-level and cause-level summaries update in response to interactions.
    - The severity dropdown allows focused analysis, letting users isolate small, medium, large, or extreme fires. This improves readability compared to showing all events at once.
    - The brush interaction enables flexible exploration, where selecting a size interval on the jitter plot instantly updates the province bar chart and causes a lollipop chart.
    - Province and cause selections provide targeted filtering, helping users examine specific categories without losing context.
    - Consistent color encoding across views enhances readability, making it easier to track causes and interpret linked interactions.
    - Opacity is used effectively for filtering, reducing clutter and making selected events more visually salient.
    - Linked interactions across all three views help users connect individual events to broader provincial and cause-based patterns, improving interpretability.
- Weaknesses:
    - Severity classes cannot be viewed side-by-side, so users must switch manually between categories, making direct comparison harder.
    - The overall distribution contains many overlapping points, which can be overwhelming and harder to interpret without filtering.
    - The lollipop chart and province bar chart only support single selection, so users cannot compare multiple provinces or multiple causes simultaneously.
    - Lollipop selection may require precision, since clicking the dot is necessary to update the jitter plot, which can be slightly harder than selecting a bar.
    - Some extreme fire events could be visually crowded near the upper end of the size scale, reducing clarity for those specific points.
    - If users apply multiple filters in sequence (brush + province + severity), interaction states may become harder to interpret without resetting.
    - The view uses a 20,000-record sample from a ~500,000-record dataset, and the sample may not be fully representative, especially some provinces that were filtered out intentionally.
  
#### Individual Summary
I chose fire severity as my project theme, but the process ended up being more challenging than I expected. Our dataset only included a few variables that were directly related to severity, mainly fire size and fire cause. Looking back, the theme could have been narrowed or reframed to be something like frequency patterns or temporal trends, which might have offered more flexibility and more diverse analysis opportunities.

Because the usable attributes in the dataset were limited and many were nominal, it was challenging to create multiple views that felt meaningfully different. To address this, I explored alternative ways to represent similar information while maintaining analytical clarity. The first view evolved from an earlier map concept into a fully interactive spatial visualization, while the second view incorporates a lollipop chart to provide a distinct comparison structure without losing interpretability. By drawing on a range of visualization ideas and adapting them to the dataset, I was able to assemble a more balanced and diverse set of views.

A key takeaway from this milestone is that designing a visualization from scratch is much harder than implementing one from instructions. Turning sketches into functional Altair code involved many unexpected issues, which highlighted how essential strong design and implementation skills are. If I were to restart, I would spend more time refining the design before coding. In practice, I frequently had to redesign during implementation, adjusting layouts and debugging errors in a repeated cycle. This back-and-forth made the process challenging and time-consuming. But it was beneficial to realize this point throughout the process.
