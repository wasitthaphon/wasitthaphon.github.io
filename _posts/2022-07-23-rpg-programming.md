---
layout: post
title: "RPG Programming"
date: 2022-07-24 20:18:00 +0700
categories: RPG AS/400
---

# RPG Programming

* TOC
{:toc}

## Purpose of the language

ภาษา RPG ออกแบบมาเพื่อการพัฒนาโปรแกรมใช้ในเชิงธุรกิจ และตัว AS/400 อยู่บนระบบปฏิบัติการ OS/400

## คำสั่ง CL ที่จะได้เรียนจากบทนี้

| Command           | Task                | Description                         |
| ----------------- | ------------------- | ----------------------------------- |
| WRKMBRPDM         | Work with Members   | เข้าไปยังลิสต์ของ Member ใน Source file |
| STRSEU            | Source code editing | Start Source Entry Utility          |
| STRSDA            | Screen display      | Screen Design Aid                   |
| CALL program-name | เรียกโปรแกรม         | ใช้เรียกโปรแกรมอ็อบเจ็กต์                |
| CRTDSPF           | Display file        | สร้างคอมไพล์ Display file             |
| CRTRPGPGM         | การคอมไพล์โปรแกรม    | สร้างคอมไพล์โปรแกรม RPG               |


## ตัวอักษร

- The letters `A-Z`
- The numbers `0-9`
- The characters `+ - * , . ' & / $ # : @`
- The blank

## ลำดับของฟอร์ม

![ภาพแสดงลำดับการประกาศหัว](/assets/images/rpg/order_of_specification.png)

| Specification                            | Describe                                           |
| ---------------------------------------- | -------------------------------------------------- |
| H : Control                              | ใช้สำหรับกำหนด Config ต่าง ๆ ให้กับโปรแกรม                |
| F : File Description                     | ใช้กำหนดเกี่ยวกับไฟล์                                    |
| E : Extension                            | ใช้ประกาศอาร์เรย์ และตาราง                            |
| L : Line Counter                         | ใช้กำหนดความยาวของ บรรทัด Overflow                    |
| I : Input                                | ใช้กำหนด Data structure, constant, records และ field |
| C : Calculation                          | ใช้ทำดำเนินการคำสั่งต่าง ๆ                                 |
| O : Output                               | ใช้สำหรับแสดงผลเป็นแบบ spool file                      |
| **: File Translation records             |                                                    |
| **: Alternate collation sequence records |                                                    |
| **: Compile-time array and table data    |                                                    |

## Indicators

Indicators เก็บค่าตัวอักษร '0' และ '1' โดยใช้เป็นตัวกำกับว่า เปิด หรือ ปิด

- **MR, 1P, KA-KN** และ **KP-KY** จะไม่สามารถใช้คำสั่ง `SETON` บังคับได้
- **MR** และ **1P** จะไม่สามารใช้คำสั่ง `SETOFF` บังคับได้

## Structured programming

XXXX

### Sequential programming

ชุดคำสั่งที่ทำงานต่อกัน

### Conditional branching

- If Else

    ตัวอย่าง สร้างเงื่อนไขเช็คตัวแปร `CENTR` ว่าใช่ `'Y'` หรือ `'N'` หรือไม่ 
    ถ้าใช่ให้เซ็ต `'0'` ให้กับตัวแปร `IN52` 
    ถ้าไม่ใช่ให้เซ็ต `'1'` ให้กับตัวแปร `IN52`

    Ex. javascript
    ``` js
        const CENTR = ''
        let IN52 = '0'

        if (CENTR == 'Y' or CENTR == 'N'){
            IN52 = '0'
        }else{
            IN52 = '1'
        }
    ```

    ![ตัวอย่างภาษา RPG If-else statement](/assets/images/rpg/if_else_source.png)

    
- Select When Other
    
    ![ตัวอย่าง Select when statement](/assets/images/rpg/select_when_source.png)

  - CASxx
  - GOTO
  - CABxx

    xx สามารถเป็นได้ดังนี้

    | อักษร | ความหมาย |
    | ---- | -------- |
    | GT   | X1 > X2  |
    | LT   | X1 < X2  |
    | EQ   | X1 == X2 |
    | NE   | X1 != X2 |
    | GE   | X1 >= X2 |
    | LE   | X1 <= X2 |


- Repeatting an operation based on a certain condition

  - Do

      ![ตัวอย่าง Do statement](/assets/images/rpg/do_source.png)

  - Do While

      ![ตัวอย่าง Do whilte statement](/assets/images/rpg/do_while_source.png)

  - Do Until

      ![ตัวอย่าง Do until statement](/assets/images/rpg/do_until_source.png)

## Operation Codes

### Arithmetic Operations
- [ADD (Add)](#add)
- [DIV (Divide)](#div)
- [MULT (Multiply)](#mult)
- [MVR (Move Remainder)](#mvr)
- [SQRT (Square Root)](#sqrt)
- [SUB (Subtract)](#sub)
- [XFOOT (Summing the Elements of an Array)](#xfoot)
- [Z-ADD (Zero and Add)](#z-add)
- [Z-SUB (Zero and Subtract)](#z-sub)


#### ADD
![Opcode add](/assets/images/rpg/OP_CODE_ADD.png)

#### DIV
![Opcode div](/assets/images/rpg/OP_CODE_DIV.png)

#### MULT
![Opcode mult](/assets/images/rpg/OP_CODE_MULT.png)

#### MVR
![Opcode mvr](/assets/images/rpg/OP_CODE_MVR.png)

#### SQRT
![Opcode sqrt](/assets/images/rpg/OP_CODE_SQRT.png)

#### SUB
![Opcode sub](/assets/images/rpg/OP_CODE_SUB.png)

#### XFOOT
![Opcode xfoot](/assets/images/rpg/OP_CODE_XFOOT.png)

#### Z-ADD
![Opcode z-add](/assets/images/rpg/OP_CODE_Z_ADD.png)

#### Z-SUB
![Opcode z-sub](/assets/images/rpg/OP_CODE_Z_SUB.png)


### Array Operations

- [LOOKUP (Look Up a Table or Array Element)](#lookup)
- [MOVEA (Move Array)](#movea)
- [SORTA (Sort an Array)](#sorta)
- [XFOOT (Summing the Elements of an Array)](#xfoot)

#### LOOKUP

![Opcode lookup](/assets/images/rpg/OP_CODE_LOOKUP.png)

คำสั่ง LOOKUP ใช้ค้นหาค่าของ Factor 1 ใน Factor 2 โดยที่ Factor 2 เป็น Array หรือ Table

Indicators จะใช้กำหนดไว้เพื่อบอกผลการค้นหาและ เปิด Indicator ได้ไม่เกิน 2 คัว

Indicator High (HI): On ถ้าตัวค้นหา (Factor 1) มีใน Array และมีตัวที่มากกว่าอยู่ถัดไป

Ex. Data [A B C C C D E] หา B ดังนั้น C ตัวแรกคือตัวที่ทำให้ HI On

Data [E D C C C B A] หา B ดังนั้น C 

Indicator Low (LO): On ถ้าตัวค้นหา (Factor 1) มีใน Array และมีตัวที่น้อยกว่าอยู่ถัดไป

Equal (EQ): เจอข้อมูล


#### MOVEA

![Opcode Movea](/assets/images/rpg/OP_CODE_MOVEA.png)

#### SORTA

![Opcode Sorta](/assets/images/rpg/OP_CODE_SORTA.png)

#### XFOOT

![Opcode Xfoot](/assets/images/rpg/OP_CODE_XFOOT.png)

### Branching Operations

- [CABxx (Compare and Branch)](#cabxx)
- [GOTO (Go To)](#goto)
- [ITER (Iterate)](#iter)
- [LEAVE (Leave a Do/For Group)](#leave)
- [TAG (Tag)](#tag)

#### CABxx

![Opcode Cabxx](/assets/images/rpg/OP_CODE_CABXX.png)

#### GOTO

![Opcode goto](/assets/images/rpg/OP_CODE_GOTO.png)

![Opcode goto2](/assets/images/rpg/OP_CODE_GOTO_2.png)

#### ITER

![Opcode iter](/assets/images/rpg/OP_CODE_ITER.png)

#### LEAVE

![Opcode leave](/assets/images/rpg/OP_CODE_LEAVE.png)

#### TAG

![Opcode tag](/assets/images/rpg/OP_CODE_TAG.png)

### Call Operations

- [CALL (Call a Program)](#call)
- [PARM (Identify Parameters)](#parm)
- [PLIST (Identify a Parameter List)](#plist)
- [RETURN (Return to Caller)](#retrn)

#### CALL

![Opcode call](/assets/images/rpg/OP_CODE_CALL.png)

#### PARM

![Opcode parm](/assets/images/rpg/OP_CODE_PARM.png)

#### PLIST

![Opcode plist](/assets/images/rpg/OP_CODE_PLIST.png)

#### RETURN

![Opcode return](/assets/images/rpg/OP_CODE_RETURN.png)

### Compare Operations

- [ANDxx (And)](#andxx-and)
- [COMP (Compare)](#comp-compare)
- [CABxx (Compare and Branch)](#cabxx-compare-and-branch)
- [CASxx (Conditionally Invoke Subroutine)](#casxx-conditionally-invoke-subroutine)
- [DOUxx (Do Until)](#douxx-do-until)
- [DOWxx (Do While)](#dowxx-do-while)
- [IFxx (If)](#ifxx-if)
- [ORxx (Or)](#orxx-or)
- [WHENxx (When true then select)](#whenxx-when-true-then-select)

#### ANDxx (And)

![Opcode andxx](/assets/images/rpg/OP_CODE_AND.png)

#### COMP (Compare)

![Opcode comp](/assets/images/rpg/OP_CODE_COMP.png)

#### CABxx (Compare and Branch)

![Opcode cabxx](/assets/images/rpg/OP_CODE_CABXX.png)

#### CASxx (Conditionally Invoke Subroutine)

![Opcode casxx](/assets/images/rpg/OP_CODE_CASXX.png)

#### DOUxx (Do Until)

![Opcode douxx](/assets/images/rpg/OP_CODE_DOUXX.png)

#### DOWxx (Do While)

![Opcode dowxx](/assets/images/rpg/OP_CODE_DOWXX.png)

#### IFxx (If)

![Opcode ifxx]/assets/images/rpg/./assets/images/rpg/OP_CODE_IFXX.png)

#### ORxx (Or)

![Opcode orxx](/assets/images/rpg/OP_CODE_ORXX.png)

#### WHENxx (When true then select)

![Opcode whenxx](/assets/images/rpg/OP_CODE_WHENXX.png)


โดยที่ xx หมายถึง

xx สามารถเป็นได้ดังนี้

| อักษร   | ความหมาย                         |
| ------ | -------------------------------- |
| GT     | X1 > X2                          |
| LT     | X1 < X2                          |
| EQ     | X1 == X2                         |
| NE     | X1 != X2                         |
| GE     | X1 >= X2                         |
| LE     | X1 <= X2                         |
| Blanks | การทำงานแบบไม่มีเงื่อนไข เช่น CAS, CAB |

### Data-Area Operations

- [IN (Retrieve a Data Area)](#in-retrieve-a-data-area)
- [OUT (Write a Data Area)](#out-write-a-data-area)
- [UNLOCK (Unlock a Data Area or Release a Record)](#unlock-unlock-a-data-area-or-release-a-record)

#### IN (Retrieve a Data Area)

![Opcode In](/assets/images/rpg/OP_CODE_IN.png)

#### OUT (Write a Data Area)

![Opcode Out](/assets/images/rpg/OP_CODE_OUT.png)

#### UNLOCK (Unlock a Data Area or Release a Record)

![Opcode Unlock](/assets/images/rpg/OP_CODE_UNLOCK_1.png)

![Opcode Unlock2](/assets/images/rpg/OP_CODE_UNLOCK_2.png)

### Declarative Operations

- [DEFINE (Field Definition)](#define-field-definition)
- [KFLD (Define Parts of a Key)](#kfld-define-parts-of-a-key)
- [KLIST (Define a Composite Key)](#klist-define-a-composite-key)
- [PARM (Identify Parameters)](#parm-identify-parameters)
- [PLIST (Identify a Parameter List)](#plist-identify-a-parameter-list)
- [TAG (tag)](#tag-tag)

#### DEFINE (Field Definition)

![Opcode define](/assets/images/rpg/OP_CODE_DEFINE.png)

#### KFLD (Define Parts of a Key)

![Opcode kfld](/assets/images/rpg/OP_CODE_KFLD.png)

#### KLIST (Define a Composite Key)

![Opcode klist](/assets/images/rpg/OP_CODE_KLIST.png)

#### PARM (Identify Parameters)

![Opcode parm](/assets/images/rpg/OP_CODE_PARM.png)

#### PLIST (Identify a Parameter List)

![Opcode plist](/assets/images/rpg/OP_CODE_PLIST.png)

#### TAG (tag)

![Opcode tag](/assets/images/rpg/OP_CODE_TAG.png)

### File Operations 

- [CHAIN (Random Retrieval from a File)](#chain-random-retrieval-from-a-file)
- [CLOSE (Close Files)](#close-close-files)
- [DELETE (Delete Record)](#delete-delete-record)
- [EXCEPT (Calculation Time Output)](#except-calculation-time-output)
- [EXFMT (Write/Then Read Format)](#exfmt-writethen-read-format)
- [OPEN (Open File for Processing)](#open-open-file-for-processing)
- [READ (Read a Record)](#read-read-a-record)
- [READC (Read Next Changed Record)](#readc-read-next-changed-record)
- [READE (Read Equal Key)](#reade-read-equal-key)
- [READP (Read Prior Record)](#readp-read-prior-record)
- [READPE (Read Prior Equal)](#readpe-read-prior-equal)
- [SETGT (Set Greater Than)](#setgt-set-greater-than)
- [SETLL (Set Lower Limit)](#setll-set-lower-limit)
- [UNLOCK (Unlock a Data Area or Release a Record)](#unlock-unlock-a-data-area-or-release-a-record)
- [UPDATE (Modify Existing Record)](#update-modify-existing-record)
- [WRITE (Create New Records)](#write-create-new-records)

#### CHAIN (Random Retrieval from a File)

![Opcode chain](/assets/images/rpg/OP_CODE_CHAIN.png)

#### CLOSE (Close Files)

![Opcode close](/assets/images/rpg/OP_CODE_CLOSE.png)

#### DELETE (Delete Record)

![OPcode delete](/assets/images/rpg/OP_CODE_DELETE.png)

#### EXCEPT (Calculation Time Output)

![Opcode except](/assets/images/rpg/OP_CODE_EXCEPT.png)

#### EXFMT (Write/Then Read Format)

![Opcode exfmt](/assets/images/rpg/OP_CODE_EXFMT.png)

#### OPEN (Open File for Processing)

![Opcode open](/assets/images/rpg/OP_CODE_OPEN.png)

#### READ (Read a Record)

![Opcode read](/assets/images/rpg/OP_CODE_READ.png)

#### READC (Read Next Changed Record)

![Opcode readc](/assets/images/rpg/OP_CODE_READC.png)

#### READE (Read Equal Key)

![Opcode reade](/assets/images/rpg/OP_CODE_READE.png)

#### READP (Read Prior Record)

![Opcode readp](/assets/images/rpg/OP_CODE_READP.png)

#### READPE (Read Prior Equal)

![Opcode readpe](/assets/images/rpg/OP_CODE_READPE.png)

#### SETGT (Set Greater Than)

![Opcode setgt](/assets/images/rpg/OP_CODE_SETGT.png)

#### SETLL (Set Lower Limit)

![Opcode setll](/assets/images/rpg/OP_CODE_SETLL.png)

#### UNLOCK (Unlock a Data Area or Release a Record)

![Opcode unlock](/assets/images/rpg/OP_CODE_UNLOCK_1.png)

![Opcode unlock 2](/assets/images/rpg/OP_CODE_UNLOCK_2.png)

#### UPDATE (Modify Existing Record)

![Opcode update](/assets/images/rpg/OP_CODE_UPDATE.png)

#### WRITE (Create New Records)

![Opcode write](/assets/images/rpg/OP_CODE_WRITE.png)


### Indicator-Setting Operations

- [SETOFF (Set Indicator Off)](#setoff-set-indicator-off)
- [SETON (Set Indicator On)](#seton-set-indicator-on)

#### SETOFF (Set Indicator Off)

![Opcode setoff](/assets/images/rpg/OP_CODE_SETOFF.png)

#### SETON (Set Indicator On)

![Opcodde seton](/assets/images/rpg/OP_CODE_SETON.png)

### Initialization Operations

- [CLEAR (Clear)](#clear-clear)

#### CLEAR (Clear)

![Opcode clear](/assets/images/rpg/OP_CODE_CLEAR.png)

### Message Operations

- [DSPLY (Display Function)](#dsply-display-function)

#### DSPLY (Display Function)

![Opcode display](/assets/images/rpg/OP_CODE_DSPLY.png)

### Move Operations

- [MOVE (Move)](#move-move)
- [MOVEA (Move Array)](#movea-move-array)
- [MOVEL (Move Left)](#movel-move-left)

#### MOVE (Move)

![Opcode move](/assets/images/rpg/OP_CODE_MOVE.png)

#### MOVEA (Move Array)

![Opcode movea](/assets/images/rpg/OP_CODE_MOVEA.png)

#### MOVEL (Move Left)

![Opcode movel](/assets/images/rpg/OP_CODE_MOVEL.png)

### String Operations

- [CAT (Concatenate Two String)](#cat-concatenate-two-string)
- [CHECK (Check Caracters)](#check-check-caracters)
- [CHECKR (Check Reverse)](#checkr-check-reverse)
- [SCAN (Scan String)](#scan-scan-string)
- [SUBST (Substring)](#subst-substring)

#### CAT (Concatenate Two String)

![Opcode cat](/assets/images/rpg/OP_CODE_CAT.png)

![Opcode cat2](/assets/images/rpg/OP_CODE_CAT_2.png)

#### CHECK (Check Caracters)

![Opcode check](/assets/images/rpg/OP_CODE_CHECK.png)

#### CHECKR (Check Reverse)

![Opcode checkr](/assets/images/rpg/OP_CODE_CHECKR.png)

#### SCAN (Scan String)

![Opcode scan](/assets/images/rpg/OP_CODE_SCAN.png)

#### SUBST (Substring)

![Opcode subst](/assets/images/rpg/OP_CODE_SUBST.png)

### Subroutine Operations

- [BEGSR (Beginning of Subroutine)](#begsr-beginning-of-subroutine)
- [ENDSR (End of Subroutine)](#endsr-end-of-subroutine)
- [EXSR (Invoke Subroutine)](#exsr-invoke-subroutine)
- [CASxx (Conditionally Invoke Subroutine)](#casxx-conditionally-invoke-subroutine)

#### BEGSR (Beginning of Subroutine)

![Opcode begsr](/assets/images/rpg/OP_CODE_BEGSR.png)

#### ENDSR (End of Subroutine)

![Opcode endsr](/assets/images/rpg/OP_CODE_ENDSR.png)

#### EXSR (Invoke Subroutine)

![Opcode exsr](/assets/images/rpg/OP_CODE_EXSR.png)

#### CASxx (Conditionally Invoke Subroutine)

![Opcode casxx](/assets/images/rpg/OP_CODE_CASXX.png)

### Other Operations

- ELSE (Else)
- ENDyy (End a Structure Group)
- LEAVE (Leave a Do/For Group)
- OTHER (Otherwise Select)

โดย yy สามารถเป็นได้ดังนี้

| อักษร   | ความหมาย                          |
| ------ | --------------------------------- |
| CS     | End CASxx                         |
| DO     | End DO, DOU, DOW                  |
| FOR    | End FOR                           |
| IF     | End IF                            |
| SL     | End SELECT                        |
| Blanks | End ให้กับ Structure operation ใด ๆ |

## Display screens

Display screen หรือหน้าจอใช้สำหรับเป็น UI ในการใช้งาน AS/400 โดย Screen เราจะสร้าง
ไฟล์ไว้ใน **[LIB]/QDDSSRC**

วิธีสร้าง Screen สามารถทำได้โดย

1. กด create (F6) บนหน้า `WRKMBRPDM`
2. ใช้ CL command `STRSDA`

## Data Files

Data files หรือไฟล์ใช้เก็บข้อมูล 

ประเภทของ Data files 
1. Physical file 

Physical file เป็นไฟล์ที่ใช้เก็บข้อมูล และตัวไฟล์ยังใช้กำกับรูปแบบของข้อมูลด้วยว่าเป็นประเภทอะไร มีขนาดเท่าไร 

Physical file สามารถมี Record format ได้แค่ 1 ตัวต่อหนึ่ง Physical file เท่านั้

Physical file สามารถกำหนดคีย์ให้ได้

2. Logical file

Logical file ไม่ได้เก็บข้อมูล

Logical file เป็น View คอยเลือกเอา Field ที่ต้องการจาก Physical file นำไปใช้ต่อไป 

Logical file สามารถกำหนดคีย์ให้ได้

วิธีสร้าง Data files ทำได้โดยใช้คำสั่ง `STRSEU` หรือ กด `F6` บน `WRKMBRPDM`

## Practices

### Practice #1 - Hello world

![Op hello world](/assets/images/rpg/practice_hello.png)

### Practice #2 - Data structure 

![Op ds](/assets/images/rpg/practice_ds.png)

### Practice #3 - Loop

![Op loop](/assets/images/rpg/practice_loop_2.png)

### Practice #4 - Data files

### Practice #5 - Chain

### Practice #6 - Cahin with keys

### Practice #7 - Read

### Practice #8 - Read with keys

### Practice #9 - Program with parameters

### Practice #10 - CRUD
