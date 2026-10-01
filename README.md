# The Analysis

## 1. What are the most demanded skills for the top 3 most popular data roles?


To find the most demanded skills for the top 3 most popular data roles, I filtered out those positions by which ones were the most popular, and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their job skills, showing which skills I should be paying attention to depending on the role I am targeting.

Viewed my notebook with the detailed steps here:
[2_Skill_Demand.ipynb](3_Project\2_Skills_Demand.ipynb)

### Visualize Data

```python
fig, ax = plt.subplots(len(job_titles), 1)
sns.set_theme(style='ticks')

for i, job_title in enumerate(job_titles):
    df_plot = df_skills_percent[df_skills_percent['job_title_short'] == job_title].head(5)
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')
    ax[i].set_title(job_title)
    ax[i].set_ylabel('')
    ax[i].set_xlabel('')
    ax[i].get_legend().remove()
    ax[i].set_xlim(0, 78)

    for n, v in enumerate(df_plot['skill_percent']):
        ax[i].text(v + 1, n, f'{v:.0f}%', va='center')

    if i != len(job_titles) - 1:
        ax[i].set_xticks([])

fig.suptitle('Likelihood of Skills Requested in US job Postings', fontsize=15)
fig.tight_layout(h_pad=0.5)
plt.show()
```

### Results

![Visualization of Top Skills for Data Positions](3_Project\Images\skill_demand_for_data_roles.png)

### Insight

- Python is the most versatile skill, as it is highly demanded in all three roles, but most prominently for Data Scientists and Data Engineers.
- SQL is the most requested skill for Data Analysts and Data Scientists, with it in over half of the job postings for both roles.
- Data Engineers require more specialized technical skills, such as AWS, Azure, and Spark. Data Analysts and Data Scientists are expected to be proficient in more general data management and analysis tools, such as Excel and Tableau.

## 2. How are in-demand skills trending for Data Scientists?

### Visualize Data

```python
sns.lineplot(data=df_plot, dashes=False, palette='tab10')
sns.set_theme(style='ticks')
sns.despine()

plt.title('Trending Top Skills for Data Scientists in the US')
plt.ylabel('Likelihood in Job Posting')
plt.xlabel('2023')
plt.legend().remove()

from matplotlib.ticker import PercentFormatter
ax = plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter())

for i in range(5):
    label = df_plot.columns[i]
    y_val = df_plot.iloc[-1, i]
    
    if label == 'tableau':
        y_val += 1.5
    elif label == 'sas':
        y_val -= 1.5
        
    plt.text(11.2, y_val, label, va='center')

plt.show()
```
### Results
![Trending Top Skills for Data Scientists in the US](3_Project\Images\skill_trend_DS_2023.png)

### Insights:
- Python consistently reigns supreme as the most demanded skill, holding a strong lead throughout 2023 and hovering comfortably between 70% and 80% in job postings.
- SQL and R maintain stable, both having distinct positions in the middle tier, with SQL steadily fluctuating in the 50s and R remaining in the 40s.
- Tableau and SAS closely compete for the final slot around the 20% to 30% range, notably experiencing an intersection in June before SAS dips significantly in October and rallies back by December.
- This graph proves that these technologies are safe to learn and are not trending towards being obsoluete in the near future.

## 3. How well do jobs and skills pay for Data Scientist?

### Salary Analysis for Data Jobs

### Visualize Data

```python
sns.boxplot(data=df_US_top6, x='salary_year_avg', y='job_title_short', order=job_order)
sns.set_theme(style='ticks')

plt.title('Salary Distributions in the United States')
plt.xlabel('Yearly Salary (USD)')
plt.ylabel('')
plt.xlim(0, 600000) 
ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
```
### Results
![Salary Distributions of Data Jobs in the US](3_Project\Images\skill_trend_DS_2023.png)

### Insights

- Senior positions command the highest pay scales, with Senior Data Scientists and Senior Data Engineers sharing the highest median salaries at roughly $150,000. This is not true for Senior Data Analyst, as their pay is lower than Data Engineer and Data Scientist.
- Data Scientists generally edge out Data Engineers across both experience levels, while Data Analysts consistently hold the lowest overall earning brackets with a base median just under $100,000.
- Every role features heavy right-skewed outliers extending past $300,000, but mid-level Data Scientists show the most extreme variance with individual salaries stretching close to $600,000.

### Highest Paid & Most Demanded Skills for Data Scientists

```python
fig, ax = plt.subplots(2, 1)

sns.set_theme(style='ticks')

sns.barplot(data=df_DS_top_pay, x='median', y=df_DS_top_pay.index, ax=ax[0], hue='median', palette='dark:b_r')
ax[0].legend().remove()

ax[0].set_title('Top 10 Highest Paid Skills for Data Scientists')
ax[0].set_ylabel('')
ax[0].set_xlabel('')
ax[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'S{int(x/1000)}K'))

sns.barplot(data=df_DS_skills, x='median', y=df_DS_skills.index, ax=ax[1], hue='median', palette='light:b')
ax[1].legend().remove()

ax[1].set_title('Top 10 Most In-Demand Skills for Data Scientists')
ax[1].set_ylabel('')
ax[1].set_xlabel('Median Salary')
ax[1].set_xlim(ax[0].get_xlim())
ax[1].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'S{int(x/1000)}K'))

fig.tight_layout()
```
### Results
![The Highest Paid & Most In Demand Skills for Data Scientists in the US](3_Project\Images\skills_for_DS.png)

### Insights
- Niche project management and collaboration software command a massive premium over core technical tools. asana leads the highest-paid chart at a staggering median salary of over $250,000, while platforms like airtable, notion, and slack dominate the upper tier. This indicates that data scientists who possess advanced workflow-coordination capabilities or specialize in integrating these operational tools are highly compensated.
- Specialized engineering and enterprise platforms outpace standard analytical frameworks in compensation. Enterprise systems like watson and redhat, along with game development engines like unreal, drive median salaries past the $190,000 to $210,000 range. These niche specializations yield higher financial rewards than popular machine learning frameworks like hugging face, which sits closer to $180,000.
- High market demand does not automatically translate to the highest pay scales. The most widely requested foundational skills max out with tensorflow at roughly $150,000 and scale down to around $120,000 for legacy tools like sas. This suggests a clear market trade-off where ubiquitous tools offer excellent job security but face standardized salary caps due to a larger pool of qualified candidates.
- Core technical staples display remarkable salary consistency within the high-demand tier. Foundational pillars like spark, sql, aws, and python all tightly cluster together with a median payout hovering between $130,000 and $135,000. This baseline represents the predictable market rate for a well-rounded data professional, which visibly outpaces traditional office software like excel.

## 4. What is the most optimal skill to learn for Data Scientists?

### Visualize Data

```python
sns.scatterplot(
    data=df_plot,
    x='skill_percent',
    y='median_salary',
    hue='technology'
)

sns.despine()
sns.set_theme(style='ticks')
texts = []
for i, txt in enumerate(df_DS_skills_high_demand.index):
    texts.append(plt.text(df_DS_skills_high_demand['skill_percent'].iloc[i], df_DS_skills_high_demand['median_salary'].iloc[i], txt))

adjust_text(texts, arrowprops=dict(arrowstyle='->', color='gray', lw=0.5))

from matplotlib.ticker import PercentFormatter 
ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K'))
ax.xaxis.set_major_formatter(PercentFormatter())

plt.xlabel('Percent of Data Scientist Jobs')
plt.ylabel('Median Yearly Salary')
plt.title('Most Optimal Skills for Data Scientists in the US')

plt.tight_layout()
plt.show()
```
### Results

![Most Optimal skills for Data Scientists in the US](3_Project\Images\pay_per_skill.png)

### Insights

- Programming skills like Python and SQL dominate market demand, appearing in roughly 72% and 51% of job listings respectively. However, this widespread adoption correlates with more moderate median salaries hovering between $132K and $135K.
- Tensorflow represents the highest-paying libraries skill on the chart, commanding a premium median yearly salary near $150K. Conversely, it remains a highly specialized tool that is requested in only about 12% of data scientist positions.
- Traditional analyst tools like SAS and Excel yield the lowest financial returns, with median salaries sitting at the bottom of the chart between $120K and $124K. They also lag behind in market relevance, appearing in fewer than 25% of active job postings.