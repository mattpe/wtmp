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
1. Rename the files by changing the file extensions: _*.css -> *.scss_
1. Fix image file paths inside _.scss_ files
1. Add media queries to _scss_ files, example for a widescreen specific file:

    ```scss
    @media screen and (min-width: 1100px) {
      nav, header, main {
          max-width: 1100px;
          margin: auto;
      }
    }
    ```

1. Import sass styles to your app in _index.js_, e.g: `import './styles/style.scss';`
1. Remove `<link rel="stylesheet"..... />` elements from _index.html_
1. Try it out: `npm start`

[webpack documentation for sass-loader](https://webpack.js.org/loaders/sass-loader/)

## Fetching HSL data

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
          "docs": "jsdoc <command-line-arguments>",
          ...
        },
     ```

1. Use [command-line arguments](https://jsdoc.app/about-commandline.html) or create [a conf file](https://jsdoc.app/about-configuring-jsdoc.html) for configuration
1. `npm run docs`

Or you can add JSDoc to your webpack build chain with [a plugin](https://www.npmjs.com/package/jsdoc-webpack-plugin).

## Routing/navigation

Simple SPA model will do:

- All static html in _index.html_
- Use e.g. `<section>` elements to separate content pages/views
- Hide/display views by using JavaScript for navigation

## Some open data & API ideas

- [ProgrammableWeb API Directory](https://www.programmableweb.com/category/all/apis)
- [Avoindata.fi](https://www.avoindata.fi/en)
- [HSL open data](https://www.hsl.fi/en/hsl/open-data)
- [THL open data](https://thl.fi/fi/tilastot-ja-data/aineistot-ja-palvelut/avoin-data)
- [Avoin data Metropoliassa](https://wiki.metropolia.fi/display/opendata/REST-rajapinnat)

## Other

- In case of having problems with autoreload or loading/seeing code changes in local development enviroment:
  - Check that service worker is not caching the data -> disable SW registeration temporarily for development in your code
  - Clear site data & cache (Chrome dev tools: _Application_ tab -> Choose _Storage_ on the side panel -> Press _Clear site data_ button -> refresh the page)
