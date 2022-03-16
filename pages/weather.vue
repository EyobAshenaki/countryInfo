<template>
  <!-- #TODO: - add an option to for the user to choose which unit of measurment like 
      #todo     METRIC, IMPERICAL AND DEFAULT and languages to use -doclink "https://openweathermap.org/current#geo" -->
  <v-row justify="center">
    <!-- Search Section -->
    <v-col cols="8" class="d-flex justify-center">
      <v-text-field
        v-model="search"
        name="search"
        label="Search"
        append-icon="mdi-magnify"
        style="max-width: 70%"
        @keypress.enter="showWeatherReport"
      ></v-text-field>
    </v-col>
    <v-col cols="12" class="pa-0">
      <v-card v-if="!!city" class="mx-auto pr-2" max-width="750">
        <v-card-text class="pa-0">
          <v-row class="pa-0 ma-0">
            <!-- image part -->
            <v-col cols="6" class="pa-0">
              <v-card height="100%" style="border-radius: 0">
                <nuxt-blur-image
                  v-if="searchedCityImage"
                  style="height: 100%"
                  :hash="searchedCityImage.blur_hash"
                  :src="searchedCityImage.urls.regular"
                  :alt="searchedCityImage.description"
                />
                <v-overlay absolute>
                  <v-card-text
                    v-if="!!city.weather.temperature"
                    class="pb-0"
                    style="margin-top: -13em"
                  >
                    <!-- #TODO : Change the image based on the weather condition -doclink "https://openweathermap.org/weather-conditions" -->
                    <!-- icon link http://openweathermap.org/img/wn/{10d}@2x.png-->
                    <v-row align="center">
                      <v-col class="text-h2" cols="9">
                        {{ `${city.weather.temperature.actual}&deg;C` }}
                        <!-- <v-row class="ma-0 pa-0">
                          <v-col cols="6" class="ma-0 pa-0">{{
                            `${city.weather.temperature.actual}`
                          }}</v-col>
                          <v-col
                            cols="4"
                            class="ma-0 pa-0"
                            @mouseenter="changeCelsiusPicToGif"
                            @mouseleave="changeCelsiusGifToPic"
                          >
                            <img
                              v-if="!isCelsiusIconGif"
                              class="pa-0 ma-0"
                              src="../assets/img/celsius/celsius.png"
                              style="height: 1em; weidth: 1em"
                            />
                            <img
                              v-else
                              src="../assets/img/celsius/celsius.gif"
                              class="pa-0 ma-0"
                              style="height: 1em; weidth: 1em"
                            />
                          </v-col>
                        </v-row> -->
                      </v-col>
                      <!-- :src="`http://openweathermap.org/img/wn/${city.weather.summary.icon}@4x.png`" -->
                      <v-col
                        cols="3"
                        @mouseenter="changeWeatherPicToGif"
                        @mouseleave="changeWeatherGifToPic"
                      >
                        <img
                          v-if="!isWeatherIconGif"
                          class="pa-0 ma-0"
                          :src="weatherImage.icon"
                          style="height: 5em; weidth: 5em"
                        />
                        <img
                          v-else
                          :src="weatherImage.gif"
                          class="pa-0 ma-0"
                          style="height: 5em; weidth: 5em"
                        />
                      </v-col>
                    </v-row>
                  </v-card-text>

                  <v-list-item two-line class="pl-6">
                    <v-list-item-icon
                      class="mr-n1"
                      @mouseenter="changeLocationPicToGif"
                      @mouseleave="changeLocationGifToPic"
                    >
                      <img
                        v-if="!isLocationIconGif"
                        class="pa-0 ma-0 mb-5"
                        src="../assets/img/location/location.png"
                        style="height: 2em; weidth: 2em"
                      />
                      <img
                        v-else
                        src="../assets/img/location/location.gif"
                        class="pa-0 ma-0 mb-5"
                        style="height: 2em; weidth: 2em"
                      />
                    </v-list-item-icon>
                    <v-list-item-content class="pt-0 pl-4">
                      <v-list-item-title class="text-h5">
                        {{ `${city.name}, ${city.country}` }}
                      </v-list-item-title>
                      <v-list-item-subtitle
                        >{{ `${getTimeOfReport.reportDate}, ` }}
                        <span class="text-body-1 font-weight-bold">{{
                          getTimeOfReport.reportDescription
                        }}</span></v-list-item-subtitle
                      >
                    </v-list-item-content>
                  </v-list-item>
                </v-overlay>
              </v-card>
            </v-col>
            <!-- text part -->
            <v-col cols="6" class="d-flex align-center pa-0">
              <!-- #TODO : Change this into loop -->
              <v-row class="ma-0 py-2">
                <!-- Feels like card -->
                <v-col cols="6" class="d-flex align-stretch pa-3">
                  <v-card
                    class="d-flex align-center flex-grow-1 pa-0 elevation-7"
                  >
                    <v-card-text>
                      <v-row>
                        <v-col
                          cols="6"
                          class="pa-0 d-flex justify-center align-center py-3"
                          @mouseenter="changeFeelsLikePicToGif"
                          @mouseleave="changeFeelsLikeGifToPic"
                        >
                          <img
                            v-if="!isFeelsLikeIconGif"
                            class="png-format pa-0"
                            src="../assets/img/termometer/thermometer.png"
                          />
                          <img
                            v-else
                            class="gif-format pa-0 ma-0"
                            src="../assets/img/termometer/thermometer.gif"
                          />
                        </v-col>
                        <v-col
                          cols="6"
                          class="pa-0 d-flex justify-center align-center"
                        >
                          <section>
                            <v-card-subtitle class="pa-0">
                              {{ `Feels like` }}
                            </v-card-subtitle>
                            <v-card-subtitle class="pa-0">
                              {{ `${city.weather.temperature.feelsLike}&deg;` }}
                            </v-card-subtitle>
                          </section>
                        </v-col>
                      </v-row>
                    </v-card-text>
                  </v-card>
                </v-col>
                <!-- Temperture card -->
                <v-col cols="6" class="d-flex align-stretch pa-3">
                  <v-card
                    class="d-flex align-center flex-grow-1 pa-0 elevation-7"
                  >
                    <v-card-text>
                      <v-row>
                        <v-col
                          cols="6"
                          class="pa-0 d-flex justify-center align-center py-3"
                          @mouseenter="changeTemperaturePicToGif"
                          @mouseleave="changeTemperatureGifToPic"
                        >
                          <img
                            v-if="!isTemperatureIconGif"
                            class="png-format pa-0"
                            src="../assets/img/temperature/temperature.png"
                          />
                          <img
                            v-else
                            class="gif-format pa-0 ma-0"
                            src="../assets/img/temperature/temperature.gif"
                          />
                        </v-col>
                        <v-col
                          cols="6"
                          class="pa-0 d-flex justify-center align-center"
                        >
                          <section>
                            <v-card-subtitle class="pa-0">
                              {{ `Max | Min` }}
                            </v-card-subtitle>
                            <v-card-subtitle class="pa-0">
                              {{
                                `${city.weather.temperature.max}&deg; | ${city.weather.temperature.max}&deg;`
                              }}
                            </v-card-subtitle>
                          </section>
                        </v-col>
                      </v-row>
                    </v-card-text>
                  </v-card>
                </v-col>
                <!-- Humidity card -->
                <v-col cols="6" class="d-flex align-stretch pa-3">
                  <v-card
                    class="d-flex align-center flex-grow-1 pa-0 elevation-7"
                  >
                    <v-card-text class="">
                      <v-row>
                        <v-col
                          cols="6"
                          class="pa-0 d-flex justify-center align-center py-3"
                          @mouseenter="changeHumidityPicToGif"
                          @mouseleave="changeHumidityGifToPic"
                        >
                          <img
                            v-if="!isHumidityIconGif"
                            class="png-format pa-0"
                            src="../assets/img/drop/drop.png"
                          />
                          <img
                            v-else
                            class="gif-format pa-0 ma-0"
                            src="../assets/img/drop/drop.gif"
                          />
                        </v-col>
                        <v-col
                          cols="6"
                          class="pa-0 d-flex justify-center align-center"
                        >
                          <section>
                            <v-card-subtitle class="pa-0">
                              {{ `Humidity` }}
                            </v-card-subtitle>
                            <v-card-subtitle class="pa-0">
                              {{ `${city.weather.clouds.humidity}%` }}
                            </v-card-subtitle>
                          </section>
                        </v-col>
                      </v-row>
                    </v-card-text>
                  </v-card>
                </v-col>
                <!-- Cloud cover card -->
                <v-col cols="6" class="d-flex align-stretch pa-3">
                  <v-card
                    class="d-flex align-center flex-grow-1 pa-0 elevation-7"
                  >
                    <v-card-text>
                      <v-row>
                        <v-col
                          cols="6"
                          class="pa-0 d-flex justify-center align-center py-3"
                          @mouseenter="changeCloudCoverPicToGif"
                          @mouseleave="changeCloudCoverGifToPic"
                        >
                          <img
                            v-if="!isCloudCoverIconGif"
                            class="png-format pa-0"
                            src="../assets/img/clouds/clouds.png"
                          />
                          <img
                            v-else
                            class="gif-format pa-0 ma-0"
                            src="../assets/img/clouds/clouds.gif"
                          />
                        </v-col>
                        <v-col
                          cols="6"
                          class="pa-0 d-flex justify-center align-center"
                        >
                          <section>
                            <v-card-subtitle class="pa-0">
                              {{ `Cloud cover` }}
                            </v-card-subtitle>
                            <v-card-subtitle class="pa-0">
                              {{ `${city.weather.clouds.all}%` }}
                            </v-card-subtitle>
                          </section>
                        </v-col>
                      </v-row>
                    </v-card-text>
                  </v-card>
                </v-col>
                <!-- Cloud visibility card -->
                <v-col cols="6" class="d-flex align-stretch pa-3">
                  <v-card
                    class="d-flex align-center flex-grow-1 pa-0 elevation-7"
                  >
                    <v-card-text>
                      <v-row>
                        <v-col
                          cols="6"
                          class="pa-0 d-flex justify-center align-center py-3"
                          @mouseenter="changeVisibilityPicToGif"
                          @mouseleave="changeVisibilityGifToPic"
                        >
                          <img
                            v-if="!isVisibilityIconGif"
                            class="png-format pa-0"
                            src="../assets/img/scanning/scanning.png"
                          />
                          <img
                            v-else
                            class="gif-format pa-0 ma-0"
                            src="../assets/img/scanning/scanning.gif"
                          />
                        </v-col>
                        <v-col
                          cols="6"
                          class="pa-0 d-flex justify-center align-center"
                        >
                          <section>
                            <v-card-subtitle class="pa-0">
                              {{ `Visibility` }}
                            </v-card-subtitle>
                            <v-card-subtitle class="pa-0">
                              {{ `${city.weather.clouds.visibility} km` }}
                            </v-card-subtitle>
                          </section>
                        </v-col>
                      </v-row>
                    </v-card-text>
                  </v-card>
                </v-col>
                <!-- Wind speed card -->
                <v-col cols="6" class="d-flex align-stretch pa-3">
                  <v-card
                    class="d-flex align-center flex-grow-1 pa-0 elevation-7"
                  >
                    <v-card-text>
                      <v-row>
                        <v-col
                          cols="6"
                          class="pa-0 d-flex justify-center align-center py-3"
                          @mouseenter="changeWindSpeedPicToGif"
                          @mouseleave="changeWindSpeedGifToPic"
                        >
                          <img
                            v-if="!isWindSpeedIconGif"
                            class="png-format pa-0"
                            src="../assets/img/wind/wind.png"
                          />
                          <img
                            v-else
                            class="gif-format pa-0 ma-0"
                            src="../assets/img/wind/wind.gif"
                          />
                        </v-col>
                        <v-col
                          cols="6"
                          class="pa-0 d-flex justify-center align-center"
                        >
                          <section>
                            <v-card-subtitle class="pa-0">
                              {{ `Wind speed` }}
                            </v-card-subtitle>
                            <v-card-subtitle class="pa-0">
                              {{ `${city.weather.wind.speed}m/s` }}
                            </v-card-subtitle>
                          </section>
                        </v-col>
                      </v-row>
                    </v-card-text>
                  </v-card>
                </v-col>
                <!-- Wind direction card -->
                <v-col cols="6" class="d-flex align-stretch pa-3">
                  <v-card
                    class="d-flex align-center flex-grow-1 pa-0 elevation-7"
                  >
                    <v-card-text>
                      <v-row>
                        <v-col
                          cols="4"
                          class="pa-0 d-flex justify-center align-center py-3"
                          @mouseenter="changeWindDirectionPicToGif"
                          @mouseleave="changeWindDirectionGifToPic"
                        >
                          <img
                            v-if="!isWindDirectionIconGif"
                            class="png-format pa-0"
                            src="../assets/img/weathercock/weathercock.png"
                          />
                          <img
                            v-else
                            class="gif-format pa-0 ma-0"
                            src="../assets/img/weathercock/weathercock.gif"
                          />
                        </v-col>
                        <v-col
                          cols="8"
                          class="pa-0 d-flex justify-center align-center"
                        >
                          <section>
                            <v-card-subtitle class="pa-0">
                              {{ `Wind direction` }}
                            </v-card-subtitle>
                            <v-card-subtitle class="pa-0">
                              {{ `${city.weather.wind.deg}&deg;` }}
                            </v-card-subtitle>
                          </section>
                        </v-col>
                      </v-row>
                    </v-card-text>
                  </v-card>
                </v-col>
              </v-row>
            </v-col>
          </v-row>
        </v-card-text>

        <!-- <v-divider></v-divider>

        <v-card-actions>
          <v-btn text> Full Report </v-btn>
        </v-card-actions> -->
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
export default {
  name: 'InspirePage',
  data() {
    return {
      showSearchMenu: false,
      search: null,
      isWeatherIconGif: false,
      isCelsiusIconGif: false,
      isLocationIconGif: false,
      isTemperatureIconGif: false,
      isFeelsLikeIconGif: false,
      isHumidityIconGif: false,
      isCloudCoverIconGif: false,
      isVisibilityIconGif: false,
      isWindSpeedIconGif: false,
      isWindDirectionIconGif: false,
      city: null,
      searchedCityImage: null,
    }
  },
  computed: {
    searchResult({ search }) {
      return search ? search.toLowerCase() : null
    },
    getTimeOfReport({ city }) {
      const timeInSeconds = city.weather.timestamp
      const reportDate = new Date(timeInSeconds).toDateString()
      const reportTime = new Date(timeInSeconds).toTimeString()

      const dateParts = reportDate.split(' ')
      const timeParts = reportTime.split(' ')[0].split(':')

      const modifiedReportDate = `${dateParts[0]}, ${dateParts[1]} ${dateParts[2]}, ${timeParts[0]}:${timeParts[1]}`

      const descriptionParts = city.weather.summary.description.split(' ')

      let modifiedDescription = ''
      for (const descriptionPart of descriptionParts) {
        modifiedDescription += `${descriptionPart
          .slice(0, 1)
          .toUpperCase()
          .concat(descriptionPart.slice(1))} `
      }

      // return `${modifiedReportDate} ${modifiedDescription}`
      return {
        reportDate: modifiedReportDate,
        reportDescription: modifiedDescription,
      }
    },
    weatherImage({ city }) {
      const fetchedIcon = city.weather.summary.icon
      let weatherIconSrc = ''
      let weatherGifSrc = ''
      if (fetchedIcon === '01d') {
        weatherIconSrc = require('../assets/img/sun/sun.png')
        weatherGifSrc = require('../assets/img/sun/sun.gif')
      } else if (fetchedIcon === '01n') {
        weatherIconSrc = require('../assets/img/night/night.png')
        weatherGifSrc = require('../assets/img/night/night.gif')
      } else if (fetchedIcon === '02d') {
        weatherIconSrc = require('../assets/img/cloudy/cloudy.png')
        weatherGifSrc = require('../assets/img/cloudy/cloudy.gif')
      } else if (fetchedIcon === '02n') {
        weatherIconSrc = require('../assets/img/cloudy-night/cloudy-night.png')
        weatherGifSrc = require('../assets/img/cloudy-night/cloudy-night.gif')
      } else if (fetchedIcon === '03d' || fetchedIcon === '03n') {
        weatherIconSrc = require('../assets/img/clouds/clouds.png')
        weatherGifSrc = require('../assets/img/clouds/clouds.gif')
      } else if (fetchedIcon === '04d' || fetchedIcon === '04n') {
        weatherIconSrc = require('../assets/img/clouds/clouds.png')
        weatherGifSrc = require('../assets/img/clouds/clouds.gif')
      } else if (fetchedIcon === '09d' || fetchedIcon === '09n') {
        weatherIconSrc = require('../assets/img/drizzle/drizzle.png')
        weatherGifSrc = require('../assets/img/drizzle/drizzle.gif')
      } else if (fetchedIcon === '10d' || fetchedIcon === '10n') {
        weatherIconSrc = require('../assets/img/rain/rain.png')
        weatherGifSrc = require('../assets/img/rain/rain.gif')
      } else if (fetchedIcon === '11d' || fetchedIcon === '11n') {
        weatherIconSrc = require('../assets/img/storm/storm.png')
        weatherGifSrc = require('../assets/img/storm/storm.gif')
      } else if (fetchedIcon === '13d' || fetchedIcon === '13n') {
        weatherIconSrc = require('../assets/img/snow/snow.png')
        weatherGifSrc = require('../assets/img/snow/snow.gif')
      } else if (fetchedIcon === '50d' || fetchedIcon === '50n') {
        weatherIconSrc = require('../assets/img/foggy/foggy.png')
        weatherGifSrc = require('../assets/img/foggy/foggy.gif')
      }

      return { icon: weatherIconSrc, gif: weatherGifSrc }
    },
  },
  methods: {
    getCityByName(name) {
      const query = `query getCityByName($name: String!, $config: ConfigInput) {
                      getCityByName(name: $name, config: $config) {
                        id
                        name
                        country
                        coord {
                          lon
                          lat
                        }
                        weather {
                          summary {
                            title
                            description
                            icon
                          }
                          temperature {
                            actual
                            feelsLike
                            min
                            max
                          }
                          wind {
                            speed
                            deg
                          }
                          clouds {
                            all
                            visibility
                            humidity
                          }
                          timestamp
                        }
                      }
                    }`
      const config = {
        units: 'metric',
        lang: 'en',
      }
      const variables = { name, config }
      return fetch('https://graphql-weather-api.herokuapp.com/', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          Accept: 'application/json',
        },
        body: JSON.stringify({
          query,
          variables,
        }),
      })
        .then((res) => res.json())
        .then((data) => data.data.getCityByName)
        .catch((err) => console.warn(err))
    },

    async showWeatherReport() {
      this.city = await this.getCityByName(this.search)
      this.getCityImage(this.city.name)
    },

    changeWeatherPicToGif() {
      this.isWeatherIconGif = true
    },
    changeWeatherGifToPic() {
      this.isWeatherIconGif = false
    },

    changeCelsiusPicToGif() {
      this.isCelsiusIconGif = true
    },
    changeCelsiusGifToPic() {
      this.isCelsiusIconGif = false
    },

    changeLocationPicToGif() {
      this.isLocationIconGif = true
    },
    changeLocationGifToPic() {
      this.isLocationIconGif = false
    },

    changeTemperaturePicToGif() {
      this.isTemperatureIconGif = true
    },
    changeTemperatureGifToPic() {
      this.isTemperatureIconGif = false
    },

    changeFeelsLikePicToGif() {
      this.isFeelsLikeIconGif = true
    },
    changeFeelsLikeGifToPic() {
      this.isFeelsLikeIconGif = false
    },

    changeHumidityPicToGif() {
      this.isHumidityIconGif = true
    },
    changeHumidityGifToPic() {
      this.isHumidityIconGif = false
    },

    changeCloudCoverPicToGif() {
      this.isCloudCoverIconGif = true
    },
    changeCloudCoverGifToPic() {
      this.isCloudCoverIconGif = false
    },

    changeVisibilityPicToGif() {
      this.isVisibilityIconGif = true
    },
    changeVisibilityGifToPic() {
      this.isVisibilityIconGif = false
    },

    changeWindSpeedPicToGif() {
      this.isWindSpeedIconGif = true
    },
    changeWindSpeedGifToPic() {
      this.isWindSpeedIconGif = false
    },

    changeWindDirectionPicToGif() {
      this.isWindDirectionIconGif = true
    },
    changeWindDirectionGifToPic() {
      this.isWindDirectionIconGif = false
    },

    async getCityImage(cityName) {
      const clientId = 'h-O6de8uDhKASCQmGZ_rkdLfN0ry8SqztvyPKdJc5ps'
      const query = cityName
      const url = `https://api.unsplash.com/search/photos/?client_id=${clientId}&query=${query}`

      const images = await fetch(url, {
        method: 'GET',
        headers: {
          headers: {
            'Content-Type': 'application/json',
            Accept: 'application/json',
            'Accept-Version': 'v1',
          },
        },
      })
        .then((res) => res.json())
        .then((data) => data.results)
        .catch((err) => console.warn(err))

      this.searchedCityImage = images[0]
    },
  },
}
</script>

<style scoped>
.blured-image {
  opacity: 0.5;
}

.png-format {
  max-height: 3em;
  max-width: 3em;
}

.gif-format {
  max-height: 3em;
  max-width: 3em;
}
</style>
