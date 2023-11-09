# MIST4610Project1

## Group & Project Information
Class Section: 
- 9:35am - CRN 20242
Group Members:
- Aden Dragulescu [@adendrag](https://github.com/adendrag)
- Adam Parent-Herzog [@parentherzog](https://github.com/parentherzog)
Database: 
- cs_apd88740

## Problem Description
The primary objective of this project is to design and construct a relational database system for a micro-loan provider. This financial institution specializes in offering small, short-term loans to both individuals and businesses. The goal for this database is to help the micro-loan provider streamline its loan operations by making data-driven decisions. Through use of this database, the company will be able to view the performance of its loans, analyze borrower demographics, ensure regulatory compliance, and more.

## Data Model
Our data model represents a MySQL relational database structured for a micro-loan provider company.  Key entities of the data model include the Borrowers, Loans, Payments, Co-signers, Loan Officers, Branches, Credit Scores, Collateral, Investors, Regulatory Compliance Reports, Loan Categories, Collections, and Applications tables. These interconnected entities capture the essential information related to the company's operations and we can use the relationships between them to gain valuable insights into the business.

For instance, a relationship between the Borrowers and Loans tables allows the micro-loan provider to track the loan history of each borrower effectively. Each borrower can have multiple loans with the micro-loan provider.

Similarly, the relationship between Loans and Payments reflects that borrowers make multiple payments over the course of a loan's repayment period, and each payment is tied to a particular loan. It enables the micro-loan provider to keep a record of all payments made for each loan, allowing for accurate tracking of loan repayments and their status/history.

![project1datamodel](https://user-images.githubusercontent.com/25353210/281260894-e08d28d1-a613-4de3-8744-abac7f3b2a76.png)

## Data Dictionary
![datadictionary1](https://github.com/adendrag/MIST4610Project1/assets/25353210/ef76ca75-33e6-4f71-99dc-df7c3e7f2e57)
![datadictionary2](https://github.com/adendrag/MIST4610Project1/assets/25353210/3c1c130c-ae92-41d2-a77d-0f5edfee159d)
![datadictionary3](https://github.com/adendrag/MIST4610Project1/assets/25353210/9d992ee3-3adb-4b21-8132-67ac357eea43)
![datadictionary4](https://github.com/adendrag/MIST4610Project1/assets/25353210/a4b5dce0-9b14-4ab6-8e1b-2e3b156227da)
![datadictionary5](https://github.com/adendrag/MIST4610Project1/assets/25353210/d8f7e3a7-189c-46e5-a323-6e3060dd7407)
![datadictionary6](https://github.com/adendrag/MIST4610Project1/assets/25353210/2b49d56b-8853-4c4c-8cb4-becdb870f7a4)
![datadictionary7](https://github.com/adendrag/MIST4610Project1/assets/25353210/d3e064b8-aeb6-46f2-9752-b879840f01f3)
![datadictionary8](https://github.com/adendrag/MIST4610Project1/assets/25353210/3cdf7088-ad59-41bf-81ae-aed6191aec2a)
![datadictionary9](https://github.com/adendrag/MIST4610Project1/assets/25353210/080eb07f-6cf6-4ef2-88be-9b9ac83f2a92)
![datadictionary10](https://github.com/adendrag/MIST4610Project1/assets/25353210/ba44dc2b-04d8-4557-baa4-e6342230cf7a)
![datadictionary11](https://github.com/adendrag/MIST4610Project1/assets/25353210/50f65328-4214-4770-8dbf-f537a07027ba)
![datadictionary12](https://github.com/adendrag/MIST4610Project1/assets/25353210/70864663-00ce-4072-a661-55215339363e)

## Queries
1. 

```
SELECT DISTINCT(Name) as Name, sum(Loans.LoanAmount) as TotalLoans
FROM Borrowers JOIN Loans on Loans.BorrowerID = Borrowers.BorrowerID
GROUP BY Name
```
\
This query calculates the total loan amounts for each borrower and presents this information along with the borrower's name. The results are listed in descending order of the total loan amounts. This query allows management to identify borrowers who have taken out the highest total loan amounts, which may indicate a need for special attention or tailored loan offerings.

2. 

```
SELECT CollateralID, EstimatedValue,LoanID 
FROM Collateral
WHERE Collateral.Description REGEXP("Car") and EstimatedValue > 10000;
```
\
This query shows the value of all cars being held as collateral over 10,000. It also shows the loan ID and collateral ID so they can be referenced after looking at the table. This table could be valuable to the business when car markets fluctuate greatly. For example, if the value of the car dropped dramatically, the business could face losses because the car is no longer worth what it was estimated at. By using this table, the business knows which cars being used as collateral need their value re-evaluated in order to prevent large losses.

3. 

```
SELECT BranchName, count(DISTINCT(LoanOfficerID)) as NumOfficers,count(Distinct(ResponsibleEmployeeID)) as NumReports
FROM RegulatoryComplianceReports, LoanOfficers, Branches
WHERE BranchName = LoanOfficers.BranchLocation and ResponsibleEmployeeID = LoanOfficerID
GROUP BY BranchName;
```
\
This table shows how many loan officers work at each branch, and how many reports must be filed at each branch. This would be very important from a managerial aspect to ensure that there are enough loan officers at each branch. If there were 10 reports to be filed with only one loan officer at one branch, while there was only 1 report with 3 loan officers at another, this table would make it apparent that loan officers or reports should be moved around in order to ensure the reports get completed on time. Alternatively, if one branch is completing alot more reports in the same amount of time as others with less loan managers, this would imply inefficiency in the other branches that would need remedying.

4. 

```
SELECT Count(Distinct(Borrowers.Name)) as count,avg(CreditScore) as AvgCredit,sum(LoanAmount) as TotalLoanAmount
FROM Loans, Borrowers, CreditScores
WHERE Loans.BorrowerID = Borrowers.BorrowerID and CreditScores.BorrowerID = Borrowers.BorrowerID
and CreditScores.CreditHistory REGEXP 'Fair';
```
\
This query would be used to analyze the number of risky investments currently undertaken by the business. This table looks at all the Borrowers who only have a “fair” credit history, and averages their credit scores and the total amount of debt they have accumulated. When operating a loans business, it is important not to spend too much on your bad debts expense. For this reason, you don’t want too much money tied up in risky investments on people with subpar credit scores.

5. 

```
SELECT InvestorID,InvestorName, PortfolioOfLoans, InvestmentAmount/(select sum(InvestmentAmount) from Investors) as PercentageInvested
FROM Investors
WHERE Investors.PortfolioOfLoans REGEXP '3';
```
\
This query selects investors id, name, and shows their related information, like the number of loans in their portfolio and the percentage of amount they've invested compared to the total amount invested by all investors. This can help the business identify potential opportunities or areas where adjustments may be needed to optimize the allocation of investor funds.

6. 

```
SELECT Borrowers.BorrowerID, Name, CreditScore, sum(LoanAmount)/sum(EstimatedValue)
FROM Collateral, Loans, Borrowers, CreditScores
WHERE CreditScores.CreditScore/((select sum(CreditScore) from CreditScores) < .5)
and CreditScores.BorrowerID = Borrowers.BorrowerID
and Collateral.LoanID = Loans.LoanID
and Loans.BorrowerID = Borrowers.BorrowerID
GROUP BY Name, Borrowers.BorrowerID
HAVING .05 < (sum(LoanAmount)/(select sum(LoanAmount) from Loans));
```
\
This query calculates the leverage ratio for borrowers whose credit scores are less than 50% of the sum of all credit scores in the system, indicating that they are below the median credit risk. The leverage ratio is determined by dividing the sum of loan amounts by the sum of the estimated values of collaterals provided. This metric helps to identify borrowers who are highly leveraged, meaning they have borrowed a significant amount relative to the value of their collateral. This could signal higher risk to the business if these borrowers default on their loans.

7. 

```
SELECT CategoryID, CategoryName, COUNT(ApplicationID) AS NumApplications
FROM LoanCategories JOIN Applications ON LoanCategories.CategoryID = Applications.CategoryID
GROUP BY CategoryID
ORDER BY NumApplications DESC;
```
\
This query provides insights into which loan categories are the most popular based on the number of applications. By joining the LoanCategories and Applications tables, the business can determine which categories are in high demand and potentially adjust their product offerings or marketing strategies accordingly.

8. 

```
SELECT Borrowers.Name,LoanType,LoanAmount,InterestRate,RepaymentTerm, ApplicationDate, CreditScore
from Loans
JOIN Borrowers on Borrowers.BorrowerID = Loans.BorrowerID
JOIN CreditScores on CreditScores.BorrowerID = Loans.BorrowerID
WHERE Borrowers.EmploymentDetails regexp 'Employed'
and Loans.ApprovalStatus regexp 'Pending'
and CreditScores.CreditHistory regexp 'Good' or CreditScores.CreditHistory regexp 'Excellent')
order by ApplicationDate;
```
\
This query lists pending loan applications for borrowers who are employed and have a 'Good' or 'Excellent' credit history. It provides detailed information including the borrower's name, loan type, amount, interest rate, repayment term, application date, and credit score. By focusing on loans that are pending approval for borrowers with stable employment and strong credit histories, the business can prioritize these applications as they are likely to represent lower risk and may be processed more quickly. The results are sorted by the application date to help the loan officers to address applications in a timely manner.

9. 

```
SELECT ApplicationID, BorrowerID, LoanAmount, ApplicationStatus
FROM Applications
WHERE NOT EXISTS (SELECT * FROM Payments WHERE Applications.LoanID = Payments.LoanID AND PaymentAmount > 0)
and ApplicationStatus = 'Approved';
```
\
This query retrieves all approved applications where no payments have been made yet. It's useful for identifying loans that might require follow-up for collections or for identifying potential issues in the payment process. By checking for the absence of any payment record with a positive amount, the business can proactively manage its loan portfolio and address any delays in repayment.

10. 

```
SELECT LoanID, count(PaymentID) as NumPayments, sum(PaymentAmount) as TotalPaid
FROM Payments
GROUP BY LoanID
HAVING count(PaymentID) > 1
ORDER BY TotalPaid desc;
```
\
The query summarizes the payment activity for each loan, including  the number of payments made and summing the total amount paid. This helps the business to track repayment progress and identify loans that are being repaid actively, which can influence credit risk assessments and customer relationship management.
