PyCitySchools_starter

This repo contains the code, the results, and the analysis performed on data pertaining to a hypothetical school district.  The PyCitySchools folder in this repo contains:
	
 	The Jupyter Notebook code:  PyCitySchools_starter.ipynb
 	
  	The results of running the code:  PyCitySchools_starter Results.pdf
  	
   	The analysis of the results:  PyCitySchools_starter Analysis.pdf

The methodology behind the code is decribed below:


District Summary

Explanation:
1.	Import Libraries: The code starts by importing the pandas library for data manipulation and the Path class from the pathlib library for file path management.
2.	Load Data: It loads data from two CSV files, schools_complete.csv and students_complete.csv, into pandas DataFrames named school_data and student_data, respectively. The Path object ensures that the file paths are handled correctly across different operating systems.
3.	Merge Data: The pd.merge() function combines the two DataFrames into a single DataFrame called school_data_complete based on the common column "school_name". A "left" merge is used, meaning all rows from student_data are kept, and matching rows from school_data are added.
4.	District Summary Calculations:
o	school_count: Calculates the number of unique schools using len() and .unique().
o	student_count: Calculates the total number of students using .count().
o	total_budget: Calculates the total budget by summing the "budget" column of the school_data DataFrame.
o	average_math_score and average_reading_score: Calculate the average math and reading scores using .mean().
o	passing_math_percentage and passing_reading_percentage: Calculate the percentage of students passing math and reading, respectively, by filtering the DataFrame based on the passing score (>= 70) and dividing the count of passing students by the total student count.
o	overall_passing_rate: Calculates the percentage of students passing both math and reading by filtering for students who passed both and dividing by the total student count.
5.	Create District Summary DataFrame: A new DataFrame, district_summary, is created to hold the calculated metrics.
6.	Formatting: The "Total Students" and "Total Budget" columns are formatted using .map() and string formatting to add commas and currency symbols, respectively.
7.	Display DataFrame: Finally, the district_summary DataFrame is displayed, showing the key district-level metrics.


School Summary

Explanation:
This code analyzes school data to generate a per-school summary including various metrics.  Here's a breakdown:
1. Data Loading and Preparation:
   - Imports the pandas library and the Path class from pathlib for file path handling.
   - Loads school and student data from CSV files using pandas.read_csv().
   - Converts the 'budget' column in school_data to numeric using pd.to_numeric() to enable calculations.
   - Calculates the per-school budget *before* merging, grouping by 'school_name' and taking the mean of 'budget' using groupby() and .mean().
   - Merges the student and school dataframes using pd.merge() with a left join on 'school_name' to create school_data_complete. This avoids having duplicate budget entries.
2. Per-School Calculations:
   - Calculates the total number of students per school using value_counts() on the 'school_name' column.
   - Calculates the per-student budget by dividing the per-school budget by the total students per school.
   - Determines school types using set_index() and selecting the "type" column.  Prints formatted output for school types.
3. Dataframe Formatting for Total Students:
   - Converts the per_school student counts (a Series) to a DataFrame.
   - Renames the 'index' column (created during the reset_index() call) to 'school_name' for consistency.
   - Prints formatted total students per school counts.
4. Average Scores Calculation:
   - Creates a new DataFrame, numeric_cols, containing only the numeric columns ('school_name', 'math_score', 'reading_score') from school_data_complete. The .copy() method is crucial to prevent a SettingWithCopyWarning.
   - Calculates the average math and reading scores per school using groupby() on 'school_name' and .mean() on respective score columns.  Prints formatted mean scores.
5. Passing Student Counts:
   - Calculates the number of students passing math, reading, and both by filtering school_data_complete based on score thresholds (>= 70).
   - Groups the filtered data by 'school_name' and uses .size() to count passing students in each school. Prints formatted passing student counts.
6. Passing Rates Calculation:
   - Calculates passing rates for math, reading, and overall by dividing the number of passing students by the total students per school and multiplying by 100.
   - Prints the formatted passing rates per school.
7. Per School Summary Creation and Formatting:
   - Creates a per_school_summary DataFrame combining all calculated metrics using a dictionary, using correct and aligned indices.
   - Formats the 'Total School Budget' and 'Per Student Budget' columns as currency using .map() and a formatting string.
8. Display:
   - Displays the final per_school_summary DataFrame.


Highest-Performing Schools (by % Overall Passing)

Explanation:
1.	top_schools = per_school_summary.sort_values("% Overall Passing", ascending=False): This line sorts the per_school_summary DataFrame by the "% Overall Passing" column in descending order (highest passing rate first). The sorted DataFrame is assigned to the top_schools variable.
2.	top_schools = top_schools.head(5): This line selects the first 5 rows (the top 5 schools) from the sorted top_schools DataFrame and reassigns the result back to the top_schools variable so that it now only holds the top 5 schools.
3.	print("Top 5 Schools by Overall Passing Rate:") and print(top_schools): This prints a descriptive header followed by the top_schools DataFrame.

Bottom Performing Schools (By % Overall Passing)

Explanation:
This code snippet sorts a DataFrame called per_school_summary to find the 5 schools with the lowest overall passing rates and then displays these schools.  The code efficiently identifies and displays the bottom 5 performing schools based on the overall passing rate. The use of the default ascending sort in sort_values and head(5) makes the code concise. The newline character in the print statement improves the readability of the output.
1.	bottom_schools = per_school_summary.sort_values("% Overall Passing"):
o	per_school_summary.sort_values(...) sorts the per_school_summary DataFrame.
o	"% Overall Passing" specifies that the sorting should be based on the column named "% Overall Passing".
o	The default behavior of sort_values() is to sort in ascending order (from lowest to highest), so the schools with the lowest overall passing rates will be at the beginning of the sorted DataFrame. The ascending=True argument was implied here as it is the default.
o	The result of the sorting is stored in a new DataFrame called bottom_schools.
2.	bottom_schools = bottom_schools.head(5):
o	bottom_schools.head(5) selects the first 5 rows of the bottom_schools DataFrame (which are the 5 schools with the lowest overall passing rates because of the ascending sort).
o	The result (the top 5 rows) is reassigned to the bottom_schools variable, effectively overwriting the previous, fully sorted DataFrame with one containing only the bottom 5 rows.
3.	print("\nBottom 5 Schools by Overall Passing Rate:"):
o	This line prints a descriptive header.
o	\n is a newline character, which creates a blank line before the header is printed, separating it from any previous output.
4.	bottom_schools:
o	This line, by itself, will display the bottom_schools DataFrame (the 5 schools with the lowest "% Overall Passing" rate) in the output. If you have configured your Jupyter Notebook or IDE to only display the last unassigned output of a cell, then this command will work. If not, you will need to explicitly print this DataFrame using print(bottom_schools).


Math Scores by Grade

Explanation:
1.	Grouping and Calculating Means:
o	The lines ninth_grade_math_scores = ninth_graders.groupby("school_name")["math_score"].mean(), etc. group the respective grade-level DataFrames by "school_name" and then calculate the mean of the "math_score" column for each school.
2.	Combining into a DataFrame:
o	The pd.DataFrame(...) constructor creates a new DataFrame, math_scores_by_grade.
o	The dictionary passed to the constructor uses the grade levels ("9th", "10th", etc.) as keys and the corresponding Series of mean math scores as values. This creates the desired structure with grades as columns and school names as rows.
3.	Removing Index Name: The line math_scores_by_grade.index.name = None simply removes the name of the index (which would otherwise be "school_name") from the DataFrame. This is just a minor data wrangling step for cleaner output, it's not strictly required.
4.	Displaying with label: A descriptive label is printed before the DataFrame for better readability of the output.
	

Reading Score by Grade

Explanation:
1.	Grouping and Calculating Means: The lines to calculate ninth_grade_reading_scores, tenth_grader_reading_scores, etc. are analogous to the math scores section, using groupby() and .mean() on the "reading_score" column.
2.	Combining into DataFrame: The reading_scores_by_grade DataFrame is created using a dictionary, with grade levels as keys and the corresponding reading score Series as values.
3.	Minor Data Wrangling (Provided): The provided lines to reorder columns and remove the index name are already correct. The column reordering is likely unnecessary if you create the DataFrame in the correct column order in the first place, as shown above.
4.	Displaying with Label: A clear descriptive label now precedes the DataFrame output.


Scores by School Spending

Explanation:
1.	Copying per_school_summary: Creates school_spending_df as a copy of per_school_summary. This is essential to avoid modifying the original summary DataFrame.
2.	Converting Per Student Budget back to numeric: The 'Per Student Budget' column in the per_school_summary was formatted as strings (with '$' signs, commas etc.). pd.cut requires numeric data, so these characters have to be removed and the column converted to numeric again to define the bins with pd.cut.
3.	Using pd.cut:
o	pd.cut(school_spending_df["Per Student Budget"], spending_bins, labels=labels) categorizes spending per student into bins defined by spending_bins. The bins represent the ranges specified, and labels provides the corresponding labels for each bin.
4.	Calculating Averages: The groupby and aggregation for mean scores for math, reading, % passing math, % passing reading, and % overall passing are all calculated based on the bins previously created, then assigned to respective variables.
5.	Creating the Summary DataFrame: spending_summary is created using a dictionary to organize the calculated average scores for each spending range, all using the correct and aligned indices.
6.	Descriptive Label for Output: A descriptive label is added for better readability.

Scores by School Size

Explanation:
1.	Using pd.cut Correctly:
o	The crucial correction is using pd.cut() on the "Total Students" column of the school_size_df DataFrame, as this contains the school size information. Make sure this column is numeric before using pd.cut.
o	I've added school_size_df['Total Students'] = pd.to_numeric(school_size_df['Total Students']) because total students may have been converted to strings during the formatting.
2.	String Conversion (Unnecessary): The line school_size_df["School Size"] = school_size_df["School Size"].astype(str) is unnecessary because pd.cut already returns categorical data which can be used directly for grouping (and printing).
3.	Calculating Averages: The code for calculating averages is essentially the same, using the newly created "School Size" column for grouping.
4.	Creating the size_summary DataFrame: This DataFrame is constructed using a dictionary to assemble the average scores for each school size category.
5.	Descriptive Print Label: I've added a print label for clarity.


Scores by School Type

Explanation:
1.	Convert to Numeric: The provided code assumed that the columns being averaged ("Average Math Score", etc.) were numeric. However, in the earlier parts of the analysis, I formatted some of these columns (e.g., adding '$' signs). Before using groupby() and .mean(), I've added lines to convert these columns back to numeric using pd.to_numeric(). This is essential for the calculations to work correctly.
2.	Creating type_summary DataFrame: The type_summary DataFrame is created using a dictionary where keys are the column names, and values are the Series you calculated (like average_math_score_by_type).
3.	Descriptive Print Label: A descriptive label is added for output clarity.
