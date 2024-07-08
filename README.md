<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>ESP32 Weight Display</title>
  <style>
    html, body {
      font-family: Arial, sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100%;
      margin: 0;
      background-color: #f0f0f0;
    }
    .container {
      width: 90%;
      max-width: 600px;
      background: #fff;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
      text-align: center;
    }
    h1 {
      color: #333;
      font-size: 2.5em;
      margin-bottom: 20px;
    }
    p {
      font-size: 1.5em;
      margin: 10px 0;
    }
    input {
      padding: 15px;
      font-size: 1.2em;
      border: 1px solid #ccc;
      border-radius: 4px;
      margin-top: 10px;
      width: 80%;
      max-width: 300px;
    }
    button {
      padding: 15px 30px;
      margin: 20px 10px;
      font-size: 1.2em;
      color: #fff;
      background-color: #007BFF;
      border: none;
      border-radius: 4px;
      cursor: pointer;
    }
    button:hover {
      background-color: #0056b3;
    }
  </style>
  <script>
    let lastAlertTime = 0;
    async function getWeightAndUpdateUnits() {
      let response = await fetch('/weight');
      let weight = await response.text();
      document.getElementById('weight').innerText = 'Weight: ' + weight + ' kg';
      let unitWeight = parseFloat(document.getElementById('unitWeight').value);
      if (!isNaN(unitWeight) && unitWeight > 0) {
        let totalWeight = parseFloat(weight);
        let numberOfUnits = Math.floor(totalWeight / unitWeight);
        document.getElementById('units').innerText = 'Number of Units: ' + numberOfUnits;
        let fractionalPart = (totalWeight % unitWeight).toFixed(1);
        if (fractionalPart != 0.0) {
          let currentTime = Date.now();
          if (currentTime - lastAlertTime >= 10000) {
            playAlert();
            alert('Total weight is not a multiple of unit weight!');
            lastAlertTime = currentTime;
          }
        }
      }
    }
    async function tareScale() {
      await fetch('/tare');
      alert('Scale tared successfully');
    }
    function playAlert() {
      const audio = new Audio('data:audio/wav;base64,UklGRiQAAABXQVZFZm10IBAAAAABAAEARKwAABCxAgAEABAAZGF0YYQAAAB/8wAAwP8AAAAA////AAD//wAAwP8AAP8AAAD//wAA//8AAP8AAP//AAD//wAA//8AAP8AAAAA////AAD//wAA//8AAP8AAAD//wAA//8AAAD/AP//AAD//wAAwP8AAP8AAAD//wAAwP8AAP8AAAAA//8AAP//AAD//wAA//8AAAAA');
      audio.play();
    }
    setInterval(getWeightAndUpdateUnits, 1000);
  </script>
</head>
<body>
  <div class='container'>
    <h1>ESP32 Weight Display</h1>
    <p id='weight'>Weight: </p>
    <input type='number' id='unitWeight' placeholder='Enter unit weight (kg)' step='0.001'>
    <p id='units'>Number of Units: </p>
    <button onclick='tareScale()'>Tare Scale</button>
  </div>
</body>
</html>
