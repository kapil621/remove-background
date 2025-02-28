1. **Backend (server.js - Node.js + Express.js)**

```javascript
require('dotenv').config();
const express = require('express');
const multer = require('multer');
const fetch = require('node-fetch');
const cors = require('cors');

const app = express();
const upload = multer();
app.use(cors());

app.post('/remove-bg', upload.single('image'), async (req, res) => {
    try {
        const response = await fetch('https://api.remove.bg/v1.0/removebg', {
            method: 'POST',
            headers: {
                'X-Api-Key': process.env.REMOVE_BG_API_KEY
            },
            body: req.file.buffer
        });

        if (!response.ok) throw new Error('Failed to remove background');

        const result = await response.buffer();
        res.set('Content-Type', 'image/png');
        res.send(result);
    } catch (error) {
        res.status(500).send({ error: error.message });
    }
});

app.listen(3000, () => console.log('Server running on port 3000'));
```

2. **.env File (API Key Securely Store करें)**
```
REMOVE_BG_API_KEY=Dhc8rs4oggADgQsnzgHVXg1B
```

3. **Frontend (index.html - HTML + JavaScript)**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Background Remover</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container">
        <h2>Upload Image to Remove Background</h2>
        <input type="file" id="uploadImage" accept="image/*">
        <button onclick="removeBackground()">Remove Background</button>
        <div class="result">
            <img id="outputImage" src="" alt="Result Image">
        </div>
    </div>
    <script>
        async function removeBackground() {
            let fileInput = document.getElementById("uploadImage");
            let file = fileInput.files[0];
            
            if (!file) {
                alert("Please upload an image!");
                return;
            }
            
            let formData = new FormData();
            formData.append("image", file);
            
            let response = await fetch("http://localhost:3000/remove-bg", {
                method: "POST",
                body: formData
            });
            
            if (!response.ok) {
                alert("Error removing background!");
                return;
            }
            
            let resultBlob = await response.blob();
            let outputImage = document.getElementById("outputImage");
            outputImage.src = URL.createObjectURL(resultBlob);
        }
    </script>
</body>
</html>
```

4. **CSS (style.css - UI Design के लिए)**
```css
body {
    font-family: Arial, sans-serif;
    text-align: center;
    margin: 50px;
    background-color: #f4f4f4;
}
.container {
    background: white;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    max-width: 400px;
    margin: auto;
}
.result img {
    max-width: 100%;
    margin-top: 20px;
}
```

### 🚀 **Final Steps to Run & Deploy**
1️⃣ **Backend Install करें**:
```sh
npm install express dotenv multer node-fetch cors
```
2️⃣ **`.env` File में API Key डालें।**
3️⃣ **Server Run करें**:
```sh
node server.js
```
4️⃣ **Frontend को Run करें** (`index.html` खोलें)।
5️⃣ **GitHub पर Upload करें और Live करें**! 😊
