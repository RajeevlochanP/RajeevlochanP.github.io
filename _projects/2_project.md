---
layout: page
title: Advantage
description: A Full stack Ecommerce like reselling platform
img: assets/img/3.jpg
importance: 2
category: work
giscus_comments: false
---

**AdVantage** is a full-stack, peer-to-peer e-commerce platform that uniquely combines reselling and renting functionalities. It enables users to securely list, sell, or rent items through a dynamic and scalable marketplace. The platform features secure JWT-based authentication, efficient product management, and a robust API for handling user transactions.

---

### Architecture Overview

AdVantage is built on the MERN stack (MongoDB, Express, React, Node.js). The backend handles user authentication, product listing management (for both sale and rent), and transaction processing. The Node.js server exposes a RESTful API, secured with JWT middleware to protect user-specific routes. MongoDB stores user data, product listings, and transaction histories, with optimized schemas for fast queries.

![System Architecture Diagram](assets/img/architecture.jpg)

---

### Core Features

The platform's frontend, built with React, provides a dynamic user experience for browsing, searching, and managing listings. It communicates with the backend via authenticated API calls to perform actions like creating new listings, placing sell requests, or initiating rental requests.

**Key features include:**
* **Secure Authentication:** User accounts are protected using JSON Web Tokens (JWT), ensuring secure access to personal data and transaction history.
* **Dynamic Product Management:** Users have full CRUD (Create, Read, Update, Delete) control over their listings through a clean, intuitive dashboard.

![User Product Management Dashboard](assets/img/advantage-dashboard.png)

Here is the code of File handling using multer in backend

{% raw %}
```js
const storage = multer.diskStorage({
    destination: (req, file, cb) => {
        let uploadPath = "uploads/";
        let folderPath;
        if (file.fieldname === "productImages") {
            if (!req.uniqueFolderPath) {
                uploadPath += "productImages/";
                const folderId = Date.now() + "-" + req.user._id;
                folderPath = path.join(uploadPath, folderId);
                req.uniqueFolderPath = folderPath; //remember this field!!
            }else{
                folderPath=req.uniqueFolderPath;
            }
        } else if (file.fieldname === "profilePic") {
            folderPath = path.join(uploadPath, "profilePics");
        } else if (file.fieldname === "invoice") {
            if(req.uniqueFolderPath){
                folderPath = req.uniqueFolderPath;
            }else{
                uploadPath += "productImages/";
                const folderId = Date.now() + "-" + req.user._id;
                folderPath = path.join(uploadPath, folderId);
                req.uniqueFolderPath = folderPath; //remember this field!!
            }
        }
        ensureDirectoryExists(folderPath);
        cb(null, folderPath);
    },
    filename: (req, file, cb) => {
        //as any way storing in unique folder for every file
        let fileName;
        if (file.fieldname === "profilePic") {
            const extension = path.extname(file.originalname);
            fileName = req.user._id + extension;
        } else {
            fileName = path.basename(file.originalname);
        }
        cb(null, fileName);
    }
});
```
{% endraw %}