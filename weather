import sys
import requests

from PyQt5.QtWidgets import (QApplication, QWidget, QLabel,
                             QPushButton, QVBoxLayout, QLineEdit)
from PyQt5.QtCore import Qt

class WeatherApp(QWidget):
    def __init__(self):
        super().__init__()
        self.label = QLabel("Enter city name:", self)
        self.line = QLineEdit(self)
        self.button = QPushButton("Get Weather", self)
        self.temperature = QLabel(self)
        self.emoji = QLabel(self)
        self.description = QLabel(self)
        self.initUI()

    def initUI(self):
# Window config
        self.setWindowTitle("Weather App")

# Layout config
        vbox = QVBoxLayout()
        vbox.addWidget(self.label)
        vbox.addWidget(self.line)
        vbox.addWidget(self.button)
        vbox.addWidget(self.temperature)
        vbox.addWidget(self.emoji)
        vbox.addWidget(self.description)

        self.setLayout(vbox)
        self.label.setAlignment(Qt.AlignCenter)
        self.line.setAlignment(Qt.AlignCenter)
        self.temperature.setAlignment(Qt.AlignCenter)
        self.emoji.setAlignment(Qt.AlignCenter)
        self.description.setAlignment(Qt.AlignCenter)

# Setting names
        self.label.setObjectName("title")
        self.line.setObjectName("line")
        self.button.setObjectName("button")
        self.temperature.setObjectName("temp")
        self.emoji.setObjectName("emoji")
        self.description.setObjectName("desc")


# Css config
        self.setStyleSheet("""
            QLabel, QPushButton{
                font-family: Calibri;
            }
            QLabel#title{
                font-size: 40px;
                font-style: Italic;
            }
            
            QLineEdit#line{
                font-size: 40px;
            }
            
            QPushButton#button{
                font-size: 30px;
                font-weight: Bold;
            }
            
            QLabel#temp{
                font-size: 75px;
            }
            
            QLabel#emoji{
                font-size: 100px;
                font-family: segoe UI emoji;
            }
            
            QLabel#desc{
                font-size: 50px;
            }
            
        """)

        self.button.clicked.connect(self.get_weather)

# Line Edit config
        self.line.setAlignment(Qt.AlignCenter)



    def get_weather(self):

        key = "7bc6b2b050b8b544c78949c55f9f1702"
        city = self.line.text()
        url = f"https://api.openweathermap.org/data/2.5/weather?q={city}&appid={key}"

# Config Errors

        try:
            response = requests.get(url)
            response.raise_for_status() #-> the "try" block usually don't catch HTTPErrors,
            data = response.json()      # we need to use raise_for_status to be able to catch it

            if data["cod"] == 200:
                self.weather_display(data)

        except requests.exceptions.HTTPError as http_error:

            match response.status_code: #-> General/Normal errors

                case 400:
                    self.error("Bad request:\nCheck your input!")

                case 401:
                    self.error("Unauthenticated:\nInvalid API Key!")

                case 403:
                    self.error("Forbidden:\nAccess is denied!")

                case 404:
                    self.error("Not Found:\nThe City was not found!")

                case 500:
                    self.error("Internal Server Error:\nPlease try again later!")

                case 502:
                    self.error("Bad gateway:\nInvalid response from the server!")

                case 503:
                    self.error("Service Unavailable:\nServer is down!")

                case 504:
                    self.error("Gateway Timeout:\nServer is not responding!")

                case _:
                    self.error(f"HTTP error occurred:\n{http_error}")

        except requests.exceptions.ConnectionError:
            self.error("Connection Error:\nCheck your internet!")

        except requests.exceptions.Timeout:
            self.error("Timeout Error:\nThe request timed out!")

        except requests.exceptions.TooManyRedirects:
            self.error("Too Many Redirects:\nCheck your Url!")

        except requests.exceptions.RequestException as req_error: #-> network problems, invalid url
            self.error(f"Request Error:\n{req_error}")


    def error(self, message):
        self.temperature.setStyleSheet("Font-size: 35px;")
        self.temperature.setText(message)
        self.emoji.clear()
        self.description.clear()

    def weather_display(self, data):
# Temperature label
        self.temperature.setStyleSheet("Font-size: 75px;")
        temperature_k = data["main"]["temp"]
        temperature_c = temperature_k - 273.15

        self.temperature.setText(f"{temperature_c:.0f}°C")

# Emoji ID

        weather_id = data["weather"][0]["id"]
        self.emoji.setText(self.get_emoji(weather_id))

# Desc Label
        desc = data["weather"][0]["description"]
        self.description.setText(f"{desc}")

    @staticmethod
    def get_emoji(weather_id):
        match id:
            case _ if 200 <= weather_id <=232:
                return "⛈️"

            case _ if 300 <= weather_id <=321:
                return "🌦️"

            case _ if 500 <= weather_id <=504:
                return "🌧️"

            case _ if 511 == weather_id:
                return "🌨️"

            case _ if 520 <= weather_id <=531:
                return "🌧️"

            case _ if 600 <= weather_id <=622:
                return "❄️"

            case _ if 700 <= weather_id <=761:
                return "🌫️"

            case _ if 762 == weather_id:
                return "🌋"

            case _ if 771 == weather_id:
                return "💨"

            case _ if 781 == weather_id:
                return "🌪️"

            case _ if 800 == weather_id:
                return "☀️"

            case _ if 801 <= weather_id <=804:
                return "🌥️"

            case _:
                return ""

if __name__ == "__main__":
    app = QApplication(sys.argv)
    weather = WeatherApp()
    weather.show()
    sys.exit(app.exec_())
