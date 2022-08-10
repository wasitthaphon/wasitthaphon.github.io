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



## Practice 

ตัวอย่างข้อมูล

![Data](/assets/images/subtotal-grandtotal/data.png)


ผลลัพธ์ที่ต้องการ

![Result](/assets/images/subtotal-grandtotal/result.png)


### Initail properties

```vb
Const EXPERIENCE_LEVEL_INDEX = 2
Const JOB_TITLE_INDEX = 4
Const SALARY_IN_USD_INDEX = 7

Const GRIDVIEW_JOB_TITLE_INDEX = 1

Property Data As DataTable
    Get
        Return Session("SESSION_SALARY_DATA")
    End Get
    Set(value As DataTable)
        Session("SESSION_SALARY_DATA") = value
    End Set
End Property

Public Property SortedData As DataView
    Get
        Return Session("SESSION_DATA_VIEW")
    End Get
    Set(value As DataView)
        Session("SESSION_DATA_VIEW") = value
    End Set
End Property

Public Property PreviouseJobTitle As String
    Get
        Return ViewState("PREVIOUS_JOB_TITLE")
    End Get
    Set(value As String)
        ViewState("PREVIOUS_JOB_TITLE") = value
    End Set
End Property

Public Property CurrentJobTitle As String
    Get
        Return ViewState("CURRENT_JOB_TITLE")
    End Get
    Set(value As String)
        ViewState("CURRENT_JOB_TITLE") = value
    End Set
End Property

Property GrandTotal As Integer
    Get
        Return ViewState("GRAND_TOTAL")
    End Get
    Set(value As Integer)
        ViewState("GRAND_TOTAL") = value
    End Set
End Property

Dim subTotalCount As Integer = 0

Public Structure SubTotal
    Dim jobTitle As String
    Dim sumSalary As Integer
End Structure

Dim subTotalDict As New Dictionary(Of String, SubTotal)

```

### อ่านข้อมูล CSV

[How to: read from comma-delimited text files in Visual Basic](https://docs.microsoft.com/en-us/dotnet/visual-basic/developing-apps/programming/drives-directories-files/how-to-read-from-comma-delimited-text-files)

``` vb
Public Function ReadCsvFile(ByVal filePath As String) As DataTable

    Dim dt As New DataTable
    Dim isFirst As Boolean = True
    Dim tempSalary As Integer

    dt.Columns.Add("experienceLevel", GetType(String))
    dt.Columns.Add("jobTitle", GetType(String))
    dt.Columns.Add("salaryInUsd", GetType(Integer))

    Using reader As New FileIO.TextFieldParser(filePath)

        reader.TextFieldType = FileIO.FieldType.Delimited
        reader.SetDelimiters(",")

        Dim currentRow As String()

        While Not reader.EndOfData
            Try
                If isFirst Then
                    reader.ReadFields()
                    isFirst = False
                End If

                currentRow = reader.ReadFields()

                If Not Integer.TryParse(currentRow(SALARY_IN_USD_INDEX), tempSalary) Then
                    tempSalary = 0
                End If

                dt.Rows.Add(currentRow(EXPERIENCE_LEVEL_INDEX), currentRow(JOB_TITLE_INDEX), currentRow(SALARY_IN_USD_INDEX))
                GrandTotal = GrandTotal + tempSalary
                AccumulateSubTotal(currentRow(JOB_TITLE_INDEX), tempSalary)

            Catch ex As Exception
                Console.WriteLine(ex.Message & "is not valid, skipped.")
            End Try
        End While
    End Using

    Return dt

End Function
```

### Accumultaion function

``` vb
Private Sub AccumulateSubTotal(key As String, value As Integer)
    Dim tempSubtotal As SubTotal
    If Not subTotalDict.ContainsKey(key) Then
        tempSubtotal = New SubTotal
        tempSubtotal.jobTitle = key
        tempSubtotal.sumSalary = value

        subTotalDict(key) = tempSubtotal
    Else
        tempSubtotal = subTotalDict(key)
        tempSubtotal.sumSalary += value
    End If
End Sub
```

### Sort data

การเรียงลำดับข้อมูลจะใช้คลาส DataView เรียง DataTable

``` vb
Dim dataView As DataView

Data = ReadCsvFile(Server.MapPath("ds_salaries.csv"))

dataView = New DataView(Data)
dataView.Sort = "jobTitle ASC"

SortedData = dataView

gvResult.DataSource = dataView
gvResult.DataBind()

```

### เพิ่มแถว

``` vb
Private Sub AddRow(description As String, value As Integer)

    Dim newRow As New GridViewRow(0, 0, DataControlRowType.DataRow, DataControlRowState.Insert)

    newRow.BackColor = ColorTranslator.FromHtml("#F9F9F9")

    Dim blankTableCell As New TableCell
    Dim descriptionCell As New TableCell
    Dim valueCell As New TableCell

    descriptionCell.Text = description
    descriptionCell.HorizontalAlign = HorizontalAlign.Left

    valueCell.Text = value
    valueCell.HorizontalAlign = HorizontalAlign.Right

    newRow.Cells.Add(blankTableCell)
    newRow.Cells.Add(descriptionCell)
    newRow.Cells.Add(valueCell)

    gvResult.Controls(0).Controls.Add(newRow)

End Sub
```

### เพิ่มแถวแบบกำหนด Index

```vb
Private Sub InsertRow(index As Integer, description As String, value As Integer)

    Dim newRow As New GridViewRow(0, 0, DataControlRowType.DataRow, DataControlRowState.Insert)

    newRow.BackColor = ColorTranslator.FromHtml("#F9F9F9")

    Dim blankTableCell As New TableCell
    Dim descriptionCell As New TableCell
    Dim valueCell As New TableCell

    descriptionCell.Text = description
    descriptionCell.HorizontalAlign = HorizontalAlign.Left

    valueCell.Text = value
    valueCell.HorizontalAlign = HorizontalAlign.Right

    newRow.Cells.Add(blankTableCell)
    newRow.Cells.Add(descriptionCell)
    newRow.Cells.Add(valueCell)

    gvResult.Controls(0).Controls.AddAt(index, newRow)

End Sub
```

