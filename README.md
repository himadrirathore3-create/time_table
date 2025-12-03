# Ex03 Time Table
# Date:27/11/2025
# AIM
To write a html webpage page to display your slot timetable.

# ALGORITHM
## STEP 1
Create a Django-admin Interface.

## STEP 2
Create a static folder and inert HTML code.

## STEP 3
Create a simple table using `<table>` tag in html.

## STEP 4
Add header row using `<th>` tag.

## STEP 5
Add your timetable using `<td>` tag.

## STEP 6
Execute the program using runserver command.

# PROGRAM
urls.py
```
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('timetable.urls')),
]
```
views.py
```
from __future__ import annotations

from typing import Any, Dict, List

from django.http import HttpRequest, HttpResponse
from django.shortcuts import render


DAYS: List[str] = [
    "Monday",
    "Tuesday",
    "Wednesday",
    "Thursday",
    "Friday",
]

TIME_SLOTS: List[Dict[str, Any]] = [
    {"id": "slot_1", "label": "08:30-09:20"},
    {"id": "slot_2", "label": "09:30-10:20"},
    {"id": "slot_3", "label": "10:30-11:20"},
    {"id": "slot_4", "label": "11:30-12:20"},
    # Slightly extended lunch slot compared to a typical timetable
    {"id": "lunch", "label": "12:20-13:10", "is_lunch": True},
    {"id": "slot_5", "label": "13:10-14:00"},
    {"id": "slot_6", "label": "14:10-15:00"},
]

TIMETABLE_ROWS: List[Dict[str, Any]] = [
    {
        "slot_id": "slot_1",
        "label": TIME_SLOTS[0]["label"],
        "cells": [
            {"code": "CS101", "room": "A1"},
            {"code": "MATH101", "room": "B2"},
            {"code": "ENG201", "room": "C3"},
            {"code": "HIST101", "room": "D1"},
            {"code": "PHY101", "room": "Lab 1"},
        ],
    },
    {
        "slot_id": "slot_2",
        "label": TIME_SLOTS[1]["label"],
        "cells": [
            {"code": "CS101", "room": "A1"},
            {"code": "MATH101", "room": "B2"},
            {"code": "ENG201", "room": "C3"},
            {"code": "HIST101", "room": "D1"},
            {"code": "PHY101", "room": "Lab 1"},
        ],
    },
    {
        "slot_id": "slot_3",
        "label": TIME_SLOTS[2]["label"],
        "cells": [
            {"code": "CS201", "room": "A2"},
            {"code": "MATH201", "room": "B2"},
            {"code": "ENG201", "room": "C3"},
            {"code": "HIST201", "room": "D1"},
            {"code": "PHY201", "room": "Lab 2"},
        ],
    },
    {
        # Lunch row: every cell is treated as a lunch break icon in the template.
        "slot_id": "lunch",
        "label": TIME_SLOTS[4]["label"],
        "is_lunch": True,
        "cells": [
            {"is_lunch": True} for _ in range(len(DAYS))
        ],
    },
    {
        "slot_id": "slot_5",
        "label": TIME_SLOTS[5]["label"],
        "cells": [
            {"code": "CS301", "room": "A3"},
            {"code": "MATH301", "room": "B3"},
            {"code": "CSLAB", "room": "Lab 3"},
            {"code": "ELEC101", "room": "E1"},
            {"code": "PHY301", "room": "Lab 1"},
        ],
    },
    {
        "slot_id": "slot_6",
        "label": TIME_SLOTS[6]["label"],
        "cells": [
            {"code": "CS301", "room": "A3"},
            {"code": "MATH301", "room": "B3"},
            {"code": "CSLAB", "room": "Lab 3"},
            {"code": "ELEC101", "room": "E1"},
            {"code": "PHY301", "room": "Lab 1"},
        ],
    },
]

COURSES: List[Dict[str, str]] = [
    {"code": "CS101", "title": "Introduction to Programming", "instructor": "Dr Rivera"},
    {"code": "CS201", "title": "Data Structures", "instructor": "Dr Patel"},
    {"code": "CS301", "title": "Algorithms", "instructor": "Dr Chen"},
    {"code": "CSLAB", "title": "Programming Lab", "instructor": "Ms Green"},
    {"code": "MATH101", "title": "Calculus I", "instructor": "Dr Brown"},
    {"code": "MATH201", "title": "Linear Algebra", "instructor": "Dr Brown"},
    {"code": "MATH301", "title": "Discrete Mathematics", "instructor": "Dr Yoon"},
    {"code": "ENG201", "title": "Academic Writing", "instructor": "Prof Stone"},
    {"code": "HIST101", "title": "World History", "instructor": "Dr Khan"},
    {"code": "HIST201", "title": "Modern History", "instructor": "Dr Khan"},
    {"code": "PHY101", "title": "Physics I", "instructor": "Dr Liu"},
    {"code": "PHY201", "title": "Physics II", "instructor": "Dr Liu"},
    {"code": "PHY301", "title": "Applied Physics", "instructor": "Dr Walker"},
    {"code": "ELEC101", "title": "Elective Seminar", "instructor": "Staff"},
]


def term_timetable(request: HttpRequest) -> HttpResponse:
    """Render the term timetable page."""
    context = {
        "days": DAYS,
        "time_slots": TIME_SLOTS,
        "timetable_rows": TIMETABLE_ROWS,
        "courses": COURSES,
    }
    return render(request, "timetable/slottimetable.html", context)
```
.html
```
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Timetable</title>
  <link rel="icon" href="sec_icon.webp">
  <style>
    table {
      border-collapse: collapse;
      width: 100%;
      text-align: center;
    }
    th, td {
      border: 2px solid black;
      padding: 10px;
    }
    th {
      background-color: rgb(147, 92, 118);
      color: black;
    }
    .vertical-word {
      background-color: rgb(217, 241, 214);
      font-weight: bolder;
      text-align: center;
      vertical-align: middle;
      width: 50px;
      font-size: 18px;
    }
    .vertical-word span {
      display: block;
      line-height: 1.8;
    }
    h2{
      text-align: center;
      margin-top: 10px
    }
    h3{
      text-align: center;
    }
  </style>
</head>
<body>
  <div style="text-align: center; margin: 15px 0;">
    <img src="SEC HEADER.jpg" height="100" width="87%">
  </div>

  <h2>SAVEETHA ENGINEERING COLLEGE</h2>
  <H3>SLOT TIME TABLE - <span style="color:rgb(41, 12, 158)"> HIMADRI S(25011498)</span></H3>
  <table>
    <tr>
      <th>Day/Time</th>
      <th>08:00-10:00</th>
      <th>10:00-12:00</th>
      <th class="vertical-word" rowspan="7">
        <span style="color: black;">L</span>
        <span style="color: black;">U</span>
        <span style="color: black;">N</span>
        <span style="color: black;">C</span>
        <span style="color: black;">H</span>
      </th>
      <th>1:00-3:00</th>
      <th>3:00-5:00</th>
    </tr>
    <tr>
      <td>Monday</td>
      <td>FWAD</td>
      <td>FREE SLOT</td>
      <td>FWAD</td>
      <td>FREE SLOT</td>
    </tr>
    <tr>
      <td>Tuesday</td>
      <td>FWAD</td>
      <td>FREE SLOT</td>
      <td>FREE SLOT</td>
      <td>PYTHON</td>
    </tr>
    <tr>
      <td>Wednesday</td>
      <td>FWAD</td>
      <td>PYTHON</td>
      <td>MENTOR MEET</td>
      <td>FREE SLOT</td>
    </tr>
    <tr>
      <td>Thursday</td>
      <td colspan="2" align="center">FREE SLOT</td>
      <td>FREE SLOT</td>
      <td>PYTHON</td>
    </tr>
    <tr>
      <td>Friday</td>
      <td colspan="2" align="center">FREE SLOT</td>
      <td>FWAD</td>
      <td>FREE SLOT</td>
    </tr>
    <tr>
      <td>Saturday</td>
      <td colspan="2" align="center">FREE SLOT</td>
      <td colspan="2" align="center">PYTHON</td>
    </tr>
  </table>
  <H3></H3>
  <table>
    <tr>
      <th>S.NO</th>
      <th>Subject Code</th>
      <th>Subject Name</th>
    </tr>
    <tr>
      <td>1</td>
      <td>19AI414</td>
      <td>Fundamentals of Web Applicaation Development(FWAD)</td>
    </tr>
    <tr>
      <td>2</td>
      <td>19AI301</td>
      <td>Python Programming(PYTHON)</td>
    </tr>
  </table>
</body>
</html>
```
# OUTPUT
![alt text](<Screenshot 2025-11-27 223033.png>)


# RESULT
The program for creating slot timetable using basic HTML tags is executed successfully.
