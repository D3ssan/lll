<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>حساب زاوية الأفق الشمسي</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            margin: 0;
            padding: 20px;
        }
        h1 {
            text-align: center;
        }
        .container {
            max-width: 600px;
            margin: auto;
            background: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        label {
            display: block;
            margin: 10px 0 5px;
        }
        input {
            width: 100%;
            padding: 10px;
            margin-bottom: 15px;
            border: 1px solid #ccc;
            border-radius: 4px;
        }
        button {
            background-color: #28a745;
            color: white;
            padding: 10px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            width: 100%;
        }
        button:hover {
            background-color: #218838;
        }
        #result {
            margin-top: 20px;
            font-size: 18px;
            text-align: center;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>حساب زاوية الأفق الشمسي</h1>
    <label for="latitude">خط العرض (بالدرجات):</label>
    <input type="number" id="latitude" placeholder="مثال: 30.0444" required>

    <label for="longitude">خط الطول (بالدرجات):</label>
    <input type="number" id="longitude" placeholder="مثال: 31.2357" required>

    <label for="date">التاريخ (YYYY-MM-DD):</label>
    <input type="date" id="date" required>

    <label for="time">الوقت (HH:MM):</label>
    <input type="time" id="time" required>

    <button onclick="calculateSolarAzimuth()">احسب زاوية الأفق الشمسي</button>

    <div id="result"></div>
</div>

<script>
    function calculateSolarAzimuth() {
        const latitude = parseFloat(document.getElementById('latitude').value);
        const longitude = parseFloat(document.getElementById('longitude').value);
        const date = new Date(document.getElementById('date').value + 'T' + document.getElementById('time').value);
        
        // حساب الزاوية
        const dayOfYear = Math.floor((date - new Date(date.getFullYear(), 0, 0)) / 1000 / 60 / 60 / 24);
        const solarDeclination = 23.45 * Math.sin((360 / 365) * (dayOfYear - 81) * (Math.PI / 180));
        const timeOffset = (date.getHours() + date.getMinutes() / 60) - 12;
        const solarTime = timeOffset + (longitude / 15);
        const solarAltitude = Math.asin(Math.sin(latitude * (Math.PI / 180)) * Math.sin(solarDeclination * (Math.PI / 180)) +
            Math.cos(latitude * (Math.PI / 180)) * Math.cos(solarDeclination * (Math.PI / 180)) * Math.cos((solarTime * 15) * (Math.PI / 180)));
        
        const solarAzimuth = Math.atan2(-Math.sin((solarTime * 15) * (Math.PI / 180)),
            (Math.cos((solarTime * 15) * (Math.PI / 180)) * Math.sin(latitude * (Math.PI / 180)) -
            Math.tan(solarDeclination * (Math.PI / 180)) * Math.cos(latitude * (Math.PI / 180))));
        
