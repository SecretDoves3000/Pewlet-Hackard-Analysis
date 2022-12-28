#Analysis of Employment Data
##Overview
In this analysis we consider the employment data for a large company which is facing a wave of retirements. The puprose of this analysis is to to give context to the scale of the issues this wave prevents and to try and provide a framework for planning solutions.

##Results
- There are a total of 72,485 retirement eligible empoyees in the company.
- There are over 20,000 retirement eligible employees in both Senior Engineering and Senior Staff positions. ![](https://raw.githubusercontent.com/SecretDoves3000/Pewlet-Hackard-Analysis/main/data/retiring_titles_scsh.png)
- There are only around 1500 employees eligible for the companies mentorship program.
- This is 2% coverage of retiring positions by eligible mentors.

##Summary
As elligible retirees begin to leave their posts, the company, to maintain current employment levels, will need to fill over 70,000 positions. At the same time, even if the company tries to fill these roles internally with promotions, they are looking at a 50:1 ratio of positions to fill vs qualified mentors according to the standards that the company has defined. Looking at the additional mentor_count table we have provided, we can see that position by position, the picture is even bleaker. Senior Engineer positions are in particular crisis, with only 334 senior engineers eligible to train for over 20,000 positions: rougly 1% coverage. 

One possible suggestion is to extend the mentorship qualification to try and reduce the mentor/mentee ratio required to cover this massive period of transition. However, even when we allow for younger mentors, the numbers do not change much. Using the following query, we extended the mentorship qualifications to allow for mentors up to 30 years younger than the current qualifications, but no further qualified employees were found than the 1549 that had already been identified.

~~~sql
SELECT DISTINCT ON (e.emp_no) 
	e.emp_no,
	e.first_name,
	e.last_name,
	e.birth_date,
	de.from_date,
	de.to_date,
	ti.title
INTO extended_mentorship_eligibility
FROM employees AS e
	JOIN dept_emp AS de 
		ON (e.emp_no = de.emp_no)
	JOIN titles as ti
		ON (e.emp_no = ti.emp_no)
WHERE (de.to_date = '9999-01-01')
	AND (e.birth_date BETWEEN '1965-01-01' AND '1995-12-31')
ORDER BY (e.emp_no);
~~~

This suggests that the scale of the crisis is greater than we would have even intially expected, and that management strategies have failed to prioritize the long term sustainability of the companies labor infrastructure. As such, the only viable solution we see at this point is to contract out training services from some other pool of instructors. This will incur significant capital costs, but the only other option is to downsize the workforce to such a degree it may lead to a full stoppage. Management should consider carefully what training services to utilize and/or how to handle the abrupt deflation of its labor pool. 
