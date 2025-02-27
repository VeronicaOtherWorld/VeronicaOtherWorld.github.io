---
title: "02/11 Diving into Backend Development: A New Challenge"
date: 2025-02-11 23:20:00 +0100
categories:
  [study, journal]
tags: 
  - [journal]
---

Last week, during a chat with a friend, we decided to start working on an affiliate e-commerce project, covering the frontend, backend, and database. Interestingly, we decided to switch roles this time—I will be responsible for the backend, while she focuses on the frontend.

The reason I initially started learning Java was that the APIs provided by others often crashed, so I wanted to write my own. After all this time, I may not have fully mastered Java yet, but I’m ready to dive in and gain hands-on experience.

Based on my current knowledge, I have already created MySQL tables and linked one of them to my Java project. The basic functionalities are working, and I have successfully tested them using Postman. 

<img src="assets/diary-pic/2025-02-11.jpg" alt="Diary Image" style="border: 1px solid black; border-radius: 2px; animation: none;"/>

My next step is to implement similar functionality for three other database tables, ensuring they reach the same level of completion. Additionally, I need to write API documentation for my frontend developer friend.

Even though I have completed some of the work, I still feel there are gaps in my understanding. That’s why I plan to continue learning Spring in a more structured way through an Udemy course, focusing on areas where I need deeper knowledge.

This whole experience is exciting—it’s learning through practice, and it feels great!

===
==Some API description==  

### ✅ get all active banners
- **URL:** `GET /api/banners`
- **description:** get all active `banners`
- **example request:**
```bash
curl -X GET http://localhost:8080/api/banners
```
- **example response:**
```json
[
  {
    "id": 1,
    "title": "Homepage Banner",
    "imageUrl": "https://example.com/banner1.jpg",
    "description": "Main homepage banner",
    "status": true
  }
]
```