# COVID Alarm Clock

The purpose of the COVID Alarm Clock is to allow the user to schedule alarms that will give them updates on the news and on the weather. The application will also give the user automatic notifications, with a particular focus on COVID-19. It uses the UK's official COVID-19 API to give the user updates similar to what might be said in a daily press briefing. 

The application aims to keep the user informed about COVID-19, because having people better informed about the current state of COVID-19 in the UK will help them make informed decisions about their own risk management.

This application is designed for both users who want an alarm clock to keep them informed, as well as developers who would like to use or extend the project's functionality.

## Installation

Use the package manager [pip](https://pip.pypa.io/en/stable/) to install the covid alarm clock, alongside it's dependencies.

The program uses:
- pyttsx3
- Flask
- requests
- APScheduler
- uk-covid19
- python-dateutil

```bash
pip install pyttsx3
pip install Flask
pip install requests
pip install APScheduler
pip install uk-covid19
pip install python-dateutil
```

## Usage and Initial Configuration

Before attempting to use the alarm clock, you should take a look at the configuration file (config.json). You should sign up for an API key for the news API and weather API. Then you will need to paste these API keys into the relevant section of the configuration file (explained in the 'Further Configuration' section).

To use the alarm clock, simply run the ``main.py`` module. This can be done on most systems by running ``python main.py`` from the terminal while in the root directory where the Python package is located (rather than the module). If you have Python2 and Python3, you should run ``python3 main.py`` instead.

If you have successfully installed all of the application's prerequisite modules listed in the installation step above, the application should begin to run. It will run a local webserver which will act as the user interface for the application.

To access this interface, go to your web browser, and enter ``127.0.0.1:5000`` into the URL (search) bar. This should take you to the application's user interface. From there you can create new alarms by filling in the form and clicking 'submit'. All alarms will be listed on the left-hand-side of the site. You can also remove any alarms by clicking on the little 'x' on the top-right of the alarm. All notifications will be on the right-hand-side of the site. Notifications will pop up automatically.

## Further Configuration

### Preface
It is imperative that you set up your API keys, which is explained as part of the Usage section of this README file, but there is the opportunity for further configuration.

### Project configuraiton
```json
"app-name": "ECM1403 CA3 Smart Alarm",
"static-file-paths": {
    "favicon": "/static/images/pill.png",
    "image": "image.jpg"
}
```

- ``app-name`` refers to the name of the application that is listed on the web UI.
- ``static-file-paths`` includes the absolute path of the ``favicon``, starting from the directory where the main.py module is located, and the name of the ``image`` assuming the image is in the ``/static/images`` file path.

### Configuring external services

```json
"external-services": {
    "refresh-rate": 3600
}
```

- ``external-services`` is the list of external APIs that this application uses, and it includes ``news``, ``weather``, and ``covid``. Each of these have their own configuration that will be further explained.
- ``refresh-rate`` is how often (in seconds) the application will try to fetch the latest news, weather, and COVID data. It is recommended to keep this value at at least ``600`` seconds, since the APIs may block your API key if too many requests are sent. By default this is set to ``3600`` seconds, i.e. an hour. The APIs will, themselves, only update ever so often, (i.e. the news API updates hourly), so it is inadvisable to have a low refresh-rate time.

#### News
```json
"news": {
    "url": "https://newsapi.org/v2/top-headlines?",
    "api-key": "c8e82381ca4e4d7598c52e6aeecd3b21",
    "use-sources": "y",
    "sources": "bbc-news",
    "country": "gb",
    "category": "health",
    "search-term": "covid"
}
```
- ``url`` refers to the URL that the application will make a request to. It is very unlikely you will need to modify this.
- ``api-key`` refers to your own API key. You will need to sign up to [NewsAPI](https://newsapi.org) to obtain your own API key. Your key should be kept private and you should never share it, not even with other developers if you are requesting support!
- ``use-sources`` means whether the news API should choose to filter by sources. This should be either set to ``y`` for true, or any other value for false. Please note that if ``use-sources`` is ``y`` then it will use ``sources`` (such as BBC news), but it will not filter by ``country`` (such as GB), or by ``category`` (such as health). Any ``search-term`` provided will be used regardless of ``use-sources``. 
- ``sources`` refers to the sources that should be considered, such as ``bbc-news``.
- ``country`` refers to countries that should be considered, such as ``gb`` for Great Britain (United Kingdom).
- ``category`` refers to categories that should be considered, such as ``health``.
- ``search-term`` refers to a search term to filter articles by, such as ``covid`` to only get articles that include the term ``covid``. 

A list of valid sources can be found [here](https://newsapi.org). At least one source, country, or category must be provided. Search term is optional.

#### Weather
```json
"weather": {
    "url": "http://api.openweathermap.org/data/2.5/weather?",
    "api-key": "93a1eca7115f5a3a87a421e2e51bb925",
    "city": "Exeter",
    "units": "metric",
    "notify-automatically-for": {
        "thunderstorm": "y",
        "drizzle": "y",
        "rain": "y",
        "snow": "y",
        "atmosphere": "y",
        "clear": "y",
        "clouds": "y"
    }
}
```

- ``url`` refers to the URL that the application will make a request to. It is very unlikely you will need to modify this.
- ``api-key`` refers to your own API key. You will need to sign up to [OpenWeatherMap](https://openweathermap.org) to obtain your own API key. Your key should be kept private and you should never share it, not even with other developers if you are requesting support!
- ``city`` refers to the city to get weather data for, such as ``Exeter``.
- ``units`` refers to the units of temperature measurement. Available options are: ``standard`` (Kelvin), ``metric`` (Celcius), and ``imperial`` (Faranheit).
- ``notify-automatically-for`` is either yes (``y``) or no (``anything else``) for each of the options. This works such that when the application requests the weather from the API, if the weather has changed since last request, and the weather is a type of weather we care about (such as ``snow``), then it will produce a notification. Available weather types to notify for include: ``thunderstorm``, ``drizzle``, ``rain``, ``snow``, ``atmosphere`` (such as mist), ``clear``, and ``clouds``. 

#### Covid
```json
"covid": {
    "filter-by": "nation",
    "nation": "England",
    "region": "Exeter"
}
```

- ``filter-by`` refers to what to filter the COVID data by. Available options are: ``overview``, ``nation``, and ``region``.
    - ``overview`` will give an overview of the whole UK, but since only England counts deaths, any death data will not be available.
    - ``nation`` will give an overview of the provided nation, and if that nation counts deaths, death data will be available.
    - ``region`` will give an overview of the provided region, and if that region is in a nation that counts deaths, death data will be available.
- ``nation`` refers to the nation to filter by. Available options are: ``England``, ``Wales``, ``Scotland``, and ``Northern Ireland``.
- ``region`` refers to the region of the UK. For example, ``South West`` refers to the area that covers Devon (alongside other counties).

## Common Pitfalls

### Configuration

- When making adjustments to the configuration file, ``config.json``, a common mistake is making an error with regards to the syntax of the file. Make sure that after editing the config file that it can successfully parse an online JSON parser, such as [this one](http://json.parser.online.fr/).

- When making adjustments to the images, it is common to forget to include the file extension, or be using the wrong file extension. If the file is called ``image.png``, then having ``"image": "image"`` or ``"image": "image.jpg"`` will mean that the application is unable to access (and therefore render) the image.

- When making adjustments to the images, it is common to forget that, as stated in the Usage section of this README, that the ``favicon`` requires an absolute file path starting from the directory where the main.py module is located, whereas the ``image`` assumes that there are directories ``/static/images`` in the directory where the main.py module is located. 

### External Libraries

- There is no guarantee of reliability in terms of service or uptime from any external library that this application uses. If an external library is down, there will be information in the LOG file.

### Module Imports

- This application uses the modules listed in the Installation stage of this README. It is possible that you were unable to successfully install these modules, which would cause errors with running this application. It is recommended to read the pip installation instructions for your OS for any module you are having difficulty installing. You should contact the developer of the specific module(s) that you are having difficulty installing for further support as we are not responsible for the development, maintenance, or support of these modules.

## Extending or Contributing

It is possible to extend this project for your own use. This project is covered by the license located in the ``LICENSE.md`` file. 