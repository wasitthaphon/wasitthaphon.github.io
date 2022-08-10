---
layout: post
title: "Sub Total & Grand Total"
date: 2022-08-09 08:00:00 +0700
categories: VB.Net
---

# Sub Total & Grand Total

## ตัวอย่าง Sub Total

![Overview](/assets/images/subtotal-grandtotal/sample.png)

## ตัวอย่าง Grand Total

![Overview2](/assets/images/subtotal-grandtotal/sample2.png)

## Dataset

[Data Scienctist Salary](https://drive.google.com/drive/u/0/folders/12YUYB0f3JqKIddU3MKe6zjWU-7rKZEke)

### Content

| Column             | Description                                                                                                                                                                                    |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| work_year          | The year the salary was paid.                                                                                                                                                                  |
| experience_level   | The experience level in the job during the year with the following possible values: EN Entry-level / Junior MI Mid-level / Intermediate SE Senior-level / Expert EX Executive-level / Director |
| employment_type    | The type of employement for the role: PT Part-time FT Full-time CT Contract FL Freelance                                                                                                       |
| job_title          | The role worked in during the year.                                                                                                                                                            |
| salary             | The total gross salary amount paid.                                                                                                                                                            |
| salary_currency    | The currency of the salary paid as an ISO 4217 currency code.                                                                                                                                  |
| salaryinusd        | The salary in USD (FX rate divided by avg. USD rate for the respective year via fxdata.foorilla.com).                                                                                          |
| employee_residence | Employee's primary country of residence in during the work year as an ISO 3166 country code.                                                                                                   |
| remote_ratio       | The overall amount of work done remotely, possible values are as follows: 0 No remote work (less than 20%) 50 Partially remote 100 Fully remote (more than 80%)                                |
| company_location   | The country of the employer's main office or contracting branch as an ISO 3166 country code.                                                                                                   |
| company_size       | The average number of people that worked for the company during the year: S less than 50 employees (small) M 50 to 250 employees (medium) L more than 250 employees (large)                    |



## Practice 1 - Data preparaion.

ตัวอย่างข้อมูล

![Data](/assets/images/subtotal-grandtotal/data.png)

### สร้าง Structure เพื่อเป็นโครงสร้างเก็บข้อมูล

```vb
Public Structure SalaryInformation
    Dim sequence as Integer
    Dim workYear as Integer
    Dim experienceLevel as String
    Dim emplymentType as String
    Dim jobTitle as String
    Dim salary as Integer
    Dim salaryCurrency as String
    Dim salaryInUsd as Integer
    Dim employeeResidence as String
    Dim remoteRatio as Integer
    Dim companyLocation as String
    Dim companySize as String

    Public Sub New(pSequence as Integer, pWorkYear as Integer, _
            pExperienceLevel as String, pEmploymentType as String, _
            pJobTitle as String, pSalary as Integer, _
            pSalaryCurrency as Integer, pSalaryInUsd as Integer,_
            pEmployeeResidence as String, pRemoteRatio as Integer, _
            pCompanyLocation as String, pCompanySize as String)

        sequence = pSequence


    End Sub
End Structure
```

### อ่านข้อมูล CSV






