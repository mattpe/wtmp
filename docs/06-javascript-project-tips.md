# JavaScript Tips for the Project Application

## Adding [SASS](https://sass-lang.com/) to webpack project

1. Install packages: `npm install --save-dev node-sass style-loader css-loader sass-loader@^10.1.1`
1. Add config to `webpack.common.js`:

    ```js
    module: {
      rules: [
        ...
        {
          test: /\.s[ac]ss$/i,
          use: [
            // Creates `style` nodes from JS strings
            'style-loader',
            // Translates CSS into CommonJS
            'css-loader',
            // Compiles Sass to CSS
            'sass-loader',
          ],
        },
      ...
      ]}
    ```

1. Create folder `src/styles` and move your css files there
1. Rename files _*.css -> *.scss_
1. Fix image paths in _.scss_ files
1. Add media queries to _scss_ files, example for widescreen file:

    ```scss
    @media screen and (min-width: 1100px) {
      nav, header, main {
          max-width: 1100px;
          margin: auto;
      }
    }
    ```

1. Import styles in _index.js_, e.g: `import './styles/style.scss';`
1. Remove `<link rel="stylesheet"..... />` elements from _index.html_
1. `npm start`

[webpack documentation for sass-loader](https://webpack.js.org/loaders/sass-loader/)

## Fethcing HSL DATA

[Digitransit Routing API](https://digitransit.fi/en/developers/apis/1-routing-api/)

Some examples

```js
const apiUrl = 'https://api.digitransit.fi/routing/v1/routers/hsl/index/graphql';

// a query with hardcoded stop name
const queryDataByName = `{
    stops(name: "myyrmäki") {
      name
      locationType
      lat
      lon
      patterns {
        id
        name
        route {
          gtfsId
          shortName
          longName
        }
        directionId
      }
    }
  }
}`;

// a sample query with coordinates object
const getQueryForStopsByLocation = (location) => {
  return `{
    stopsByRadius(lat: ${location.lat}, lon: ${location.lon}, radius: 600, first: 10) {
      edges {
        node {
          stop {
            gtfsId
            name
            lat
            lon
            stoptimesWithoutPatterns {
              scheduledArrival
              realtimeArrival
              arrivalDelay
              scheduledDeparture
              realtimeDeparture
              departureDelay
              realtime
              realtimeState
              serviceDay
              headsign
              trip {
                tripHeadsign
                routeShortName
              }
            }
          }
          distance
        }
        cursor
      }
      pageInfo {
          hasNextPage
          endCursor
      }
    }
  }`;
};

/**
 * https://digitransit.fi/en/developers/apis/1-routing-api/stops/#query-scheduled-departure-and-arrival-times-of-a-stop
 * @param {number} id - id number of the hsl stop
 */
const getQueryForNextRidesByStopId = (id) => {
  return `{
    stop(id: "HSL:${id}") {
      name
      stoptimesWithoutPatterns {
        scheduledArrival
        realtimeArrival
        arrivalDelay
        scheduledDeparture
        realtimeDeparture
        departureDelay
        realtime
        realtimeState
        serviceDay
        headsign
        trip {
          routeShortName
          tripHeadsign
        }
      }
    }
  }`;
};

// Fetch example
const response = await fetch(apiUrl, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/graphql'
    },
    body: queryDataByName
  });
```

## Using [JSDoc](https://jsdoc.app/)

1. Install: `npm i --save-dev jsdoc`
1. Add jsdoc command to your scripts in `package.json`:

     ```json
       "scripts": {
          ...
          "doc": "jsdoc",
          ...
        },
     ```

1. Create a settings file (optional)
1. `npm run doc [LIST_JS_FILE_PATHS_HERE]`

## Routing/navigation

Simple SPA model will do.

## Some open data & API ideas

- [ProgrammableWeb API Directory](https://www.programmableweb.com/category/all/apis)
- [Avoindata.fi](https://www.avoindata.fi/en)
- [HSL open data](https://www.hsl.fi/en/hsl/open-data)
- [THL open data](https://thl.fi/fi/tilastot-ja-data/aineistot-ja-palvelut/avoin-data)
- [Avoin data Metropoliassa](https://wiki.metropolia.fi/display/opendata/REST-rajapinnat)