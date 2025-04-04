<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dynamic Dropdown</title>
</head>
<body>

    <label for="nameDropdown">Select Name:</label>
    <select id="nameDropdown">
        <option value="">Loading...</option> <!-- Initially empty -->
    </select>

    <script>
        function getQueryParam(param) {
            const urlParams = new URLSearchParams(window.location.search);
            return urlParams.get(param);
        }

        function fixEscapedJson(escapedJson) {
            try {
                // Remove leading/trailing quotes and decode JSON properly
                let cleanedJson = escapedJson.replace(/^"|"$/g, '');
                cleanedJson = cleanedJson.replace(/\\"/g, '"'); // Fix escape characters
                return JSON.parse(cleanedJson);
            } catch (error) {
                console.error('Error decoding JSON:', error);
                return null;
            }
        }

        function populateDropdown() {
            let payload = getQueryParam('payload');

            if (payload) {
                let jsonData = fixEscapedJson(payload);
                if (jsonData && jsonData.records) {
                    let dropdown = document.getElementById('nameDropdown');
                    dropdown.innerHTML = '<option value="">Select Name</option>'; // Reset dropdown

                    jsonData.records.forEach(record => {
                        let option = document.createElement('option');
                        option.value = record.id;
                        option.textContent = record.name;
                        dropdown.appendChild(option);
                    });
                }
            } else {
                console.warn('No valid payload found in the URL');
            }
        }

        // Populate dropdown on page load
        window.onload = populateDropdown;
    </script>

</body>
</html>
# dropdown
