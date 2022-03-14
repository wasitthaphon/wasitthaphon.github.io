---
layout: post
title: "Introduction to git"
date: 2022-03-14 15:06:00 +0700
categories: Git 101
---

## Git คืออะไร ?

Git เป็นโปรโตคอล และซอฟต์แวร์สำหรับการจัดการเวอร์ชันของไฟล์ Source code ทั้งต้ว ไฟล์ Text และ ไฟล์ Binary

## ทำไมต้องเป็น Git ?

Git เข้ามามีบทบาทในการทำเวอร์ชันให้กับไฟล์ในแต่ละการอัปเดตได้ จุดเด่นที่ทำให้ Git มีการใช้งานที่แพร่หลายมาจาก Feature 2 ตัวนี้

1. Branching
2. Distributed versioning control

### Branching

ผู้ใช้สามารถแยก Branch หรือสาขาการทำงานบน Repository เพื่อไม่ให้ตัวไฟล์กระทบกับ Branch หลักได้

### Distibuted versioning control

กรณีทำงานร่วมกันหลายคน ผู้ใช้ไม่จำเป็นต้องเชื่อมต่อเซิฟเวอร์กลางเพื่อทำเวอร์ชันตลอดเวลา ผู้ใช้จะอัปเดตเวอร์ชันภายวน Local repository ของตนเอง และหากมีอัปเดตที่ต้องการให้ทุกคนอัปเดตด้วย จึงค่อยอัปเดตขึ้นไปยังตัวเซิฟเวอร์กลาง

## คำสั่งที่จำเป็น

คำสั่งดังต่อไปนี้อ้างอิงมาจาก [Github git cheat sheet](https://training.github.com/downloads/github-git-cheat-sheet.pdf)

### Configuration ก่อนเริ่มต้นทำงาน

คำสั่งสำหรับการตั้งค่า information สำหรับ user บน Local repository

```git
git config --global user.name "[name]"
```

ตั้งค่าชื่อที่ต้องการบันทึกลงบน Transaction ที่เรา commit

```git
git config --global user.email "[email address]"
```

ตั้งค่า email ที่ต้องการบันทึกลงบน Transaction ที่เรา commit

### Branches

> :memo: commit ใด ๆ ที่ผู้ใช้บันทึกจะเกิดขึ้นบน Branch ปัจจุบันที่ผุ้ใช้กำลังทำงานอยู่

```git
git branch [branch-name]
```

สร้าง branch ใหม่

```git
git checkout [branch-name]
```

สลับไปยัง branch ที่ระบุและอัปเดตไฟล์ทั้งหมดตาม branch นั้น

```git
git merge [branch]
```

รวม branch ที่ระบุมาไว้ฃใน branch ที่ผู้ใช้กำลังทำงานอยู่ ณ ปัจจุบัน

```git
git branch -d [branch-name]
```

ลบ branch

### คำสั่งกลุ่มสำหรับการ commit การเปลี่ยนแปลงใหม่ ๆ

```git
git log
```

ลิสต์ประวัติการ commit ของ branch ที่กำลังทำงานอยู่ ณ ปัจจุบัน

```git
git add [file]
```

ทำ snapshot ของไฟล์ต่าง ๆ ที่ต้องการให้บันทึกการเปลี่ยนแปลง

```git
git commit -m "[descriptive message]"
```

บันทึกกลุ่มไฟล์ที่ได้ทำ snapshot ด้วยคำสั่ง git add เรียบร้อยแล้ว

### Synchronize changes

คำสั่งการ Sycnhronize ระหว่าง Local repository กับ Remote repository

```git
git fetch
```

ดาวน์โหลดประวัติ commit ทั้งหมดของ branch ต่าง ๆ ที่ทำ Tracking (Set upstream หากันแล้ว) เรียบร้อยแล้วมากองไว้

```git
git merge
```

รวม commit จาก remote repository ที่ fetch มาแล้วไปไว้ใน local branch

```git
git push
```

อัปโหลด local branch ขึ้นไปยัง remote branch

```git
git pull
```

อัปเดต local branch ปัจจุบันด้วย commit ต่าง ๆ บน remote branch คำสั่งนี้เกิดจากการทำงานของ 2 คำสั่งต่อไปนี้ร่วมกัน คือ **git fetch** และ **git merge**

### Redo commits

คำสั่งสำหรับการย้อนกลับ หรือยกเลิกการแก้ไขต่าง ๆ

```git
git reset
```

```git
git cherry pick
```

```git
git revert
```

```git
git rebase
```

## การใช้ Git บน VS code

การทำงาน Source control บน vs code สามารถเข้าไปใช้งานง่าย ๆ โดยที่ไม่ต้องใช้ command บน command prompt ได้

![Food & drink](/assets/images/git-resource/git-entrance.PNG)

ก่อนจะเข้ามาใช้งานที่ Extension source control ได้ ที่ Directory นั้น ๆ ต้องมีไฟล์ที่เกี่ยวกับ .git ก่อน (ผ่านการ git init มาแล้ว)

### Features

ฟีเจอร์ที่ Source control ที่บน vs code ให้มาเป็น interface ดูง่าย ๆ มีตามภาพ

![Features](/assets/images/git-resource/git-features.PNG)

- SOURCE CONTROL

  เป็นพื้นที่สำหรับการทำ commit change ต่าง ๆ

- COMMITS

  เป็นพื้นที่สำหรับแสดงประวัติ commit ทั้งหมดของ branch ที่กำลังทำงานอยู่

- FILE HISTORY

  เป็นพื้นที่แสดงประวัติการเปลี่ยนแปลงของไฟล์ที่เลือกอยู่ ณ ขณะนั้นว่าในแต่ละ commit ที่มีไฟล์นี้เกี่ยวข้องนั้น มีการอัปเดตอะไรไปบ้าง

- BRANCHES

  เป็นพื้นที่แสดง branch ทั้งหมดที่อยู่บน Local repository ของเรา และยังแสดงรายการ commit ของแต่ละ branch ได้ด้วย

- REMOTES

  เป็นพื้นที่สำหรับดูว่าเชื่อมต่อกับ Remote branch ใดบ้าง

- STASHES

  เป็นพื้นที่สำหรับเก็บ changes ที่ยังไม่ต้องการ commit

- TAGS

## Workshop

workshop นี้จะเป็นการพาใช้งาน git เบื้องต้นทั้งการทำงานคนเดียว (Local repository) และการทำงานร่วมกับหลาย ๆ คน (Remote repository)

### การใช้งาน Git เบื้องต้นบน Local repository

#### หัวข้อนี้จะได้ทำความเข้า

- การ initialize git
- การใช้เครื่องมือ VS code จัดการกับ Source control
- การ Tracked ไฟล์
- การแก้ไขไฟล์
- การ commit พร้อมทั้งการทำ commit message
- การ Redo commit

#### Let's get started

1. เปิด command promp แล้วไปยัง Directory ที้ต้องการจะสร้าง git source control

   ![Git init location](/assets/images/git-resource/cmd.PNG)

2. ใช้คำสั่ง git init เพื่อสร้าง git project

   ```git
   git init
   ```

3. สร้างไฟล์ Text ขึ้นมา 1 ไฟล์ชื่อ drinks.txt

   ![Drinks](/assets/images/git-resource/create-file-drinks.PNG)

   จะสังเกตเห็นว่าที่ไฟล์ drinks.txt จะมีสี และตัวอักษรพิมพ์ใหญ่ปรากฏบนไฟล์ (ฟีเจอร์ของ git ตัวนี้ขึ้นอยู่กับเครื่องมือที่ support ด้วย) ขออธิบายที่ตัวอักษรนะครับ

   1. U: Untracked คือ ไฟล์ที่ยังไม่เคยถูกทำ source control เลย
   2. A: Index Added คือ ไฟล์ที่ถูกเพิ่มเข้ามาใหม่ใน Staging area (หลังจาก add)
   3. M: Modified คือ ไฟล์ที่ถูก Tracked และมีการแก้ไข
   4. R คือ ไฟล์ที่จะถูกเปลี่ยนชื่อใน commit ถัดไป
   5. D คือ ไฟล์ที่จะถูกลบใน commit ถัดไป

4. ใส่ข้อมูลเครื่องดี่ม (หรืออะไรก็ได้)

   ![Drinks some data](/assets/images/git-resource/some-data.PNG)

   จากภาพมีข้อมูลเครื่องดื่ม 3 รายการ ได้แก่ ไวน์, เบียร์ และนม

5. ทำ Tracked ไฟล์ drinks.txt

   การทำ Track ไฟล์สามารถทำผ่าน interface ของ IDE ที่สนับสนุน หรือ ผ่าน git cli ได้โดยตรง

   ![track file](/assets/images/git-resource/git-add-track.PNG)

   จากภาพจะมีอยู่ 2 วิธีที่ทำ Track ไฟล์ได้

   1. คลิกที่ปุ่ม "+"
   2. ใช้คำสั่ง `git add [filename]`

6. สังเกตผลการทำ track ไฟล์ drinks.txt

   ![added file](/assets/images/git-resource/git-added.PNG)

   จะเห็นว่าสีแดงซ้ายมือ หมายถึงบรรทัดนั้น ๆ ถูกลบออก และสีเขียวที่อยู่ขวามือหมายถึงบรรทัดนั้น ๆ ถูกเพิ่มเข้ามาใหม่

7. commit การทำงาน

   commit เพื่อบันทึกการทำงานโดยการ commit นั้น มีหลักการตั้งชื่ออธิบายการอัปเดตในแต่ละครั้งด้วยเช่นกัน สามารถเข้าไปศึกษาเพิ่มเติมได้ที่ [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)

   การทำ commit สามารถทำได้ทั้งผ่าน git cli หรือ ผ่าน interface ของ IDE ที่สนับสนุน โดยจากภาพจะเป็นการทำ commit message ผ่าน GUI ของ VS Code

   ![Commit message](/assets/images/git-resource/commit-message.PNG)

   หากใส่ข้อความเรียบร้อยแล้วให้คลิ้กที่ `เครื่องหมายถูก`

8. ตรวจประวัติการ commit

   สามารถดูประวัติการ commit ของ branch นั้น ๆ ได้ด้วยการไปยังแท็บ `COMMIT`

   ![Commit history](/assets/images/git-resource/commit-history.PNG)

9. แก้ไขข้อมูลไฟล์ drinks.txt

   ลองแก้ไขข้อมูลรายการเครื่องดื่มบางอย่างในไฟล์ drinks.txt เช่น ลบบางเครื่องดื่ม แล้วเพิ่มเครื่องดื่มชนิดใหม่เข้าไป

   ![Add drinks](/assets/images/git-resource/add-new-drink.PNG)

   จากภาพลบ`เบียร์`ออกไปแล้วเพิ่ม`กาแฟ` และ`ชา`เข้ามา

10. Add ไฟล์ drinks.txt เพื่อเข้า staging area สำหรับเตรียม commit ใหม่

    add ไฟล์ที่ต้องการ commit เข้าไปยัง staging area เมื่อ add สำเร็จจะเห็นไฟล์ไปอยู่ใน Tab `Staged Changes` ดังแสดงตามภาพ

    ![Git differenct](/assets/images/git-resource/git-different.PNG)

    หากลองคลิ้กที่ไฟล์ `drinks.txt` บน `Staged Changes` ตัว vs code จะแสดงสิ่งที่นำออก (ไฮไลท์สีแดงฝั่งซ้าย) และสิ่งที่เพิ่มเข้าไป (ไฮไลท์สีเขียวฝั่งขวา)

    จากภาพจะเห็นว่า

    1. ลบเบียร์บรรทัดที่ 2 ออก แล้วเพิ่ม กาแฟเข้าไปแทน
    2. เพิ่มชาที่บรรทัดที่ 4

11. commit changes

    บันทึกการทำงานและใส่ commit message ที่สื่อความหมายได้ลงไป

    ![Git commit #2](/assets/images/git-resource/commit-2.PNG)

12. สังเกตประวัติ commits

    ให้ไปที่แท็บ `COMMITS` จะพบว่ามีประวัติการ commit เพิ่มขึ้นมาใหม่ 1 commit

    ![Git history #2](/assets/images/git-resource/commit-history-2.PNG)

### การใช้งาน Git เบื้องต้นบน Remote repository
