this is a repository of my code in mssql ,oracle,T-sql,PL-sql : 


1-
#find time difference in oracle

select h.END_DATE,
  h.START_DATE,
  trunc(months_between(h.END_DATE,h.START_DATE) / 12) as years,
  trunc(months_between(h.END_DATE,h.START_DATE) -
    (trunc(months_between(h.END_DATE,h.START_DATE) / 12) * 12)) as months,
  trunc(h.END_DATE)
    - add_months(h.START_DATE, trunc(months_between(h.END_DATE,h.START_DATE))) as days
from HRE_EMPLOYEE_DUTY_HISTORY h


2-
#convert date persian to miladi  in mssql
Create FUNCTION [dbo].[UDF_Persian_To_Julian](@SHAMSI_DATE  varchar(10))
RETURNS bigint
AS
Begin

Declare @PERSIAN_EPOCH  as int
Declare @epbase as bigint
Declare @epyear as bigint
Declare @mdays as bigint
Declare @Jofst  as Numeric(18,2)
Declare @jdn bigint
Declare @iYear as int
Declare @iMonth as int
Declare @iDay as int

Set @PERSIAN_EPOCH=1948321
Set @Jofst=2415020.5
SET @iYear = CONVERT(INT ,SUBSTRING(@SHAMSI_DATE, 1, 4)) 
SET @iMonth = CONVERT(INT ,SUBSTRING(@SHAMSI_DATE, 5, 2)) 
SET @iDay = CONVERT(INT ,SUBSTRING(@SHAMSI_DATE, 7, 2)) 

If @iYear>=0 
	Begin 
        Set @epbase=@iyear-474 
	End
Else
	Begin
		Set @epbase = @iYear - 473 
	End
    set @epyear=474 + (@epbase%2820) 
If @iMonth<=7
	Begin 
        Set @mdays=(Convert(bigint,(@iMonth) - 1) * 31)
	End
Else
	Begin
		Set @mdays=(Convert(bigint,(@iMonth) - 1) * 30+6)
	End
    Set @jdn =Convert(int,@iday) + @mdays+ Cast(((@epyear * 682) - 110) / 2816 as int)  + (@epyear - 1) * 365 + Cast(@epbase / 2820 as int) * 1029983 + (@PERSIAN_EPOCH - 1) 
    RETURN @jdn
End
Go
