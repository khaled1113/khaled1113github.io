<!DOCTYPE html>
<html>
<head>
    <title>Real-Time Weight Data</title>
    <script src="https://www.gstatic.com/firebasejs/9.0.0/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.0.0/firebase-database.js"></script>
    <script>
        document.addEventListener('DOMContentLoaded', function() {
            console.log('DOM fully loaded and parsed');

            // Your web app's Firebase configuration
            const firebaseConfig = {
                apiKey: "AIzaSyCENwRUTdGb7oa2ymsSGs2TDvN0z-aQIvA",
                authDomain: "esp32-c72b0.firebaseapp.com",
                databaseURL: "https://esp32-c72b0-default-rtdb.firebaseio.com",
                projectId: "esp32-c72b0",
                storageBucket: "esp32-c72b0.appspot.com",
                messagingSenderId: "1035271337480",
                appId: "1:1035271337480:web:4b4b43b6f518f1adfa83bd",
                measurementId: "G-ERER2ESDC3"
            };

            console.log('Firebase config:', firebaseConfig);

            // Initialize Firebase
            const app = firebase.initializeApp(firebaseConfig);
            const database = firebase.database();

            console.log('Firebase initialized:', app);

            function fetchData() {
                console.log('Fetching data...');
                const weightRef = database.ref('/weight');
                weightRef.on('value', (snapshot) => {
                    const data = snapshot.val();
                    console.log('Data from Firebase:', data);
                    document.getElementById('weight').innerText = 'Weight: ' + data + ' kg';
                }, (error) => {
                    console.error('Error fetching data:', error);
                });
            }

            fetchData();
        });
    </script>
</head>
<body>
    <h1>Real-Time Weight Data</h1>
    <div id="weight">Fetching weight...</div>
</body>
</html>
