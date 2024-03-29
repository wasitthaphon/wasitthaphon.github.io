---
layout: post
title: "VB.Net Gridview"
date: 2022-08-09 08:00:00 +0700
categories: VB.Net
---

# Gridview

* TOC
{:toc}

## Gridview ใช้ทำอะไร

ใช้แสดงผลข้อมูลอ้างอิงจาก table โดยที่คอลัมน์คือฟิลด์ และแถวคือระเบียน
ตัว Gridview เองสามารถมี Interactive ได้ด้วย เช่น เลือกแถว เรียงลำดับ แก้ไข เป็นต้น

Ref: [Gridview](https://docs.microsoft.com/en-us/dotnet/api/system.web.ui.webcontrols.gridview?view=netframework-4.8)

## Gridview controls

- Binding to data source
- ทำ Sort 
- Update และ Delete 
- ทำ Paging 
- เลือกแถว
- เข้าถึง Object model ของ Gridview เพื่อตั้งค่า property ต่าง ๆ หรือ event ต่าง ๆ ได้
- ฟิลด์ที่เป็นคีย เป็นได้หลายตัว
- ฟิลด์ข้อมูลสำหรับคอลัมน์ลิงก์

## Column Fields

ปกติ Column ใน Gridview จะถูกสร้างด้วยอ็อบเจ็กต์ AutoGeneratedField (สืบทอดจาก BoundField) เพราะว่าโดย Default ค่าใน Property AutoGenerateColumns นั้นถูกตั้งค่าให้เป็น `true` แต่หาก Property ดังกล่าวนั้นถูกตั้งค่าให้เป็น `false` เราสามารถสร้าง Column ประเภทต่าง ๆ เองเพิ่มเติมได้

| Type           | Description                                               |
| -------------- | --------------------------------------------------------- |
| BoundField     | แสดงค่าตาม Data source ฟิลด์ประเภทนี้เป็น Default ให้กับ Gridview |
| ButtonField    | แสดงปุ่มกดบนฟิลด์ใน เช่น ปุ่มเพิ่ม ลบ                              |
| CheckBoxField  | แสดง Checkbox ให้แต่ละฟิลด์                                   |
| CommandField   | ตั้งค่าให้แสดงปุ่มการเลือก แก้ไข หรือลบได้                          |
| HyperLinkField | แสดงค่าตาม Data source ให้เป็นแบบลิงก์                         |
| ImageField     | แสดงภาพในฟิลด์                                              |
| TemplateField  | ฟิลด์ที่สามารถ Custom ได้                                      |

## จัดการกับ UI

เราสามารถตั้งค่า Property เพื่อกำหนด Style ให้กับ Gridview ได้

| Style property              | Description                                                |
| --------------------------- | ---------------------------------------------------------- |
| AlternatingRowStyle         | ใช้แสดง Style สลับกันระหว่าง RowStyle กับตัว AlternatingRowStyle |
| EditRowStyle                | แสดง Style ขณะกำลังแก้ไข                                      |
| EmptyDataRowStyle           | แสดง Style บนแถว EmptyDataTemplate                         |
| FooterStyle                 | แสดง Style บนแถว Footer                                    |
| HeaderStyle                 | แสดง Style บนแถว Header                                    |
| PagerStyle                  | แสดง Style บนแถวแสดงตัวดัชนี Page                             |
| RowStyle                    | แดสง Style บนแถว                                           |
| SelectedRowStyle            | แสดง Style ขณะที่แถวนั้นถูกเลือก                                 |
| SortedAscendingCellStyle    | แสดง Style บนคอลัมน์ที่ถูกเรียงลำดับจากน้อยไปมาก                    |
| SortedAscendingHeaderStyle  | แสดง Style บนหัวคอลัมน์ ที่ถูกเรียงลำดับจากน้อยไปมาก                 |
| SortedDescendingCellStyle   | แสดง Style บนคอลัมน์ที่ถูกเรียงลำดับจากมากไปน้อย                    |
| SortedDescendingHeaderStyle | แดสง Style บนหัวคอลัมน์ที่ถูกเรียงลำดับจากมากไปน้อย                  |


## Events

เราสามารถ custom route ใน event ต่าง ๆ ได้

| Event                 | Description                                                                                                                                                     |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| PageIndexChanged      | Event จะเกิดขึ้นเมื่อปุ่ม Pager ถูกคลิ้กแต่เกิดขึ้นต่อจาก Event PageIndexchanging                                                                                              |
| PageIndexChanging     | Event จะเกิดขึ้นเมื่อปุ่ม Pager ถูกคลิ้กแต่เกิดขึ้นก่อน Event PageIndexChanged                                                                                                 |
| RowCancelingEdit      | Event จะเกิดเมื่อปุ่ม Cancel บนแถวถูกคลิ้กก่อนที่ Gridview control จะออกจากโหมดแก้ไขนั้น                                                                                      |
| RowCommand            | Event จะเกิดขึ้นเมื่อปุ่มใด ๆ บน Gridview ถูกคลิ้ก                                                                                                                        |
| RowCreated            | Event จะเกิดขึ้นเมื่อมีแถวใหม่ถุกสร้างขึ้น Event นี้มักจะใช้กับการแก้ไข content ของแถว                                                                                           |
| RowDataBound          | Event จะเกิดเมื่อ data row ถูกผูกกับ data ใน Gridview control แล้ว Event มักจะใช้สำหรับแก้ไข contents ของแถว                                                                |
| RowDeleted            | Event จะเกิดขึ้นเมื่อปุ่ม Delete ของ row นั้น ๆ ถุกคลิ้ก แต่จะเกิดขึ้นหลัจาก Gridview control ลบ data source นั้นแล้ว มักจะใช้ Event นี้เพื่อเช็คผลการลบ                                   |
| RowDeleting           | Event จะเกิดขึ้นเมื่อปุ่ม Delete ของ Row นั้น ๆ ถูกคลิ้กและก่อนที่ข้อมูลนั้นจะถูกลบออกจาก Data source Event นี้มักจะใช้เพื่อยกเลิกการลบ                                                    |
| RowEditing            | Event นี้ปุ่มแก่ไขถูกคลิ้กแต่ก่อนที่ Gridview control จะเริ่มเข้าแก้ไข Event นี้มักจะใช้เพื่อการยกเลิกการแก้ไข                                                                          |
| RowUpdated            | Event นี้เกิดขึ้นเมื่อปุ่มอัปเดตถูกคลิ้ก แต่เกิดขึ้นหลังจาก Gridview control อัปเดตแถวเรียบร้อยแล้ว Event นี้มักจะใช้เพื่อเช็คผลการอัปเดต                                                     |
| RowUpdating           | Event นี้เกิดขึ้นเมื่อปุ่มอัปเดตถูกคลิ้ก แต่เกิดขึ้นก่อน Gridview control จะอัปเดตแถว Event นี้มักจะใช้เพื่อยกเลิกการอัปเดต                                                                |
| SelectedIndexChanged  | Event นี้จะเกิดขึ้นเมื่อปุ่ม Select ถูกคลิ้กแต่เกิดขึ้นหลังจาก Gridview control จัดการการเลือก Event นี้มักจะใช้ทำงานบางอย่างหลังจากแถวนั้นถูกเลือกใน Gridview control แล้ว                    |
| SelectedIndexChanging | Event นี้จะเกิดขึ้นเมื่อปุ่ม Select ถูกคลิ้กแต่เกิดขึ้นก่อน Gridview control จัดการการเลือก Event นี้มักใช้เพื่อยกเลิกการเลือก                                                             |
| Sorted                | Event นี้เกิดขึ้นเมื่อ Hyperlink ของ Sort column ถูกคลิ้ก แต่เกิดขึ้นหลังจาก Gridview control จัดการกับการเรียงลำดับแล้ว Event นี้มักใช้เพื่อจัดการงานบางอย่างหลังจากคลิ้ก hyperlink เพื่อเรียงลำดับ |
| Sorting               | Event นี้เกิดขึ้นเมื่อ Hyperlink ของ Sort colomn ถูกคลิ้ก แต่เกิดขึ้นก่อนที่ Gridview control จัดการกับการเรียงลำดับ Event นี้มักใช้เพื่อยกเลิกการเรียงลำดับ หรือ custom routine การเรียง         |
