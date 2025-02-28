<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Background Remover</title>
    <link rel="stylesheet" href="style.css">
    <style>
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
    </style>
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
