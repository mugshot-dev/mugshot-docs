---
sidebar_position: 1
---

# Express JS Face CRUD

Docusaurus can manage multiple versions of your docs.

```javascript
const express = require("express");
const multer = require("multer");
const { MugshotClient } = require("mugshot-js");

const app = express();
const mugshot = MugshotClient("API_KEY");

// Configure Multer storage
const storage = multer.diskStorage({
  destination: "uploads/",
  filename: (req, file, cb) => {
    cb(null, Date.now() + "-" + file.originalname);
  },
});

const upload = multer({ storage: storage });

// Define upload route
app.post("/upload", upload.single("file"), (req, res) => {
  if (req.file) {
    const blob = req.file.buffer;
    mugshot.addFace(blob, { name: "john" });
    res.json({ message: "File uploaded successfully!", filename: req.file.filename });
  } else {
    res.status(400).json({ message: "No file uploaded!" });
  }
});

app.post("/search", upload.single("file"), (req, res) => {
  if (req.file) {
    const blob = req.file.buffer;
    const result = mugshot.searchFace(blob);
    res.json({ message: "Success!", data: result.data });
  } else {
    res.status(400).json({ message: "No file uploaded!" });
  }
});

// Start server
app.listen(3000, () => {
  console.log("Server listening on port 3000");
});

```
