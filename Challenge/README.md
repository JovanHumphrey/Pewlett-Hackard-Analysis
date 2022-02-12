#Pewlett-Hackard-Analysis#

#Employee Database with SQL#

#Overview#

The purpose of the analysis is to create two deliverables for my client Pewlett Hachard as they prepare to adjust their business to an onslaught of retiring employees. They wanted to know what roles may soon be empty and wanted to start a mentorship program to help facilitate that transition.

##Retirement Titles##

I created a table to hold the titles of retiring employees (born between 1952 and 1955). Because some employees may have had multiple job titles during their tenure I had to use a DISTINCT ON statement to contain only the employees' most recent titles. Then I used the count function to create that table. Finally because I only wanted to include current employees I had to exclude employees that had already left the company. (See retiring_titles.csv)

##Mentorship Program##

My client wants to initiate a mentorship program with employees who are likely to retire in the next ten years. I created a table to hold the titles of those employees (born in 1965). I then combined that data with their job titles and the dates that they held those positions. (See mentorship_eligibilty.csv)

#Results#

	• As we can see form retiring_titles.csv the majority of soon-to-be retired employees occupy senior roles and leadership positions. Pewlett Hachard would be well served to analyze data regarding the lower tiers of the same positions so they can determine which junior employees may be a natural fit for the soon-to-be vacant positions.
	
	• We can also wee that the company only has 2 employees of Management positions that are about to retire. From the table created prior to retiring_titles.csv (unique_titles.csv) we can see these two employees are Hauke Zhang in the Sales Department and Hilary Kambil in the Customer Service department. As it was pointed out earlier in the model there are only 5 managers for the 9 departments, leading me to believe that there may be some cross-department managers. Are Zhang and Kambil part of that cross-over? If so I recommend that my client consider the impact their retirement would have on those other departments.

	• As a result of mentorship_eligibilty.csv we can see that my client will need to reach out to over 3000 potential mentors for its mentorship program. This table can serve as the basis for a mailing list.
	
	• That said they may have more mentors than they need as many employees share the same title. Additionally, some positions such as Staff may not be able to offer as valuable mentorship as other  positions. More data should be collected to design the mentorship program before reaching out to potential mentors.
	
#Summary#

Since so many employees share titles I believe that retiring_titles.csv would be more useful if it also contained department information. I created an additional table for that. (See retiring_titles_dept.csv)

I believe the same is true for mentorship_eligibilty.csv, and created a new table for it. I learned from retiring_titles_dept.csv and determined that the department titles would be more useful than just the department numbers. (See mentorship_eligibility_dept)