# Overview
Welcome to my analysis of the data job market, focusing on data anlayst roles.  THis project was created out of a desire to mavigate and understand the job market more effectively. IT delves into the top-pyaing and in-demand skills to help find optimal job opportunities for data analysts.

The data source form [Luke Baroussse's Python Course](https://github.com/lukebarousse/Python_Data_Analytics_Course) which provides a foundation for my analysis, containing detailed information on job titles, salaries, locations, and essenetial skills.  Through a series of Python syntax, i explore key quetions such as the most demanded skills, salary trends, and hte intersection of demand and salary in data analytic jobs.

# The Questions

Below are the questions I want to answer in my porject:

1. What are the skilsl most in dmeand for the top 3 most popular data roles?
2. How are in-demand skills trending for Data Analysts?
3. How ell do jobs and skills pay for Data Analysts?
4. What are the optimal skills for data analysts to learn (high demand AND high paying jobs)? 


# Tools I used

For my deep dive into the data analyst job market, I harness the power of several key tools:

- **Python:**  The backbone of my analysis, allowing me to analyze the data and find critical insights.  I also used the follwing Python librarires:
    - **Pandas Libary:** THis was used to analyze the data
    - **Matplotlib Library:** This was used to visualized the data
    - **Seaborn Libaray:** Helped me create more advanced visualizations
- **Jupyter Notebooks:** The tool i used to run my Python scripts which helped me to include my notes and analysis
- **Visual Studio Code:** My go-to for executing my python scripts.
- **Git & GitHub:** Essential for version control and sharing my Python code and analysis, ensuring collaboration and project tracking


# Data Preparation and Clean up

This section outlines the step taken to prepare for data analysis, ensuring accuracy and usability.
## Import & Clean Up Data
I start by importing the neccessary libraries and loading the dataset, followed by initial data cleaning tasks to ensure quality


# The Analysis
## 1. what are the most demanded skills for the top 3 most popular data roles in the US?

To find the most demanded skills for the top 3 most popular data roles in the US, I filtered dataset for US jobs only.  From there, I filtered out the top 3 most popular data roles.  From these top 3 roles, I analyzed the top 5 skills that were used in each role.  This query highlights the most popular job titles and their respective top 5 skills needed. 

To view my notebook [skills_count.ipynb](skills_count.ipynb)


### Visualize Data
```python

fig, ax = plt.subplots (len(job_titles), 1)

sns.set_theme(style='dark')

for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short']==job_title].head(5)
    #df_plot.plot(kind='barh', x='job_skills', y= 'skill_percent', ax = ax[i], title = job_title)
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:pink_r')
    ax[i].set_ylabel('')
    ax[i].set_title(job_title)
    ax[i].set_xlabel('')
    ax[i].legend().set_visible(False)
    ax[i].set_xlim(0,75)
    

    for n, v in enumerate (df_plot['skill_percent']):
        ax[i].text(v+.5, n, f'{v:.0f}%', va='center')
    
    if i != 2:
        ax[i].set_xticks([])

fig.suptitle ('Likelihood of Skills Requested in US Job Postings', fontsize=15)
plt.tight_layout()
plt.show()
```

### Results
![Visualization of Tops Skills for Top 3 Data Roles](images\Skills_demand_top3_roles.png)


### Insights
- Python is a versatile skill, highly dmeanded aross all three roles, but most prominently for Data Scientists(72%) and Data Engineers(65%)
- SQL is the most requested skill for Data Analysts and Engineers, whereas python was the most requested skill for Data Scientists
- Data Enginner require more specialized tehcnical skills (AWS, Azure, Spark) compared to Data Analysts and Data Scientist who are expected to be proficient in more general data management and analysis tools (Excel, Tableau, and SAS)

## 2. How are in-demand skills trending for Data Analysts? 
### Visualize Data
```python

from matplotlib.ticker import PercentFormatter
sns.lineplot(data=df_plot, dashes=False, palette='pastel')
sns.set_theme(style = 'white')
sns.despine()
ax = plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter(decimals=0))
    
offsets = [0, 0, 1.5, -1.5, 0]

for i in range(5):
    y = df_plot.iloc[-1, i] + offsets[i]
    plt.text(11.2, y, df_plot.columns[i], fontsize=10)
```

### Results

![Trending TOp Skills fo Data Analysts in the US for 2023](images\trending_skills_DA_2023.png)

*Line graph visualizing the trending top skills for data analysts in the US in 2023*

### Insights
- SQL remains the most consistently demanded skill throughout the yar, although it shows a grdual decrease in demand throughout the year. 

- Excel is the second in demand skill for data analysts, surpassing tableau and python and even show a significant increase by November of 2023.

- Tableau and python are equally in demand, competing for third place with python showing a slightly higher jump compared to tableau towards the end of the year.

- SAS although not as in demand, is still one of the top 5 skills needed in data analyst roles. 


## 3. How well do jobs and skills pay for Data Analyst

### Salary Analysis for Data-Related Jobs
#### visualize Data
```python
sns.boxplot(data=df_US_top6, x='salary_year_avg', y='job_title_short', palette = 'pastel', order =job_order)
sns.set_theme(style='white')
sns.despine()

plt.gca().xaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K'))
plt.show()
```

#### Results

![Salary of Data Related Jobs](images\salary_by_job.png)*Box plot visualizing the salary distributions for the top 6 data job titles.*

#### Insights
- there is a signficant variation in salar range across different job titles.  Senior Data cientis positions tend to have the highest salary potential, with up to $600K, indicating the hight value placed on advanced data skills and experience in the industry

- Senior Data Engineer and Senior Data scientis roles show a considerable amroung of outliers on the higher end of the salary spectrum, suggesting that exceptional skills or circumstances can lead to high pay in these roles.  In contrast, Data Analayst roles demonstrate more consistency in salary, with fewer outliers.

- The median salaries of senior roles generally pay higher.  However, not all the senior roles generally pay better than their junior roles.  Notably, Data Scientists and Data Engineers have higher median salaries compare to Senior Data Analysts, suggesting that more data related skills may be paid more compared to increased responbilities among Data Analyst jobs. 




### Highest Paid Skills and Most Demanded Skills for Data Analysts

#### Visualize Data
```python

# Top 10 Highest Paid Skills for Data Analysts')
sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, hue='median', ax=ax[0], palette='pastel')


# Top 10 Most In-Demand Skills for Data Analysts')
sns.barplot(data=df_DA_top_skills, x='median', y=df_DA_top_skills.index, hue='median', ax=ax[1], palette='pastel')
```
#### Results
![Highest Paid Skills & Most In_Demand Skills Among Data Analysts](images\skills_DA.png) *Two separate bar graphs visualizing the highest paid skills and most in-demand skills for data analysts in the US.


#### Insights
- The top graph shows specialized tehnical skills like `dplyr`, `Bitbucket`, and `Gitlab` are assocated with higher salaries, some reaching up to $200K, suggesting that advanced technical proficiency can increase earning potential.

- The bottom graph highlights that foundational skills like `SQL`, `Excel`, and `Ptyhon` are the most in-demand, even though they may not offer the highest salaries.  This demonstrates the importance of these core skills for employability in data analysis roles.

- There is a clear distinction between the skills that are highest paid and those that are most in-demand.  Data analysts aiming to maximize their career potential should consider developing a divrse skill set that include both high-paying specialized skilsl and widely dmenaded foundational skills. 

## 4. What is the most optimal skill to learn for Data Analysts?
### Visualize Data
```python
sns.scatterplot(
    data=df_plot,
     x='skill_percent',
      y='median_salary',
       hue='technology',
        s=140 )

```

#### Results
![Most Optimal SKills for Data Analysts in the US](images\optimal_skills.png)*A scatter plot visualizing the most optimal skills (high paying & high demand) for data analyts in the US.*

#### Insights
- THe scatter plot shows that most of the `programming` skills (colored blue) tend to cluster at higher salary levels compared to other categories, indicating that programming expertise might offer greater salary benefits within the data analytics field.
- Analyst tools (colored green), indlcuing `Tableau` and `Power BI`, are prelavent in jobs postings and offer competitive salaries, showing that visualization and data analysis software are crucial for curret data roles.  This category not only has good salaries but is also versatile across different types of data tasks.
- The database skills (colored orange), such as `Oracle` and `SQL Server`, are assocated with some of the highest salaries among data analyst tools. This indicates a significant demand for data managment and manipulation expertise in the data analytics industry.

# What I learned
Throughout this project, I deepened my understanding of the data analyst job market and enhanced my technical skills in Python, especially in data manipulation and visualization. Here are a few specific things I learned:

- **Advanced Python Usage:** Utilizing libraries such as Pandas for data manipulation, Seaborn and Matplotlib for data visualization, and other libraries helped me perform complex data analysis tasks more efficiently.
- **Data Cleaning Importance:** I learned that thorough data cleaning and preparation are crucial before any analysis can be conducted, ensuring the accuracy of insights derived from the data.
- **Strategic Skill Analysis**: The project emphasized the importance of aligning one's skills with market demand. Understanding the relationship between skill demand, salary, and job availability allows for more strategic career planning in the tech industry.
# Insights
This project provided several general insights into the data job market for analysts:

- **Skill Demand and Salary Correlation:** There is a clear correlation between the demand for specific skills and the salaries these skills command. Advanced and specialized skills like Python and Oracle often lead to higher salaries.

- **Market Trends:** There are changing trends in skill demand, highlighting the dynamic nature of the data job market. Keeping up with these trends is essential for career growth in data analytics.

- **Economic Value of Skills:** Understanding which skills are both in-demand and well-compensated can guide data analysts in prioritizing learning to maximize their economic returns.

# Challenges
This project was not without its challenges, but it provided good learning opportunities:

- **Data Inconsistencies:** Handling missing or inconsistent data entries requires careful consideration and thorough data-cleaning techniques to ensure the integrity of the analysis.

- **Complex Data Visualization:** Designing effective visual representations of complex datasets was challenging but critical for conveying insights clearly and compellingly.

- **Balancing Breadth and Depth**: Deciding how deeply to dive into each analysis while maintaining a broad overview of the data landscape required constant balancing to ensure comprehensive coverage without getting lost in details.

# Conclusion
This exploration into the data analyst job market has been incredibly informative, highlighting the critical skills and trends that shape this evolving field. The insights I got enhance my understanding and provide actionable guidance for anyone looking to advance their career in data analytics. As the market continues to change, ongoing analysis will be essential to stay ahead in data analytics. This project is a good foundation for future explorations and underscores the importance of continuous learning and adaptation in the data field.