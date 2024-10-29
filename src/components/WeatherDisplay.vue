<template>
    <div v-if="weatherData" class="weather-display">
      <h2>{{ weatherData.name }}, {{ getCountryName(weatherData.sys.country) }}
        <img :src="getFlagUrl(weatherData.sys.country)" alt="Flag" class="country-flag" />
      </h2>
      <div class="weather-details">
        <p>
            <strong>Condition:</strong> {{ weatherData.weather[0].description }}
            <img :src="getWeatherIcon(weatherData.weather[0].icon)" alt="Weather Icon" class="weather-icon" />
          </p>          

        <p><strong>Temperature:</strong> {{ (weatherData.main.temp).toFixed(1) }} °C</p>
        <p><strong>Feels Like:</strong> {{ (weatherData.main.feels_like).toFixed(1) }} °C</p>
        <p><strong>Condition:</strong> {{ weatherData.weather[0].description }}</p>
        <p><strong>Humidity:</strong> {{ weatherData.main.humidity }} %</p>
        <p><strong>Pressure:</strong> {{ weatherData.main.pressure }} hPa</p>
        <p><strong>Wind Speed:</strong> {{ weatherData.wind.speed }} m/s</p>
        <p><strong>Wind Direction:</strong> {{ getWindDirection(weatherData.wind.deg) }}</p>
        <p><strong>Visibility:</strong> {{ (weatherData.visibility / 1000).toFixed(1) }} km</p>
        <p><strong>Cloudiness:</strong> {{ weatherData.clouds.all }} %</p>
        <p><strong>Sunrise:</strong> {{ formatTime(weatherData.sys.sunrise) }}</p>
        <p><strong>Sunset:</strong> {{ formatTime(weatherData.sys.sunset) }}</p>
        <p v-if="weatherData.rain"><strong>Rain:</strong> {{ weatherData.rain['1h'] || weatherData.rain['3h'] }} mm/h</p>
        <p v-if="weatherData.snow"><strong>Snow:</strong> {{ weatherData.snow['1h'] || weatherData.snow['3h'] }} mm/h</p>
        <p v-if="uvIndex"><strong>UV Index:</strong> {{ uvIndex }}</p>
      </div>
    </div>
  
    <div v-else class="no-data">
      <p>No weather data available. Please search for a city.</p>
    </div>
</template>
<script>
export default {
    props: ['weatherData'],
    methods: {
        getWeatherIcon(iconCode) {
      // Assuming you are using OpenWeatherMap API, this URL works for their icons.
      return `http://openweathermap.org/img/wn/${iconCode}@2x.png`;
    },
    getWindDirection(degrees) {
      const directions = ['North', 'North-East', 'East', 'South-East', 'South', 'South-West', 'West', 'North-West'];
      const index = Math.round(degrees / 45) % 8;
      return directions[index];
    },
    formatTime(unixTime) {
      const date = new Date(unixTime * 1000); // Convert from UNIX timestamp to milliseconds
      return date.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit', hour12: true });
    },
    getCountryName(countryCode) {
      const countryNames = {
        "AF": "Afghanistan",
        "AL": "Albania",
        "DZ": "Algeria",
        "AS": "American Samoa",
        "AD": "Andorra",
        "AO": "Angola",
        "AI": "Anguilla",
        "AQ": "Antarctica",
        "AG": "Antigua and Barbuda",
        "AR": "Argentina",
        "AM": "Armenia",
        "AW": "Aruba",
        "AU": "Australia",
        "AT": "Austria",
        "AZ": "Azerbaijan",
        "BS": "Bahamas",
        "BH": "Bahrain",
        "BD": "Bangladesh",
        "BB": "Barbados",
        "BY": "Belarus",
        "BE": "Belgium",
        "BZ": "Belize",
        "BJ": "Benin",
        "BM": "Bermuda",
        "BT": "Bhutan",
        "BO": "Bolivia",
        "BQ": "Bonaire, Sint Eustatius and Saba",
        "BA": "Bosnia and Herzegovina",
        "BW": "Botswana",
        "BV": "Bouvet Island",
        "BR": "Brazil",
        "IO": "British Indian Ocean Territory",
        "BN": "Brunei Darussalam",
        "BG": "Bulgaria",
        "BF": "Burkina Faso",
        "BI": "Burundi",
        "CV": "Cabo Verde",
        "KH": "Cambodia",
        "CM": "Cameroon",
        "CA": "Canada",
        "KY": "Cayman Islands",
        "CF": "Central African Republic",
        "TD": "Chad",
        "CL": "Chile",
        "CN": "China",
        "CX": "Christmas Island",
        "CC": "Cocos (Keeling) Islands",
        "CO": "Colombia",
        "KM": "Comoros",
        "CD": "Congo, Democratic Republic of the",
        "CG": "Congo",
        "CK": "Cook Islands",
        "CR": "Costa Rica",
        "HR": "Croatia",
        "CU": "Cuba",
        "CW": "Curaçao",
        "CY": "Cyprus",
        "CZ": "Czech Republic",
        "DK": "Denmark",
        "DJ": "Djibouti",
        "DM": "Dominica",
        "DO": "Dominican Republic",
        "EC": "Ecuador",
        "EG": "Egypt",
        "SV": "El Salvador",
        "GQ": "Equatorial Guinea",
        "ER": "Eritrea",
        "EE": "Estonia",
        "SZ": "Eswatini",
        "ET": "Ethiopia",
        "FK": "Falkland Islands (Malvinas)",
        "FO": "Faroe Islands",
        "FJ": "Fiji",
        "FI": "Finland",
        "FR": "France",
        "GF": "French Guiana",
        "PF": "French Polynesia",
        "TF": "French Southern Territories",
        "GA": "Gabon",
        "GM": "Gambia",
        "GE": "Georgia",
        "DE": "Germany",
        "GH": "Ghana",
        "GI": "Gibraltar",
        "GR": "Greece",
        "GL": "Greenland",
        "GD": "Grenada",
        "GP": "Guadeloupe",
        "GU": "Guam",
        "GT": "Guatemala",
        "GG": "Guernsey",
        "GN": "Guinea",
        "GW": "Guinea-Bissau",
        "GY": "Guyana",
        "HT": "Haiti",
        "HM": "Heard Island and McDonald Islands",
        "VA": "Holy See",
        "HN": "Honduras",
        "HK": "Hong Kong",
        "HU": "Hungary",
        "IS": "Iceland",
        "IN": "India",
        "ID": "Indonesia",
        "IR": "Iran, Islamic Republic of",
        "IQ": "Iraq",
        "IE": "Ireland",
        "IM": "Isle of Man",
        "IL": "Israel",
        "IT": "Italy",
        "JM": "Jamaica",
        "JP": "Japan",
        "JE": "Jersey",
        "JO": "Jordan",
        "KZ": "Kazakhstan",
        "KE": "Kenya",
        "KI": "Kiribati",
        "KP": "Korea, Democratic People’s Republic of",
        "KR": "Korea, Republic of",
        "KW": "Kuwait",
        "KG": "Kyrgyzstan",
        "LA": "Lao People’s Democratic Republic",
        "LV": "Latvia",
        "LB": "Lebanon",
        "LS": "Lesotho",
        "LR": "Liberia",
        "LY": "Libya",
        "LI": "Liechtenstein",
        "LT": "Lithuania",
        "LU": "Luxembourg",
        "MO": "Macao",
        "MG": "Madagascar",
        "MW": "Malawi",
        "MY": "Malaysia",
        "MV": "Maldives",
        "ML": "Mali",
        "MT": "Malta",
        "MH": "Marshall Islands",
        "MQ": "Martinique",
        "MR": "Mauritania",
        "MU": "Mauritius",
        "YT": "Mayotte",
        "MX": "Mexico",
        "FM": "Micronesia, Federated States of",
        "MD": "Moldova, Republic of",
        "MC": "Monaco",
        "MN": "Mongolia",
        "ME": "Montenegro",
        "MS": "Montserrat",
        "MA": "Morocco",
        "MZ": "Mozambique",
        "MM": "Myanmar",
        "NA": "Namibia",
        "NR": "Nauru",
        "NP": "Nepal",
        "NL": "Netherlands",
        "NC": "New Caledonia",
        "NZ": "New Zealand",
        "NI": "Nicaragua",
        "NE": "Niger",
        "NG": "Nigeria",
        "NU": "Niue",
        "NF": "Norfolk Island",
        "MP": "Northern Mariana Islands",
        "NO": "Norway",
        "OM": "Oman",
        "PK": "Pakistan",
        "PW": "Palau",
        "PS": "Palestine, State of",
        "PA": "Panama",
        "PG": "Papua New Guinea",
        "PY": "Paraguay",
        "PE": "Peru",
        "PH": "Philippines",
        "PN": "Pitcairn",
        "PL": "Poland",
        "PT": "Portugal",
        "PR": "Puerto Rico",
        "QA": "Qatar",
        "RE": "Réunion",
        "RO": "Romania",
        "RU": "Russian Federation",
        "RW": "Rwanda",
        "BL": "Saint Barthélemy",
        "SH": "Saint Helena, Ascension and Tristan da Cunha",
        "KN": "Saint Kitts and Nevis",
        "LC": "Saint Lucia",
        "MF": "Saint Martin (French part)",
        "SX": "Saint Martin (Dutch part)",
        "PM": "Saint Pierre and Miquelon",
        "VC": "Saint Vincent and the Grenadines",
        "WS": "Samoa",
        "SM": "San Marino",
        "ST": "Sao Tome and Principe",
        "SA": "Saudi Arabia",
        "SN": "Senegal",
        "RS": "Serbia",
        "SC": "Seychelles",
        "SL": "Sierra Leone",
        "SG": "Singapore",
       // "SX": "Sint Maarten (Dutch part)",
        "SK": "Slovakia",
        "SI": "Slovenia",
        "SB": "Solomon Islands",
        "SO": "Somalia",
        "ZA": "South Africa",
        "GS": "South Georgia and the South Sandwich Islands",
        "SS": "South Sudan",
        "ES": "Spain",
        "LK": "Sri Lanka",
        "SD": "Sudan",
        "SR": "Suriname",
        "SJ": "Svalbard and Jan Mayen",
      //  "SZ": "Swaziland",
        "SE": "Sweden",
        "CH": "Switzerland",
        "SY": "Syrian Arab Republic",
        "TW": "Taiwan, Province of China",
        "TJ": "Tajikistan",
        "TZ": "Tanzania, United Republic of",
        "TH": "Thailand",
        "TL": "Timor-Leste",
        "TG": "Togo",
        "TK": "Tokelau",
        "TO": "Tonga",
        "TT": "Trinidad and Tobago",
        "TN": "Tunisia",
        "TR": "Turkey",
        "TM": "Turkmenistan",
        "TC": "Turks and Caicos Islands",
        "TV": "Tuvalu",
        "UG": "Uganda",
        "UA": "Ukraine",
        "AE": "United Arab Emirates",
        "GB": "United Kingdom",
        "US": "United States of America",
        "UM": "United States Minor Outlying Islands",
        "UY": "Uruguay",
        "UZ": "Uzbekistan",
        "VU": "Vanuatu",
        "VE": "Venezuela",
        "VN": "Viet Nam",
        "WF": "Wallis and Futuna",
        "EH": "Western Sahara",
        "YE": "Yemen",
        "ZM": "Zambia",
        "ZW": "Zimbabwe"

      
      };
      return countryNames[countryCode] || countryCode;
    },
    getFlagUrl(countryCode) {
      return `https://flagcdn.com/16x12/${countryCode.toLowerCase()}.png`;
    }
  }
};
    </script>
    
    <style scoped>
    .weather-display {
        background-color: #ffffff; /* White background */
        padding: 20px;
        border-radius: 10px;
        box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1); /* Subtle shadow */
        margin: 20px auto;
        max-width: 400px;
        text-align: left;
        font-family: 'Arial', sans-serif;
      }
      
      /* Heading style for city and country */
      .weather-display h2 {
        color: #2c3e50;
        font-size: 1.8rem;
        margin-bottom: 15px;
        border-bottom: 2px solid #3498db; /* Light blue border */
        padding-bottom: 5px;
      }
      
      .country-flag {
        widows: 30px;
        height: 20px;
        vertical-align: middle;
        margin-left: 5px;
        border:1px solid #ccc;
        border-radius: 3px;
       
      }
      /* Details section styling */
      .weather-details p {
        font-size: 1.1rem;
        color: #555555;
        margin: 8px 0;
      }
      .weather-icon {
        width: 60px; /* or any size that fits your design */
        height: auto; /* maintain aspect ratio */
        vertical-align: middle; /* align with the text if needed */
      }
      .weather-details p strong {
        color: #2c3e50; /* Darker color for labels */
      }
      
      /* No data message styling */
      .no-data {
        text-align: center;
        font-size: 1.2rem;
        color: #e74c3c; /* Red color for the alert */
        margin-top: 20px;
        font-weight: bold;
      }
    </style>