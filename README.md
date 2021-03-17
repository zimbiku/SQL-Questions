# SQL-Questions

Duplicate Email Address

SELECT email, COUNT(*)
FROM Employee
GROUP BY email
HAVING COUNT(*) > 1



Department Highest Salaries

SELECT subSelect.name Department, emp.name Employee, subSelect.salary Salary 
FROM ( SELECT dept.name, dept.departmentId, max(emp1.salary) salary 
       FROM Department dept 
       JOIN Employee emp1 ON emp1.departmentId = dept.departmentId 
       GROUP BY dept.name, dept.departmentId ) subSelect 
JOIN Employee emp ON emp.departmentId = subSelect.departmentId 
WHERE emp.salary = subSelect.salary;




Millcreek

-- Stored Procedure
USE [Millcreek Canyon]
GO
/****** Object:  StoredProcedure [dbo].[AddReservation]    Script Date: 3/17/2021 11:35:29 AM ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
-- =============================================
-- Author:		<Author,,Name>
-- Create date: <Create Date,,>
-- Description:	<Description,,>
-- =============================================
ALTER PROCEDURE [dbo].[AddReservation]
	-- Add the parameters for the stored procedure here
	@name	nchar(10)
	,@date	datetime
	,@siteId	int
	,@occupants	smallint
AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
	SET NOCOUNT ON;

    -- Insert statements for procedure here
	insert into reservations (name, date, siteId, occupants)
	values(@name, @date, @siteId, @occupants)

END



-- The view code
SELECT        TOP (100) PERCENT dbo.reservations.name, dbo.reservations.date, dbo.reservations.occupants, dbo.sites.name AS Expr1
FROM            dbo.reservations INNER JOIN
                         dbo.sites ON dbo.reservations.siteId = dbo.sites.siteId


-- I did have to search online for help creating this as I have never made a sql function before
CREATE FUNCTION PopularDay ()  
RETURNS TABLE  
AS  
RETURN   
(  
    SELECT TOP 1 r.date
		, COUNT(r.date) as occurance
    FROM reservations AS r   
    group by r.date
	order by occurance desc
);  
